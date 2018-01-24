---
title: "Administración de claves"
author: rick-anderson
description: "Este documento describen los detalles de implementación de la administración de claves de protección las API de datos principal de ASP.NET."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 53adb067751917a9539a310bb7d91e599696f213
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="key-management"></a><span data-ttu-id="b15bb-103">Administración de claves</span><span class="sxs-lookup"><span data-stu-id="b15bb-103">Key Management</span></span>

<a name="data-protection-implementation-key-management"></a>

<span data-ttu-id="b15bb-104">El sistema de protección de datos administra automáticamente la duración de claves maestras de usa para proteger y desproteger cargas.</span><span class="sxs-lookup"><span data-stu-id="b15bb-104">The data protection system automatically manages the lifetime of master keys used to protect and unprotect payloads.</span></span> <span data-ttu-id="b15bb-105">Cada clave puede estar en uno de cuatro fases:</span><span class="sxs-lookup"><span data-stu-id="b15bb-105">Each key can exist in one of four stages:</span></span>

* <span data-ttu-id="b15bb-106">Creado: la clave existe en el anillo de clave, pero aún no se ha activado.</span><span class="sxs-lookup"><span data-stu-id="b15bb-106">Created - the key exists in the key ring but has not yet been activated.</span></span> <span data-ttu-id="b15bb-107">La clave no debe utilizarse para nuevas operaciones de protección hasta que haya transcurrido el tiempo suficiente que la clave ha tenido la oportunidad de propagarse a todas las máquinas que consumen este anillo de clave.</span><span class="sxs-lookup"><span data-stu-id="b15bb-107">The key shouldn't be used for new Protect operations until sufficient time has elapsed that the key has had a chance to propagate to all machines that are consuming this key ring.</span></span>

* <span data-ttu-id="b15bb-108">Active - la clave existe en el anillo de clave y debe utilizarse para todas las operaciones de proteger de nuevo.</span><span class="sxs-lookup"><span data-stu-id="b15bb-108">Active - the key exists in the key ring and should be used for all new Protect operations.</span></span>

* <span data-ttu-id="b15bb-109">Ha caducado: la clave de su duración natural ha ejecutado y ya no debe usarse para nuevas operaciones de protección.</span><span class="sxs-lookup"><span data-stu-id="b15bb-109">Expired - the key has run its natural lifetime and should no longer be used for new Protect operations.</span></span>

* <span data-ttu-id="b15bb-110">Revocar - la clave está en peligro y no se debe utilizar para nuevas operaciones de protección.</span><span class="sxs-lookup"><span data-stu-id="b15bb-110">Revoked - the key is compromised and must not be used for new Protect operations.</span></span>

<span data-ttu-id="b15bb-111">Claves creadas, activas y caducadas pueden utilizarse para desproteger cargas entrantes.</span><span class="sxs-lookup"><span data-stu-id="b15bb-111">Created, active, and expired keys may all be used to unprotect incoming payloads.</span></span> <span data-ttu-id="b15bb-112">Claves revocadas de forma predeterminada no pueden usarse para desproteger cargas, pero el desarrollador de aplicaciones puede [invalidar este comportamiento](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect) si es necesario.</span><span class="sxs-lookup"><span data-stu-id="b15bb-112">Revoked keys by default may not be used to unprotect payloads, but the application developer can [override this behavior](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect) if necessary.</span></span>

>[!WARNING]
> <span data-ttu-id="b15bb-113">El programador podría verse tentado a eliminar una clave desde el anillo de clave (p. ej., eliminando el archivo correspondiente del sistema de archivos).</span><span class="sxs-lookup"><span data-stu-id="b15bb-113">The developer might be tempted to delete a key from the key ring (e.g., by deleting the corresponding file from the file system).</span></span> <span data-ttu-id="b15bb-114">En ese momento, todos los datos protegidos por la clave es indescifrables permanentemente, y no hay ninguna invalidación emergencia que hay con claves revocadas.</span><span class="sxs-lookup"><span data-stu-id="b15bb-114">At that point, all data protected by the key is permanently undecipherable, and there is no emergency override like there is with revoked keys.</span></span> <span data-ttu-id="b15bb-115">Si se elimina una clave es un comportamiento destructivo realmente y, por consiguiente, el sistema de protección de datos no expone ninguna API de primera clase para realizar esta operación.</span><span class="sxs-lookup"><span data-stu-id="b15bb-115">Deleting a key is truly destructive behavior, and consequently the data protection system exposes no first-class API for performing this operation.</span></span>

## <a name="default-key-selection"></a><span data-ttu-id="b15bb-116">Selección de clave predeterminada</span><span class="sxs-lookup"><span data-stu-id="b15bb-116">Default key selection</span></span>

