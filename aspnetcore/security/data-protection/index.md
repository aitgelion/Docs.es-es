---
title: "Protección de datos en ASP.NET Core"
author: rick-anderson
description: "Este documento sirve como tabla de contenido para los distintos temas de protección de datos de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: b846fb7cb28eeceb8c0bdb47135e1cf014ae08a7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>Protección de datos en ASP.NET Core: API de consumidor, configuración, API de extensibilidad e implementación

* [Introducción a la protección de datos](introduction.md)

* [Introducción a las API de protección de datos](using-data-protection.md)

* [API de consumidor](consumer-apis/index.md)

  * [Información general sobre las API de consumidor](consumer-apis/overview.md)

  * [Cadenas de propósito](consumer-apis/purpose-strings.md)

  * [Jerarquía de propósito y configuración multiempresa](consumer-apis/purpose-strings-multitenancy.md)

  * [Hash de contraseña](consumer-apis/password-hashing.md)

  * [Limitación de la duración de cargas protegidas](consumer-apis/limited-lifetime-payloads.md)

  * [Desprotección de cargas cuyas claves se han revocado](consumer-apis/dangerous-unprotect.md)

* [Configuración](configuration/index.md)

  * [Configuración de la protección de datos](configuration/overview.md)

  * [Configuración predeterminada](configuration/default-settings.md)

  * [Directiva de todo el equipo](configuration/machine-wide-policy.md)

  * [Escenarios no compatibles con DI](configuration/non-di-scenarios.md)

* [API de extensibilidad](extensibility/index.md)

  * [Extensibilidad de criptografía de núcleo](extensibility/core-crypto.md)

  * [Extensibilidad de administración de claves](extensibility/key-management.md)

  * [Otras API](extensibility/misc-apis.md)

* [Implementación](implementation/index.md)

  * [Detalles de cifrado autenticado](implementation/authenticated-encryption-details.md)

  * [Derivación de subclave y cifrado autenticado](implementation/subkeyderivation.md)

  * [Encabezados de contexto](implementation/context-headers.md)

  * [Administración de claves](implementation/key-management.md)

  * [Proveedores de almacenamiento de claves](implementation/key-storage-providers.md)

  * [Cifrado de claves en reposo](implementation/key-encryption-at-rest.md)

  * [Inmutabilidad de claves y cambio de configuración](implementation/key-immutability.md)

  * [Formato de almacenamiento de claves](implementation/key-storage-format.md)

  * [Proveedores de protección de datos efímeros](implementation/key-storage-ephemeral.md)

* [Compatibilidad](compatibility/index.md)

  * [Uso compartido de cookies entre aplicaciones](xref:security/data-protection/compatibility/cookie-sharing)

  * [Reemplazar <machineKey> en ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
