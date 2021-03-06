---
title: Encabezados de contexto
author: rick-anderson
description: "Este documento describen los detalles de implementación de los encabezados de contexto de protección de datos de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: c047c54efdcdb6192e4d38d2822c1077ee0a73e1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="context-headers"></a>Encabezados de contexto

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>Segundo plano y la teoría

En el sistema de protección de datos, una "clave" significa autenticado de un objeto que puede proporcionar servicios de cifrado. Cada clave está identificada por un identificador único (GUID) y lleva con él algorítmica información y al material entropic. Se pretende que cada clave llevar entropía único, pero el sistema no puede exigir y también es necesario tener en cuenta para los desarrolladores que cambiaría el anillo de clave manualmente mediante la modificación de la información de una clave existente en el anillo de clave algorítmica. Para lograr los requisitos de seguridad tiene estos casos, el sistema de protección de datos tiene un concepto de [agilidad criptográfica](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), que permite de forma segura mediante un único valor entropic entre varios algoritmos criptográficos.

Mayoría de los sistemas que son compatibles con agilidad criptográfica hacerlo mediante la inclusión de cierta información de identificación sobre el algoritmo en la carga. OID del algoritmo suele ser un buen candidato para esto. Sin embargo, un problema que encontramos es que hay varias maneras de especificar el mismo algoritmo: "AES" (CNG) y los administrados Aes, AesManaged, AesCryptoServiceProvider, AesCng y RijndaelManaged (determinados parámetros específicos) clases todo realmente son las mismas lo y se tendría que mantener una asignación de todos estos para el OID correcto. Si un desarrollador desea proporcionar un algoritmo personalizado (o incluso otra implementación de AES), tendría que Díganos su OID. Este paso de registro adicional, la configuración de sistema es especialmente muy complicada.

Ejecución paso a paso atrás, decidimos que estábamos se está aproximando al problema de la dirección equivocada. Un OID indica cuál es el algoritmo, pero se no realmente le interesa esto. Si se necesita usar un único valor entropic de forma segura en los dos algoritmos diferentes, no es necesario para que podamos saber cuáles son en realidad los algoritmos. ¿Qué nos realmente importa es su comportamiento. Cualquier algoritmo de cifrado de bloques simétrico decente también es una permutación pseudoaleatoria segura (PRP): corrija las entradas (clave, el encadenamiento de texto simple de modo, IV) y la salida de texto cifrado con una sobrecarga probabilidad será distinta de cualquier otro cifrado por bloques simétrico algoritmo dada las entradas de la mismas. Del mismo modo, cualquier función de hash con clave decente también es una función pseudoaleatoria segura (PRF), y debido a un conjunto de entrada fijo su salida muy será distinta de cualquier otra función de hash con clave.