<span data-ttu-id="b15bb-117">Cuando el sistema de protección de datos lee el anillo de clave desde el repositorio de respaldo, intentará encontrar una clave de "default" desde el anillo de clave.</span><span class="sxs-lookup"><span data-stu-id="b15bb-117">When the data protection system reads the key ring from the backing repository, it will attempt to locate a "default" key from the key ring.</span></span> <span data-ttu-id="b15bb-118">La clave predeterminada se usa para operaciones de proteger de nuevo.</span><span class="sxs-lookup"><span data-stu-id="b15bb-118">The default key is used for new Protect operations.</span></span>

<span data-ttu-id="b15bb-119">La heurística general es que el sistema de protección de datos elige la clave con la fecha de activación más reciente que la clave predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b15bb-119">The general heuristic is that the data protection system chooses the key with the most recent activation date as the default key.</span></span> <span data-ttu-id="b15bb-120">(No hay un factor de aglutinante pequeño para permitir el reloj del servidor a servidor sesgo). Si la clave expiró o se revocó, generación de claves y si la aplicación no ha deshabilitado automática, se generará una nueva clave con la activación inmediata por la [clave de expiración y las sucesivas](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) directiva siguiente.</span><span class="sxs-lookup"><span data-stu-id="b15bb-120">(There's a small fudge factor to allow for server-to-server clock skew.) If the key is expired or revoked, and if the application has not disabled automatic key generation, then a new key will be generated with immediate activation per the [key expiration and rolling](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) policy below.</span></span>

<span data-ttu-id="b15bb-121">El motivo por el sistema de protección de datos genera una nueva clave inmediatamente en lugar de usar una clave diferente es que la nueva generación de claves debe tratarse como una expiración implícita de todas las claves que se activaron antes de la nueva clave.</span><span class="sxs-lookup"><span data-stu-id="b15bb-121">The reason the data protection system generates a new key immediately rather than falling back to a different key is that new key generation should be treated as an implicit expiration of all keys that were activated prior to the new key.</span></span> <span data-ttu-id="b15bb-122">La idea general es que pueden haber sido configuradas nuevas claves con algoritmos diferentes o mecanismos de cifrado en el resto de las claves antiguas, y el sistema debe preferir al usar la configuración actual.</span><span class="sxs-lookup"><span data-stu-id="b15bb-122">The general idea is that new keys may have been configured with different algorithms or encryption-at-rest mechanisms than old keys, and the system should prefer the current configuration over falling back.</span></span>

<span data-ttu-id="b15bb-123">Hay una excepción.</span><span class="sxs-lookup"><span data-stu-id="b15bb-123">There is an exception.</span></span> <span data-ttu-id="b15bb-124">Si el desarrollador de aplicaciones tiene [deshabilita la generación automática de claves](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), a continuación, el sistema de protección de datos debe elegir algo como la clave predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b15bb-124">If the application developer has [disabled automatic key generation](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), then the data protection system must choose something as the default key.</span></span> <span data-ttu-id="b15bb-125">En este escenario de reserva, el sistema elegirá la clave no revocados con la fecha de activación más reciente, con preferencia otorgado a las claves que hayan tenido tiempo para propagar a otros equipos del clúster.</span><span class="sxs-lookup"><span data-stu-id="b15bb-125">In this fallback scenario, the system will choose the non-revoked key with the most recent activation date, with preference given to keys that have had time to propagate to other machines in the cluster.</span></span> <span data-ttu-id="b15bb-126">El sistema de reserva puede acabar elegir una clave predeterminada expiradas como resultado.</span><span class="sxs-lookup"><span data-stu-id="b15bb-126">The fallback system may end up choosing an expired default key as a result.</span></span> <span data-ttu-id="b15bb-127">El sistema de reserva no elegirá nunca una clave revocada como la clave predeterminada y si el anillo de clave está vacío o todas las claves se ha revocado el sistema generará un error en la inicialización.</span><span class="sxs-lookup"><span data-stu-id="b15bb-127">The fallback system will never choose a revoked key as the default key, and if the key ring is empty or every key has been revoked then the system will produce an error upon initialization.</span></span>

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a><span data-ttu-id="b15bb-128">Expiración de la clave y gradual</span><span class="sxs-lookup"><span data-stu-id="b15bb-128">Key expiration and rolling</span></span>

