---
title: "Configuración de protección de datos en ASP.NET Core"
author: rick-anderson
description: "Obtenga información acerca de cómo configurar la protección de datos en ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 07/17/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: c19b1a9cba89a02420c8792b575827d439bc262b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="configuring-data-protection-in-aspnet-core"></a><span data-ttu-id="eb7a1-103">Configuración de protección de datos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eb7a1-103">Configuring Data Protection in ASP.NET Core</span></span>

<span data-ttu-id="eb7a1-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eb7a1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eb7a1-105">Cuando se inicializa el sistema de protección de datos, se aplica [configuración predeterminada](xref:security/data-protection/configuration/default-settings) basado en el entorno operativo.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="eb7a1-106">Esta configuración es suelen ser adecuada para aplicaciones que se ejecutan en un único equipo.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="eb7a1-107">Hay casos donde un desarrollador puede cambiar la configuración predeterminada, es posible porque la aplicación se distribuye entre varias máquinas o por motivos de cumplimiento.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-107">There are cases where a developer may want to change the default settings, perhaps because their app is spread across multiple machines or for compliance reasons.</span></span> <span data-ttu-id="eb7a1-108">En estos casos, el sistema de protección de datos ofrece una API enriquecida de configuración.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-108">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

<span data-ttu-id="eb7a1-109">Hay un método de extensión [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) que devuelve un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="eb7a1-109">There's an extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) that returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="eb7a1-110">`IDataProtectionBuilder`expone métodos de extensión que se pueden encadenar juntos para configurar la protección de datos de opciones.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-110">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

## <a name="persistkeystofilesystem"></a><span data-ttu-id="eb7a1-111">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="eb7a1-111">PersistKeysToFileSystem</span></span>