Este concepto de PRPs y PRFs seguros se usa para crear un encabezado de contexto. Este encabezado de contexto actúa esencialmente como una huella digital estable sobre los algoritmos en uso para una operación determinada, así como la agilidad criptográfica necesaria para el sistema de protección de datos. Este encabezado es "reproducible" y se utiliza posteriormente como parte de la [proceso de derivación de la subclave](subkeyderivation.md#data-protection-implementation-subkey-derivation). Hay dos maneras diferentes para generar el encabezado de contexto de función de los modos de funcionamiento de los algoritmos subyacentes.

## <a name="cbc-mode-encryption--hmac-authentication"></a>Cifrado del modo CBC + autenticación HMAC

<a name="data-protection-implementation-context-headers-cbc-components"></a>

El encabezado de contexto está formada por los siguientes componentes:

* [16 bits] El valor 00 00, que es un marcador de lo que significa "cifrado CBC + autenticación HMAC".

* [32 bits] La longitud de clave (en bytes, big-endian) del algoritmo de cifrado de bloques simétrico.

* [32 bits] El tamaño de bloque (en bytes, big-endian) del algoritmo de cifrado de bloques simétrico.

* [32 bits] La longitud de clave (en bytes, big-endian) del algoritmo HMAC. (Actualmente el tamaño de clave siempre coincide con el tamaño de texto implícita.)

* [32 bits] El tamaño de texto implícita (en bytes, big-endian) del algoritmo HMAC.

* EncCBC (K_E, IV, ""), que es el resultado del algoritmo de cifrado de bloques simétrico dado una entrada de cadena vacía y donde IV es un vector de ceros. La construcción de K_E se describe a continuación.

* MAC (K_H, ""), que es el resultado del algoritmo HMAC dado una entrada de cadena vacía. La construcción de K_H se describe a continuación.

Lo ideal es que, podríamos pasamos vectores de ceros para K_E y K_H. Sin embargo, debe evitar la situación donde el algoritmo subyacente comprueba la existencia de claves débiles antes de realizar cualquier operación (especialmente DES y 3DES), lo que impide utilizar un modelo simple o repeatable como un vector de ceros.

En su lugar, usamos NIST SP800-108 KDF en modo de contador (vea [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) con una clave de longitud cero, etiqueta y contexto y HMACSHA512 como el PRF subyacente. Se derivan | K_E | + | K_H | bytes de salida, a continuación, descomponer el resultado en K_E y K_H por sí mismos. Matemáticamente, se representa como se indica a continuación.

( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>Ejemplo: AES-192-CBC + HMACSHA256

Por ejemplo, considere el caso donde el algoritmo de cifrado de bloques simétrico es AES-192-CBC y el algoritmo de validación es HMACSHA256. El sistema generaría el encabezado de contexto mediante los pasos siguientes.

En primer lugar, se permiten (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", etiqueta = "", contexto = ""), donde | K_E | = 192 bits y | K_H | = 256 bits por los algoritmos especificados. Esto conduce al K_E = 5BB6... 21DD y K_H = A04A... 00A9 en el ejemplo siguiente:

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

A continuación, calcular Enc_CBC (K_E, IV, "") de AES-192-CBC dado IV = 0 * y K_E como anteriormente.

resultado: = F474B1872B3B53E4721DE19C0841DB6F

A continuación, calcular MAC (K_H, "") para HMACSHA256 dado K_H como anteriormente.

result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

Esto produce el encabezado de contexto completo siguiente:

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

Este encabezado de contexto es la huella digital del par de algoritmo de cifrado autenticado (cifrado de AES-192-CBC + HMACSHA256 validación). Los componentes, como se describe [anteriormente](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) son:

* el marcador (00 00)

* la longitud de clave de cifrado de bloque (00 00 00 18)

* el tamaño de bloque de cifrado de bloque (00 00 00 10)

* la longitud de clave de HMAC (00 00 00 20)

* el tamaño de la síntesis HMAC (00 00 00 20)

* el cifrado por bloques salida PRP (F4 74 - DB 6F) y

* la salida de HMAC PRF (D4 79 - final).

> [!NOTE]
> El cifrado del modo CBC + HMAC encabezado de contexto de autenticación se basa en la misma forma, independientemente de si se proporcionan las implementaciones de algoritmos CNG de Windows o tipos administrados SymmetricAlgorithm y KeyedHashAlgorithm. Esto permite que aplicaciones que se ejecutan en sistemas operativos diferentes generar de forma confiable el mismo encabezado de contexto, aunque las implementaciones de los algoritmos difieren entre sistemas operativos. (En la práctica, la KeyedHashAlgorithm no tiene que ser un HMAC correcto. Puede ser cualquier tipo de algoritmo hash con clave.)

### <a name="example-3des-192-cbc--hmacsha1"></a>Ejemplo: 3DES-192-CBC + HMACSHA1

En primer lugar, se permiten (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", etiqueta = "", contexto = ""), donde | K_E | = 192 bits y | K_H | = 160 bits por los algoritmos especificados. Esto conduce al K_E = A219... E2BB y K_H = DC4A... B464 en el ejemplo siguiente:

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

A continuación, calcular Enc_CBC (K_E, IV, "") para 3DES-192-CBC dado IV = 0 * y K_E como anteriormente.

resultado: = ABB100F81E53E10E

A continuación, calcular MAC (K_H, "") para HMACSHA1 dado K_H como anteriormente.

result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

Esto genera el encabezado de contexto completo que es una huella digital de los autenticados par de algoritmo de cifrado (cifrado 3DES-192-CBC + validación HMACSHA1), se muestra a continuación:

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

Los componentes se dividen como sigue:

* el marcador (00 00)

* la longitud de clave de cifrado de bloque (00 00 00 18)

* el tamaño de bloque de cifrado de bloque (00 00 00 08)

* la longitud de clave de HMAC (00 00 00 14)

* el tamaño de la síntesis HMAC (00 00 00 14)

* el cifrado por bloques salida PRP (B1 AB - E1 0E) y

* la salida de HMAC PRF (76 EB - final).

## <a name="galoiscounter-mode-encryption--authentication"></a>El cifrado del modo de Galois/contador + autenticación

El encabezado de contexto está formada por los siguientes componentes:

* [16 bits] El valor 00 01, que es un marcador de lo que significa "cifrado de GCM + autenticación".

* [32 bits] La longitud de clave (en bytes, big-endian) del algoritmo de cifrado de bloques simétrico.

* [32 bits] El tamaño (en bytes, big-endian) nonce que usa durante las operaciones de cifrado autenticado. (En nuestro sistema, esto se fija en tamaño nonce = 96 bits.)

* [32 bits] El tamaño de bloque (en bytes, big-endian) del algoritmo de cifrado de bloques simétrico. (Para GCM, esto se fija en el tamaño de bloque = 128 bits.)

* [32 bits] La autenticación etiqueta tamaño (en bytes, big-endian) creado por la función de cifrado autenticado. (En nuestro sistema, esto se fija en el tamaño de la etiqueta = 128 bits.)

* [128 bits] La etiqueta de Enc_GCM (K_E, nonce, ""), que es el resultado del algoritmo de cifrado de bloques simétrico dado una entrada de cadena vacía y donde nonce es un vector de ceros de 96 bits.

K_E se deduce usando el mismo mecanismo como en el cifrado CBC + el escenario de autenticación de HMAC. Sin embargo, puesto que no hay ninguna K_H en play aquí, se suelen tener | K_H | = 0, y el algoritmo se contrae en el siguiente formulario.

K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")

### <a name="example-aes-256-gcm"></a>Ejemplo: AES-256-GCM

First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.

K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

A continuación, calcular la etiqueta de autenticación de Enc_GCM (K_E, nonce, "") de AES-256-GCM dado nonce = 096 y K_E como anteriormente.

result := E7DCCE66DF855A323A6BB7BD7A59BE45

Esto produce el encabezado de contexto completo siguiente:

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

Los componentes se dividen como sigue:

   * el marcador (00 01)

   * la longitud de clave de cifrado de bloque (00 00 00 20)

   * el tamaño del valor de seguridad (00 00 00 0c)

   * el tamaño de bloque de cifrado de bloque (00 00 00 10)

   * el tamaño de la etiqueta de autenticación (00 00 00 10) y

   * la etiqueta de autenticación el cifrado de bloques de ejecución (controlador de dominio E7 - final).