<span data-ttu-id="b15bb-129">Cuando se crea una clave, que se genera automáticamente una fecha de activación de {now + 2 días} y una fecha de expiración de {now + 90 días}.</span><span class="sxs-lookup"><span data-stu-id="b15bb-129">When a key is created, it is automatically given an activation date of { now + 2 days } and an expiration date of { now + 90 days }.</span></span> <span data-ttu-id="b15bb-130">El retraso de 2 días antes de la activación le ofrece la key time en propagarse a través del sistema.</span><span class="sxs-lookup"><span data-stu-id="b15bb-130">The 2-day delay before activation gives the key time to propagate through the system.</span></span> <span data-ttu-id="b15bb-131">Es decir, permite que otras aplicaciones que apunta a la memoria auxiliar observar la clave en el siguiente período de actualización automática, lo que maximiza las posibilidades de que cuando la clave de anillo activo hace que se convierten en que se propague a todas las aplicaciones que pueden necesitar para utilizan.</span><span class="sxs-lookup"><span data-stu-id="b15bb-131">That is, it allows other applications pointing at the backing store to observe the key at their next auto-refresh period, thus maximizing the chances that when the key ring does become active it has propagated to all applications that might need to use it.</span></span>

<span data-ttu-id="b15bb-132">Si la clave predeterminada expirará dentro de 2 días y el anillo de clave aún no tiene una clave que se activará tras la expiración de la clave de forma predeterminada, el sistema de protección de datos conservará automáticamente una nueva clave para el anillo de clave.</span><span class="sxs-lookup"><span data-stu-id="b15bb-132">If the default key will expire within 2 days and if the key ring does not already have a key that will be active upon expiration of the default key, then the data protection system will automatically persist a new key to the key ring.</span></span> <span data-ttu-id="b15bb-133">Esta nueva clave tiene una fecha de activación de {fecha de expiración de la clave predeterminada} y una fecha de expiración de {now + 90 días}.</span><span class="sxs-lookup"><span data-stu-id="b15bb-133">This new key has an activation date of { default key's expiration date } and an expiration date of { now + 90 days }.</span></span> <span data-ttu-id="b15bb-134">Esto permite al sistema poner automáticamente las claves de forma regular con ninguna interrupción del servicio.</span><span class="sxs-lookup"><span data-stu-id="b15bb-134">This allows the system to automatically roll keys on a regular basis with no interruption of service.</span></span>

<span data-ttu-id="b15bb-135">Puede haber circunstancias donde se creará una clave con la activación inmediata.</span><span class="sxs-lookup"><span data-stu-id="b15bb-135">There might be circumstances where a key will be created with immediate activation.</span></span> <span data-ttu-id="b15bb-136">Un ejemplo sería cuando la aplicación no se haya ejecutado durante un tiempo y todas las claves en el anillo de clave se ha caducado.</span><span class="sxs-lookup"><span data-stu-id="b15bb-136">One example would be when the application hasn't run for a time and all keys in the key ring are expired.</span></span> <span data-ttu-id="b15bb-137">Cuando esto ocurre, la clave se genera una fecha de activación de {ahora} sin el retardo de activación normal de 2 días.</span><span class="sxs-lookup"><span data-stu-id="b15bb-137">When this happens, the key is given an activation date of { now } without the normal 2-day activation delay.</span></span>

<span data-ttu-id="b15bb-138">La vigencia de la clave predeterminada es 90 días, aunque esto es configurable como en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="b15bb-138">The default key lifetime is 90 days, though this is configurable as in the following example.</span></span>

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

<span data-ttu-id="b15bb-139">Un administrador también puede cambiar el valor predeterminado de todo el sistema, aunque una llamada explícita a `SetDefaultKeyLifetime` invalidará cualquier directiva de todo el sistema.</span><span class="sxs-lookup"><span data-stu-id="b15bb-139">An administrator can also change the default system-wide, though an explicit call to `SetDefaultKeyLifetime` will override any system-wide policy.</span></span> <span data-ttu-id="b15bb-140">La vigencia de la clave predeterminada no puede ser inferior a 7 días.</span><span class="sxs-lookup"><span data-stu-id="b15bb-140">The default key lifetime cannot be shorter than 7 days.</span></span>

## <a name="automatic-key-ring-refresh"></a><span data-ttu-id="b15bb-141">Actualización automática clave de anillo</span><span class="sxs-lookup"><span data-stu-id="b15bb-141">Automatic key ring refresh</span></span>

<span data-ttu-id="b15bb-142">Cuando se inicializa el sistema de protección de datos, lee el anillo de clave desde el repositorio subyacente y lo almacena en caché en memoria.</span><span class="sxs-lookup"><span data-stu-id="b15bb-142">When the data protection system initializes, it reads the key ring from the underlying repository and caches it in memory.</span></span> <span data-ttu-id="b15bb-143">Esta memoria caché permite proteger y desproteger operaciones podrán continuar sin alcanzar el almacén de copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="b15bb-143">This cache allows Protect and Unprotect operations to proceed without hitting the backing store.</span></span> <span data-ttu-id="b15bb-144">El sistema comprobará automáticamente la memoria auxiliar para cambios aproximadamente cada 24 horas o cuando caduca la clave predeterminada actual, lo que ocurra primero.</span><span class="sxs-lookup"><span data-stu-id="b15bb-144">The system will automatically check the backing store for changes approximately every 24 hours or when the current default key expires, whichever comes first.</span></span>

>[!WARNING]
> <span data-ttu-id="b15bb-145">Los desarrolladores rara vez deberían (si alguna vez) que deba usar las API de administración clave directamente.</span><span class="sxs-lookup"><span data-stu-id="b15bb-145">Developers should very rarely (if ever) need to use the key management APIs directly.</span></span> <span data-ttu-id="b15bb-146">El sistema de protección de datos llevará a cabo la administración automática de claves como se describió anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b15bb-146">The data protection system will perform automatic key management as described above.</span></span>

<span data-ttu-id="b15bb-147">El sistema de protección de datos expone una interfaz `IKeyManager` que se puede utilizar para inspeccionar y realizar cambios en el anillo de clave.</span><span class="sxs-lookup"><span data-stu-id="b15bb-147">The data protection system exposes an interface `IKeyManager` that can be used to inspect and make changes to the key ring.</span></span> <span data-ttu-id="b15bb-148">El sistema DI que proporciona la instancia de `IDataProtectionProvider` también puede proporcionar una instancia de `IKeyManager` para su consumo.</span><span class="sxs-lookup"><span data-stu-id="b15bb-148">The DI system that provided the instance of `IDataProtectionProvider` can also provide an instance of `IKeyManager` for your consumption.</span></span> <span data-ttu-id="b15bb-149">Como alternativa, puede extraer el `IKeyManager` directamente desde el `IServiceProvider` como en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="b15bb-149">Alternatively, you can pull the `IKeyManager` straight from the `IServiceProvider` as in the example below.</span></span>

<span data-ttu-id="b15bb-150">Cualquier operación que modifica el anillo de clave (crear una nueva clave explícitamente o realizar una revocación) invalidará la memoria caché en memoria.</span><span class="sxs-lookup"><span data-stu-id="b15bb-150">Any operation which modifies the key ring (creating a new key explicitly or performing a revocation) will invalidate the in-memory cache.</span></span> <span data-ttu-id="b15bb-151">La siguiente llamada a `Protect` o `Unprotect` hará que el sistema de protección de datos leer el anillo de clave y volver a crear la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="b15bb-151">The next call to `Protect` or `Unprotect` will cause the data protection system to reread the key ring and recreate the cache.</span></span>

<span data-ttu-id="b15bb-152">El ejemplo siguiente muestra cómo utilizar el `IKeyManager` interfaz para inspeccionar y manipular el anillo de clave, incluida la revocación de claves existentes y generar una nueva clave manualmente.</span><span class="sxs-lookup"><span data-stu-id="b15bb-152">The sample below demonstrates using the `IKeyManager` interface to inspect and manipulate the key ring, including revoking existing keys and generating a new key manually.</span></span>

[!code-csharp[Main](key-management/samples/key-management.cs)]

## <a name="key-storage"></a><span data-ttu-id="b15bb-153">Almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="b15bb-153">Key storage</span></span>

<span data-ttu-id="b15bb-154">El sistema de protección de datos tiene una heurística mediante el cual intenta deducir automáticamente una ubicación de almacenamiento de claves adecuado y el cifrado en el mecanismo de rest.</span><span class="sxs-lookup"><span data-stu-id="b15bb-154">The data protection system has a heuristic whereby it tries to deduce an appropriate key storage location and encryption at rest mechanism automatically.</span></span> <span data-ttu-id="b15bb-155">Esto también es configurable por el desarrollador de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="b15bb-155">This is also configurable by the app developer.</span></span> <span data-ttu-id="b15bb-156">Los documentos siguientes explican las implementaciones de forma predeterminada de estos mecanismos:</span><span class="sxs-lookup"><span data-stu-id="b15bb-156">The following documents discuss the in-box implementations of these mechanisms:</span></span>

* [<span data-ttu-id="b15bb-157">Proveedores de almacenamiento de claves de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="b15bb-157">In-box key storage providers</span></span>](key-storage-providers.md#data-protection-implementation-key-storage-providers)

* [<span data-ttu-id="b15bb-158">Cifrado de claves de forma predeterminada en proveedores de rest</span><span class="sxs-lookup"><span data-stu-id="b15bb-158">In-box key encryption at rest providers</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)