<span data-ttu-id="eb7a1-112">Para almacenar las claves en un recurso compartido UNC en lugar de en el *% LOCALAPPDATA %* ubicación predeterminada, configure el sistema con [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="eb7a1-112">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="eb7a1-113">Si cambia la ubicación de persistencia de clave, el sistema cifra ya no automáticamente claves en reposo, ya que no sabe si DPAPI es un mecanismo de cifrado adecuado.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-113">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="eb7a1-114">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="eb7a1-114">ProtectKeysWith\*</span></span>

<span data-ttu-id="eb7a1-115">Puede configurar el sistema para proteger las claves en reposo mediante una llamada a cualquiera de los [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) API de configuración.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-115">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="eb7a1-116">Tenga en cuenta el ejemplo siguiente, que almacena las claves en un recurso compartido UNC y cifra estas claves en reposo con un certificado X.509 concreto:</span><span class="sxs-lookup"><span data-stu-id="eb7a1-116">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="eb7a1-117">Vea [clave de cifrado de datos almacenados](xref:security/data-protection/implementation/key-encryption-at-rest) para obtener más ejemplos e información sobre los mecanismos de cifrado de clave integrado.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-117">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="eb7a1-118">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="eb7a1-118">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="eb7a1-119">Para configurar el sistema para usar una vigencia de clave de 14 días en lugar del predeterminado de 90 días, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="eb7a1-119">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="eb7a1-120">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="eb7a1-120">SetApplicationName</span></span>

<span data-ttu-id="eb7a1-121">De forma predeterminada, el sistema de protección de datos aísla aplicaciones entre sí, incluso si comparte el mismo repositorio clave físico.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-121">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="eb7a1-122">Esto evita que las aplicaciones de descripción de todas las demás cargas protegido.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-122">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="eb7a1-123">Para compartir protegidos cargas entre las dos aplicaciones, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) con el mismo valor para cada aplicación:</span><span class="sxs-lookup"><span data-stu-id="eb7a1-123">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="eb7a1-124">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="eb7a1-124">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="eb7a1-125">Puede que tenga un escenario donde no desea que una aplicación para revertir automáticamente las claves (crear nuevas claves) tal y como se aproximen a expiración.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-125">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="eb7a1-126">Un ejemplo de esto podría ser aplicaciones configuradas en una relación principal/secundario, donde solo la aplicación principal es responsable de problemas de administración de claves y aplicaciones secundarias solo tienen una vista de solo lectura del anillo de clave.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-126">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="eb7a1-127">Las aplicaciones secundarias pueden configurarse para tratar el anillo de clave como de solo lectura configurando el sistema con [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="eb7a1-127">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="eb7a1-128">Aislamiento por aplicación</span><span class="sxs-lookup"><span data-stu-id="eb7a1-128">Per-application isolation</span></span>

<span data-ttu-id="eb7a1-129">Cuando el sistema de protección de datos se proporciona un host de ASP.NET Core, aísla automáticamente aplicaciones entre sí, incluso si esas aplicaciones se ejecuten en la misma cuenta de proceso de trabajo y están usando el mismo material de clave maestro.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-129">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="eb7a1-130">Esto es similar al modificador IsolateApps de System.Web  **\<machineKey >** elemento.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-130">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="eb7a1-131">El mecanismo de aislamiento funciona teniendo en cuenta cada aplicación en el equipo local como un único inquilino, por lo tanto la [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) para cualquier aplicación determinada incluye automáticamente el identificador de aplicación como un discriminador de la raíz.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-131">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="eb7a1-132">Identificador único de la aplicación procede de uno de estos dos lugares:</span><span class="sxs-lookup"><span data-stu-id="eb7a1-132">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="eb7a1-133">Si la aplicación se hospeda en IIS, el identificador único es la ruta de acceso de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-133">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="eb7a1-134">Si una aplicación se implementa en un entorno de granja de servidores web, este valor debería ser estable suponiendo que los entornos de IIS están configurados de forma similar en todos los equipos de la granja de servidores web.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-134">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="eb7a1-135">Si la aplicación no está hospedada en IIS, el identificador único es la ruta de acceso física de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-135">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="eb7a1-136">El identificador único está diseñado para sobrevivir restablece &mdash; tanto de la aplicación individual de la propia máquina.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-136">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="eb7a1-137">Este mecanismo de aislamiento se da por supuesto que las aplicaciones no son malintencionadas.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-137">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="eb7a1-138">Una aplicación malintencionada siempre puede afectar a cualquier otra aplicación que se ejecuta en la misma cuenta de proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-138">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="eb7a1-139">En un entorno de hospedaje compartido donde las aplicaciones no son de confianza mutua, el proveedor de hospedaje debe tomar medidas para garantizar el aislamiento de nivel de sistema operativo entre aplicaciones, incluida la separación de las aplicaciones subyacentes repositorios de clave.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-139">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="eb7a1-140">Si el sistema de protección de datos no se proporcionó un host de ASP.NET Core (por ejemplo, si crea instancias de él a través de la `DataProtectionProvider` tipo concreto) se deshabilita el aislamiento de la aplicación de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-140">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="eb7a1-141">Cuando se deshabilita el aislamiento de la aplicación, respaldadas por el mismo material de claves de todas las aplicaciones pueden compartir cargas mientras proporcionan adecuado [fines](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="eb7a1-141">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="eb7a1-142">Para proporcionar aislamiento de aplicaciones en este entorno, llame a la [SetApplicationName](#setapplicationname) método en la configuración de objeto y proporcione un nombre único para cada aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-142">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="eb7a1-143">Cambiar algoritmos con UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="eb7a1-143">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="eb7a1-144">La pila de protección de datos permite cambiar el algoritmo predeterminado usado por claves recién generado.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-144">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="eb7a1-145">La manera más sencilla de hacerlo consiste en llamar a [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) de la devolución de llamada de configuración:</span><span class="sxs-lookup"><span data-stu-id="eb7a1-145">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="eb7a1-146">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="eb7a1-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="eb7a1-147">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="eb7a1-147">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="eb7a1-148">El algoritmo de cifrado predeterminada es AES-256-CBC y el valor predeterminado ValidationAlgorithm es HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-148">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="eb7a1-149">La directiva predeterminada se puede establecer un administrador del sistema a través de un [todo el equipo directiva](xref:security/data-protection/configuration/machine-wide-policy), pero una llamada explícita a `UseCryptographicAlgorithms` invalida la directiva predeterminada.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-149">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="eb7a1-150">Al llamar a `UseCryptographicAlgorithms` le permite especificar el algoritmo deseado de una lista predefinida de integrados.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-150">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="eb7a1-151">No tiene que preocuparse acerca de la implementación del algoritmo.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-151">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="eb7a1-152">En el escenario anterior, el sistema de protección de datos intenta usar la implementación de CNG de AES si se ejecuta en Windows.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-152">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="eb7a1-153">En caso contrario, vuelve a los recursos administrados [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) clase.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-153">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="eb7a1-154">Puede especificar manualmente una implementación a través de una llamada a [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="eb7a1-154">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="eb7a1-155">Algoritmos de cambio no afecta a las claves existentes en el anillo de clave.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-155">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="eb7a1-156">Solo afecta a las claves recién generado.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-156">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="eb7a1-157">Especificar algoritmos administrados personalizados</span><span class="sxs-lookup"><span data-stu-id="eb7a1-157">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="eb7a1-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="eb7a1-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="eb7a1-159">Para especificar algoritmos administrados personalizados, cree una [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instancia que señala a los tipos de implementación:</span><span class="sxs-lookup"><span data-stu-id="eb7a1-159">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="eb7a1-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="eb7a1-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="eb7a1-161">Para especificar algoritmos administrados personalizados, cree una [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instancia que señala a los tipos de implementación:</span><span class="sxs-lookup"><span data-stu-id="eb7a1-161">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

<span data-ttu-id="eb7a1-162">Por lo general el \*propiedades de tipo deben apuntar a concreto, las implementaciones (a través de un constructor sin parámetros público) se pueden crear instancias de [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) y [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), aunque el casos de especial del sistema, como algunos valores `typeof(Aes)` para su comodidad.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-162">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="eb7a1-163">El SymmetricAlgorithm debe tener una longitud de clave de 128 bits ≥ y un tamaño de bloque de 64 bits ≥ y debe admitir el cifrado del modo CBC con relleno PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-163">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="eb7a1-164">El KeyedHashAlgorithm debe tener un tamaño de síntesis de > = 128 bits, y debe ser compatible con claves de una longitud igual a la longitud de texto implícita del algoritmo de hash.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-164">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="eb7a1-165">El KeyedHashAlgorithm no es estrictamente necesaria para ser HMAC.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-165">The KeyedHashAlgorithm is not strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="eb7a1-166">Especificar algoritmos personalizados de CNG de Windows</span><span class="sxs-lookup"><span data-stu-id="eb7a1-166">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="eb7a1-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="eb7a1-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="eb7a1-168">Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado de modo CBC con validación de HMAC, cree un [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instancia que contiene la información de algoritmo:</span><span class="sxs-lookup"><span data-stu-id="eb7a1-168">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="eb7a1-169">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="eb7a1-169">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="eb7a1-170">Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado de modo CBC con validación de HMAC, cree un [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instancia que contiene la información de algoritmo:</span><span class="sxs-lookup"><span data-stu-id="eb7a1-170">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> <span data-ttu-id="eb7a1-171">El algoritmo de cifrado de bloques simétrico debe tener una longitud de clave de > = 128 bits, un tamaño de bloque de > = 64 bits, y debe admitir el cifrado del modo CBC con relleno PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-171">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="eb7a1-172">El algoritmo hash debe tener un tamaño de síntesis de > = 128 bits y debe ser compatible con que se abre con la BCRYPT\_ALG\_controlar\_HMAC\_indicador de marca.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-172">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="eb7a1-173">El \*propiedades del proveedor se pueden establecer en null para utilizar el proveedor predeterminado para el algoritmo especificado.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-173">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="eb7a1-174">Consulte la [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-174">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="eb7a1-175">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="eb7a1-175">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="eb7a1-176">Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado del modo de Galois/contador con la validación, cree un [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instancia que contiene la información de algoritmo:</span><span class="sxs-lookup"><span data-stu-id="eb7a1-176">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="eb7a1-177">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="eb7a1-177">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="eb7a1-178">Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado del modo de Galois/contador con la validación, cree un [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instancia que contiene la información de algoritmo:</span><span class="sxs-lookup"><span data-stu-id="eb7a1-178">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> <span data-ttu-id="eb7a1-179">El algoritmo de cifrado de bloques simétrico debe tener una longitud de clave de > = 128 bits, un tamaño de bloque de 128 bits exactamente, y debe ser compatible con cifrado de GCM.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-179">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="eb7a1-180">Puede establecer la [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) propiedad en null para utilizar el proveedor predeterminado para el algoritmo especificado.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-180">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="eb7a1-181">Consulte la [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-181">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="eb7a1-182">Especificar otros algoritmos personalizados</span><span class="sxs-lookup"><span data-stu-id="eb7a1-182">Specifying other custom algorithms</span></span>

<span data-ttu-id="eb7a1-183">Aunque no se expone como una API de primera clase, el sistema de protección de datos es lo suficientemente extensible para permitir la especificación de casi cualquier tipo de algoritmo.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-183">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="eb7a1-184">Por ejemplo, es posible mantener todas las claves contenidas dentro de un módulo de seguridad de Hardware (HSM) y para proporcionar una implementación personalizada del núcleo de rutinas de cifrado y descifrado.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-184">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="eb7a1-185">Vea [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) en [principales de extensibilidad de criptografía](xref:security/data-protection/extensibility/core-crypto) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-185">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="eb7a1-186">Claves de persistencia cuando se hospedan en un contenedor de Docker</span><span class="sxs-lookup"><span data-stu-id="eb7a1-186">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="eb7a1-187">Cuando se hospedan en un [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) contenedor, las claves deben mantenerse en la vista:</span><span class="sxs-lookup"><span data-stu-id="eb7a1-187">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="eb7a1-188">Una carpeta que es un volumen de Docker que se conserva más allá de la duración del contenedor, como un volumen compartido o un volumen montado de host.</span><span class="sxs-lookup"><span data-stu-id="eb7a1-188">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="eb7a1-189">Un proveedor externo, como [el almacén de claves de Azure](https://azure.microsoft.com/services/key-vault/) o [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="eb7a1-189">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="eb7a1-190">Vea también</span><span class="sxs-lookup"><span data-stu-id="eb7a1-190">See also</span></span>

* [<span data-ttu-id="eb7a1-191">Escenarios no compatibles con DI</span><span class="sxs-lookup"><span data-stu-id="eb7a1-191">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="eb7a1-192">Directiva para toda la máquina</span><span class="sxs-lookup"><span data-stu-id="eb7a1-192">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)