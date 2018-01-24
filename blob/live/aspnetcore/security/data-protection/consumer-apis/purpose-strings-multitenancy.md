---
title: "Cadenas de propósito en ASP.NET Core"
author: rick-anderson
description: "Este documento describen la arquitectura multiempresa y jerarquía de la cadena de propósito en lo referente a las API de protección de datos de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: e720c3e245f4a20ffeb728035ee3ad3c1320ab92
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a><span data-ttu-id="ae67c-103">Jerarquía de propósito y la arquitectura multiempresa en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae67c-103">Purpose hierarchy and multi-tenancy in ASP.NET Core</span></span>

<span data-ttu-id="ae67c-104">Puesto que un `IDataProtector` también es implícitamente una `IDataProtectionProvider`, con fines se pueden encadenar juntas.</span><span class="sxs-lookup"><span data-stu-id="ae67c-104">Since an `IDataProtector` is also implicitly an `IDataProtectionProvider`, purposes can be chained together.</span></span> <span data-ttu-id="ae67c-105">En este sentido, `provider.CreateProtector([ "purpose1", "purpose2" ])` es equivalente a `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.</span><span class="sxs-lookup"><span data-stu-id="ae67c-105">In this sense, `provider.CreateProtector([ "purpose1", "purpose2" ])` is equivalent to `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.</span></span>

<span data-ttu-id="ae67c-106">Esto permite algunas relaciones jerárquicas interesantes a través del sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="ae67c-106">This allows for some interesting hierarchical relationships through the data protection system.</span></span> <span data-ttu-id="ae67c-107">En el ejemplo anterior de [Contoso.Messaging.SecureMessage](purpose-strings.md#data-protection-contoso-purpose), puede llamar el componente SecureMessage `provider.CreateProtector("Contoso.Messaging.SecureMessage")` inicial una vez y almacenar en caché el resultado en privado `_myProvide` campo.</span><span class="sxs-lookup"><span data-stu-id="ae67c-107">In the earlier example of [Contoso.Messaging.SecureMessage](purpose-strings.md#data-protection-contoso-purpose), the SecureMessage component can call `provider.CreateProtector("Contoso.Messaging.SecureMessage")` once up-front and cache the result into a private `_myProvide` field.</span></span> <span data-ttu-id="ae67c-108">Protectores futuras, a continuación, pueden crearse a través de llamadas a `_myProvider.CreateProtector("User: username")`, y se utilizan estos protectores para proteger los mensajes individuales.</span><span class="sxs-lookup"><span data-stu-id="ae67c-108">Future protectors can then be created via calls to `_myProvider.CreateProtector("User: username")`, and these protectors would be used for securing the individual messages.</span></span>

<span data-ttu-id="ae67c-109">Esto también se voltea.</span><span class="sxs-lookup"><span data-stu-id="ae67c-109">This can also be flipped.</span></span> <span data-ttu-id="ae67c-110">Considere la posibilidad de una sola aplicación lógica qué hosts de varios inquilinos (un CMS parece razonable) y cada inquilino pueden configurarse con su propio sistema de administración de autenticación y el estado.</span><span class="sxs-lookup"><span data-stu-id="ae67c-110">Consider a single logical application which hosts multiple tenants (a CMS seems reasonable), and each tenant can be configured with its own authentication and state management system.</span></span> <span data-ttu-id="ae67c-111">La aplicación aglutina tiene un proveedor de maestro único y llama `provider.CreateProtector("Tenant 1")` y `provider.CreateProtector("Tenant 2")` para proporcionar a cada inquilino de su propio segmento aislado del sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="ae67c-111">The umbrella application has a single master provider, and it calls `provider.CreateProtector("Tenant 1")` and `provider.CreateProtector("Tenant 2")` to give each tenant its own isolated slice of the data protection system.</span></span> <span data-ttu-id="ae67c-112">Los inquilinos, a continuación, pudieron derivar sus propios protectores individuales según sus propias necesidades, pero con independencia de forma rígida intentan no pueden crear protectores que estén en conflicto con otro inquilino en el sistema.</span><span class="sxs-lookup"><span data-stu-id="ae67c-112">The tenants could then derive their own individual protectors based on their own needs, but no matter how hard they try they cannot create protectors which collide with any other tenant in the system.</span></span> <span data-ttu-id="ae67c-113">Esto se representa gráficamente las indicadas a continuación.</span><span class="sxs-lookup"><span data-stu-id="ae67c-113">Graphically, this is represented as below.</span></span>

![Con fines de varios inquilinos](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> <span data-ttu-id="ae67c-115">Se supone el paraguas de controles de la aplicación qué API están disponibles a los inquilinos individuales y que los inquilinos no pueden ejecutar código arbitrario en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ae67c-115">This assumes the umbrella application controls which APIs are available to individual tenants and that tenants cannot execute arbitrary code on the server.</span></span> <span data-ttu-id="ae67c-116">Si un inquilino puede ejecutar código arbitrario, podrían realizar la reflexión privada para interrumpir las garantías de aislamiento, o que simplemente podrían leer directamente el material de claves principal y derivan de las subclaves lo desean.</span><span class="sxs-lookup"><span data-stu-id="ae67c-116">If a tenant can execute arbitrary code, they could perform private reflection to break the isolation guarantees, or they could just read the master keying material directly and derive whatever subkeys they desire.</span></span>

<span data-ttu-id="ae67c-117">El sistema de protección de datos utiliza realmente una ordenación de la arquitectura multiempresa en su configuración de predeterminada de fábrica.</span><span class="sxs-lookup"><span data-stu-id="ae67c-117">The data protection system actually uses a sort of multi-tenancy in its default out-of-the-box configuration.</span></span> <span data-ttu-id="ae67c-118">De forma predeterminada el material de claves maestro se almacena en la carpeta de perfil de usuario de la cuenta de proceso de trabajo (o el registro, para identidades de grupo de aplicaciones de IIS).</span><span class="sxs-lookup"><span data-stu-id="ae67c-118">By default master keying material is stored in the worker process account's user profile folder (or the registry, for IIS application pool identities).</span></span> <span data-ttu-id="ae67c-119">Pero es realmente bastante habitual usar una única cuenta para ejecutar varias aplicaciones y, por tanto, todas estas aplicaciones se terminen compartiendo al maestro de material de claves.</span><span class="sxs-lookup"><span data-stu-id="ae67c-119">But it is actually fairly common to use a single account to run multiple applications, and thus all these applications would end up sharing the master keying material.</span></span> <span data-ttu-id="ae67c-120">Para resolver este problema, el sistema de protección de datos inserta automáticamente un identificador único por la aplicación como el primer elemento de la cadena de propósito general.</span><span class="sxs-lookup"><span data-stu-id="ae67c-120">To solve this, the data protection system automatically inserts a unique-per-application identifier as the first element in the overall purpose chain.</span></span> <span data-ttu-id="ae67c-121">Ello implícita sirve para [aislar las aplicaciones individuales](xref:security/data-protection/configuration/overview#per-application-isolation) entre sí al tratar de forma eficaz cada aplicación como un único inquilino dentro del sistema y el proceso de creación de protector parece idéntico a la imagen anterior.</span><span class="sxs-lookup"><span data-stu-id="ae67c-121">This implicit purpose serves to [isolate individual applications](xref:security/data-protection/configuration/overview#per-application-isolation) from one another by effectively treating each application as a unique tenant within the system, and the protector creation process looks identical to the image above.</span></span>