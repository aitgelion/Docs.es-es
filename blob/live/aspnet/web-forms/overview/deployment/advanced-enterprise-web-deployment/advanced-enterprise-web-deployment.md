---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: "Avanzada de implementación Web de empresa | Documentos de Microsoft"
author: jrjlee
description: "Este tutorial le mostrará cómo realizar diversas tareas que están necesario o deseable en muchos escenarios de implementación de empresa. Para una translati italiano..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: c3cb7f63cf7c0246a0c4da6038a65a6ac43a7b59
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="advanced-enterprise-web-deployment"></a><span data-ttu-id="ea7af-104">Implementación de Web de empresa avanzadas</span><span class="sxs-lookup"><span data-stu-id="ea7af-104">Advanced Enterprise Web Deployment</span></span>
====================
<span data-ttu-id="ea7af-105">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="ea7af-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="ea7af-106">Descarga de PDF</span><span class="sxs-lookup"><span data-stu-id="ea7af-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="ea7af-107">Este tutorial le mostrará cómo realizar diversas tareas que están necesario o deseable en muchos escenarios de implementación de empresa.</span><span class="sxs-lookup"><span data-stu-id="ea7af-107">This tutorial will show you how to perform various tasks that are required or desirable in a lot of enterprise deployment scenarios.</span></span>
> 
> <span data-ttu-id="ea7af-108">Para una traducción de italiano de estos tutoriales, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).</span><span class="sxs-lookup"><span data-stu-id="ea7af-108">For an Italian translation of these tutorials, visit [http://www.lucamorelli.it](http://www.lucamorelli.it).</span></span>


<span data-ttu-id="ea7af-109">Esto forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales que utiliza una solución de ejemplo & #x 2014; la [póngase en contacto con el Administrador de](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución & #x 2014; para representar una aplicación web con un nivel de complejidad, incluso una aplicación de ASP.NET MVC 3, Windows realista Servicio de Communication Foundation (WCF) y un proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ea7af-109">This forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="ea7af-110">El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), en que el proceso de compilación se controla mediante dos proyectos, archivos de & #x 2014; uno que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación.</span><span class="sxs-lookup"><span data-stu-id="ea7af-110">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="ea7af-111">En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.</span><span class="sxs-lookup"><span data-stu-id="ea7af-111">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="ea7af-112">Información general del escenario</span><span class="sxs-lookup"><span data-stu-id="ea7af-112">Scenario Overview</span></span>

<span data-ttu-id="ea7af-113">Se describe el escenario de alto nivel para estos tutoriales en [implementación Web de empresa: información general del escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ea7af-113">The high-level scenario for these tutorials is described in [Enterprise Web Deployment: Scenario Overview](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).</span></span> <span data-ttu-id="ea7af-114">Se recomienda que revise este tema antes de empezar a trabajar en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="ea7af-114">We recommend that you review this topic before you get started on this tutorial.</span></span>

## <a name="how-to-use-this-tutorial"></a><span data-ttu-id="ea7af-115">Cómo usar este tutorial</span><span class="sxs-lookup"><span data-stu-id="ea7af-115">How to Use This Tutorial</span></span>

- <span data-ttu-id="ea7af-116">Cada uno de los temas de este tutorial es independiente entre sí y soluciona un problema que se produce en los escenarios de implementación de empresa o desafío particular.</span><span class="sxs-lookup"><span data-stu-id="ea7af-116">Each of the topics in this tutorial is self-contained and addresses a particular challenge or problem that occurs in enterprise deployment scenarios.</span></span> <span data-ttu-id="ea7af-117">No es necesario para que funcione a través de estos temas en ningún orden concreto.</span><span class="sxs-lookup"><span data-stu-id="ea7af-117">You don't need to work through these topics in any particular order.</span></span> <span data-ttu-id="ea7af-118">Sin embargo, en este tutorial se trata algunas tareas avanzadas.</span><span class="sxs-lookup"><span data-stu-id="ea7af-118">However, this tutorial covers some advanced tasks.</span></span> <span data-ttu-id="ea7af-119">Por lo tanto, debe familiarizarse con los conceptos y técnicas que los [implementación Web de la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) tutorial trata con el fin de obtener el máximo rendimiento de este contenido.</span><span class="sxs-lookup"><span data-stu-id="ea7af-119">As such, you should familiarize yourself with the concepts and techniques that the [Web Deployment in the Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) tutorial covers in order to gain the most benefit from this content.</span></span>
- <span data-ttu-id="ea7af-120">Este tutorial incluye en estos temas:</span><span class="sxs-lookup"><span data-stu-id="ea7af-120">This tutorial includes these topics:</span></span>
- <span data-ttu-id="ea7af-121">[Realizar una implementación de "¿Qué ocurre si"](performing-a-what-if-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ea7af-121">[Performing a "What If" Deployment](performing-a-what-if-deployment.md).</span></span> <span data-ttu-id="ea7af-122">En muchos escenarios, desea determinar el impacto de una implementación propuesto en un entorno de destino o cualquier contenido existente antes de implementar los cambios.</span><span class="sxs-lookup"><span data-stu-id="ea7af-122">In a lot of scenarios, you'll want to determine the impact of a proposed deployment on a target environment or any existing content before you actually make any changes.</span></span> <span data-ttu-id="ea7af-123">Este tema describe cómo ejecutar una implementación "¿Qué ocurre si" para generar archivos de registro y scripts de actualización de base de datos como si hubiera implementado contenido en un entorno de destino, sin realizar ningún cambio real.</span><span class="sxs-lookup"><span data-stu-id="ea7af-123">This topic describes how you can run a "what if" deployment to generate log files and database update scripts as if you had deployed content to a target environment, without actually making any changes.</span></span> <span data-ttu-id="ea7af-124">Analizar estos recursos puede ayudar a identificar posibles problemas antes de una implementación en vivo.</span><span class="sxs-lookup"><span data-stu-id="ea7af-124">Analyzing these resources can help you to spot any potential problems in advance of a live deployment.</span></span>
- <span data-ttu-id="ea7af-125">[Personalización de las implementaciones de la base de datos para varios entornos](customizing-database-deployments-for-multiple-environments.md).</span><span class="sxs-lookup"><span data-stu-id="ea7af-125">[Customizing Database Deployments for Multiple Environments](customizing-database-deployments-for-multiple-environments.md).</span></span> <span data-ttu-id="ea7af-126">Al implementar un proyecto de base de datos a varios destinos, a menudo desea personalizar las propiedades de implementación para cada entorno de destino.</span><span class="sxs-lookup"><span data-stu-id="ea7af-126">When you deploy a database project to multiple destinations, you'll often want to customize the deployment properties for each target environment.</span></span> <span data-ttu-id="ea7af-127">Por ejemplo, en entornos de prueba, se suele volver a la base de datos en cada implementación, mientras que en los entornos de ensayo o de producción sería mucho más probable que las actualizaciones incrementales para conservar los datos.</span><span class="sxs-lookup"><span data-stu-id="ea7af-127">For example, in test environments you'd typically recreate the database on every deployment, whereas in staging or production environments you'd be a lot more likely to make incremental updates to preserve your data.</span></span> <span data-ttu-id="ea7af-128">Este tema describe cómo se pueden incorporar estos cambios de propiedad en la lógica de implementación mediante la creación de un archivo de configuración (.sqldeployment) de la implementación específica del entorno para cada entorno de destino.</span><span class="sxs-lookup"><span data-stu-id="ea7af-128">This topic describes how you can incorporate these property changes into your deployment logic by creating an environment-specific deployment configuration (.sqldeployment) file for each target environment.</span></span>
- <span data-ttu-id="ea7af-129">Implementación de las pertenencias a roles de base de datos en entornos de prueba.</span><span class="sxs-lookup"><span data-stu-id="ea7af-129">Deploying Database Role Memberships to Test Environments.</span></span> <span data-ttu-id="ea7af-130">Al volver a crear una base de datos en cada implementación & #x 2014; por ejemplo, como parte de una compilación de integración continua (CI) e implementar en un entorno de prueba & #x 2014; normalmente necesitará configurar las pertenencias a roles de base de datos cada vez.</span><span class="sxs-lookup"><span data-stu-id="ea7af-130">When you recreate a database on every deployment&#x2014;for example, as part of a continuous integration (CI) build and deploy to a test environment&#x2014;you'll typically need to configure database role memberships every time.</span></span> <span data-ttu-id="ea7af-131">Por ejemplo, normalmente será necesario conceder permisos a la identidad del grupo de aplicaciones asociado a la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="ea7af-131">For example, you'll usually need to grant permissions to the application pool identity associated with your web application.</span></span> <span data-ttu-id="ea7af-132">Este tema describe cómo se puede automatizar este proceso mediante la adición de una secuencia de comandos SQL posterior a la implementación para la lógica de implementación.</span><span class="sxs-lookup"><span data-stu-id="ea7af-132">This topic describes how you can automate this process by adding a post-deployment SQL script to your deployment logic.</span></span>
- <span data-ttu-id="ea7af-133">[Implementación de las bases de datos de pertenencia en entornos empresariales](deploying-membership-databases-to-enterprise-environments.md).</span><span class="sxs-lookup"><span data-stu-id="ea7af-133">[Deploying Membership Databases to Enterprise Environments](deploying-membership-databases-to-enterprise-environments.md).</span></span> <span data-ttu-id="ea7af-134">Las bases de datos de pertenencia ASP.NET tienen varias características que pueden complicar el proceso de implementación.</span><span class="sxs-lookup"><span data-stu-id="ea7af-134">ASP.NET membership databases have various characteristics that can complicate the deployment process.</span></span> <span data-ttu-id="ea7af-135">Por ejemplo, una implementación de solo esquema dejará la base de datos en un estado no operativo.</span><span class="sxs-lookup"><span data-stu-id="ea7af-135">For example, a schema-only deployment will leave the database in a non-operational state.</span></span> <span data-ttu-id="ea7af-136">En la mayoría de los escenarios, es preferible para crear una base de datos de pertenencia directamente en cada entorno de destino.</span><span class="sxs-lookup"><span data-stu-id="ea7af-136">In most scenarios, it's preferable to create a membership database directly in each destination environment.</span></span> <span data-ttu-id="ea7af-137">Sin embargo, si tiene que implementar una base de datos de pertenencia, en este tema se describe algunos de los enfoques que puede usar para cumplir los desafíos inherentes.</span><span class="sxs-lookup"><span data-stu-id="ea7af-137">However, if you do have to deploy a membership database, this topic describes some of the approaches you can use to meet the inherent challenges.</span></span>
- <span data-ttu-id="ea7af-138">[Excluir archivos y carpetas de implementación](excluding-files-and-folders-from-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ea7af-138">[Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span> <span data-ttu-id="ea7af-139">En algunos escenarios, desea personalizar el contenido del paquete web a los entornos de destino específico.</span><span class="sxs-lookup"><span data-stu-id="ea7af-139">In some scenarios, you'll want to tailor the contents of your web package to specific destination environments.</span></span> <span data-ttu-id="ea7af-140">Por ejemplo, puede incluir las versiones completas de las bibliotecas de JavaScript cuando se implementa en un entorno de prueba, para admitir la depuración de cliente, pero usa reducidas versiones de las bibliotecas cuando se implementa en un entorno de ensayo o producción.</span><span class="sxs-lookup"><span data-stu-id="ea7af-140">For example, you might want to include full versions of JavaScript libraries when you deploy to a test environment, to support client-side debugging, but use minified versions of the libraries when you deploy to a staging or production environment.</span></span> <span data-ttu-id="ea7af-141">Este tema describe cómo puede excluir determinados archivos y carpetas desde el proceso de creación de paquete.</span><span class="sxs-lookup"><span data-stu-id="ea7af-141">This topic describes how you can exclude specific files and folders from the package creation process.</span></span>
- <span data-ttu-id="ea7af-142">[Implementarán aplicaciones sin conexión de toma de Web con Web](taking-web-applications-offline-with-web-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="ea7af-142">[Taking Web Applications Offline with Web Deploy](taking-web-applications-offline-with-web-deploy.md).</span></span> <span data-ttu-id="ea7af-143">Al implementar las soluciones en un entorno de ensayo o de producción, a menudo querrá tomar las aplicaciones web sin conexión durante el proceso de implementación.</span><span class="sxs-lookup"><span data-stu-id="ea7af-143">When you deploy solutions to a staging or production environment, you'll often want to take your web applications offline for the duration of the deployment process.</span></span> <span data-ttu-id="ea7af-144">En este tema se describe cómo agregar un *aplicación\_offline.htm* del archivo a la aplicación web al principio del proceso de implementación y quitar al final.</span><span class="sxs-lookup"><span data-stu-id="ea7af-144">This topic describes how you can add an *App\_offline.htm* file to your web application at the start of the deployment process and remove it at the end.</span></span> <span data-ttu-id="ea7af-145">Mientras el *aplicación\_offline.htm* archivo está en su lugar, cualquier usuario que vaya a la aplicación web se redirige automáticamente a la *aplicación\_offline.htm* archivo.</span><span class="sxs-lookup"><span data-stu-id="ea7af-145">While the *App\_offline.htm* file is in place, any users who browse to the web application are automatically redirected to the *App\_offline.htm* file.</span></span>
- <span data-ttu-id="ea7af-146">[Ejecutar Scripts de Windows PowerShell de MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md).</span><span class="sxs-lookup"><span data-stu-id="ea7af-146">[Running Windows PowerShell Scripts from MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md).</span></span> <span data-ttu-id="ea7af-147">Muchos escenarios de implementación requieren acciones posteriores a la implementación más complejas, como agregar orígenes de eventos personalizado en el registro o configurar la replicación entre instancias de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ea7af-147">Many deployment scenarios require more complex post-deployment actions, like adding custom event sources to the registry or configuring replication between SQL Server instances.</span></span> <span data-ttu-id="ea7af-148">Estas acciones se llevan a cabo a menudo a través de scripts de Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ea7af-148">These actions are often accomplished through Windows PowerShell scripts.</span></span> <span data-ttu-id="ea7af-149">En este tema se describe cómo ejecutar scripts de Windows PowerShell desde un archivo de proyecto de Microsoft Build Engine (MSBuild) como parte del proceso de compilación e implementación.</span><span class="sxs-lookup"><span data-stu-id="ea7af-149">This topic describes how to run Windows PowerShell scripts from a Microsoft Build Engine (MSBuild) project file as part of the build and deployment process.</span></span>
- <span data-ttu-id="ea7af-150">[Solucionar problemas del proceso de empaquetado](troubleshooting-the-packaging-process.md).</span><span class="sxs-lookup"><span data-stu-id="ea7af-150">[Troubleshooting the Packaging Process](troubleshooting-the-packaging-process.md).</span></span> <span data-ttu-id="ea7af-151">La canalización de publicación de Web (WPP) define una propiedad de MSBuild denominada **EnablePackageProcessLoggingAndAssert** que puede usar para generar información detallada sobre el proceso de empaquetado para proyectos de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="ea7af-151">The Web Publishing Pipeline (WPP) defines an MSBuild property named **EnablePackageProcessLoggingAndAssert** that you can use to generate in-depth information about the packaging process for web application projects.</span></span> <span data-ttu-id="ea7af-152">Este tema describe lo que hace la propiedad y cómo utilizarlo.</span><span class="sxs-lookup"><span data-stu-id="ea7af-152">This topic describes what the property does and how to use it.</span></span>

## <a name="key-technologies"></a><span data-ttu-id="ea7af-153">Tecnologías de clave</span><span class="sxs-lookup"><span data-stu-id="ea7af-153">Key Technologies</span></span>

<span data-ttu-id="ea7af-154">Este tutorial se centra en cómo utilizar estos productos y tecnologías para admitir la compilación automatizada y la implementación web:</span><span class="sxs-lookup"><span data-stu-id="ea7af-154">This tutorial focuses on how to use these products and technologies to support automated build and web deployment:</span></span>

- <span data-ttu-id="ea7af-155">Visual Studio 2010 y Team Foundation Server (TFS) 2010</span><span class="sxs-lookup"><span data-stu-id="ea7af-155">Visual Studio 2010 and Team Foundation Server (TFS) 2010</span></span>
- <span data-ttu-id="ea7af-156">MSBuild y compilación de equipo TFS</span><span class="sxs-lookup"><span data-stu-id="ea7af-156">MSBuild and TFS Team Build</span></span>
- <span data-ttu-id="ea7af-157">Servicios de Internet Information Server (IIS) 7.5</span><span class="sxs-lookup"><span data-stu-id="ea7af-157">Internet Information Services (IIS) 7.5</span></span>
- <span data-ttu-id="ea7af-158">Herramienta de implementación Web de IIS (Web Deploy) 2.1</span><span class="sxs-lookup"><span data-stu-id="ea7af-158">IIS Web Deployment Tool (Web Deploy) 2.1</span></span>
- <span data-ttu-id="ea7af-159">La utilidad de implementación de la base de datos de VSDBCMD.exe</span><span class="sxs-lookup"><span data-stu-id="ea7af-159">The VSDBCMD.exe database deployment utility</span></span>

## <a name="other-tutorials-in-this-series"></a><span data-ttu-id="ea7af-160">Otros tutoriales de esta serie</span><span class="sxs-lookup"><span data-stu-id="ea7af-160">Other Tutorials in This Series</span></span>

<span data-ttu-id="ea7af-161">Esto forma parte de una serie de cinco tutoriales en la implementación de web de escala empresarial.</span><span class="sxs-lookup"><span data-stu-id="ea7af-161">This forms part of a series of five tutorials on enterprise-scale web deployment.</span></span> <span data-ttu-id="ea7af-162">Estos son otros tutoriales de la serie:</span><span class="sxs-lookup"><span data-stu-id="ea7af-162">These are other tutorials in the series:</span></span>

- <span data-ttu-id="ea7af-163">[Implementar aplicaciones Web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="ea7af-163">[Deploying Web Applications in Enterprise Scenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span></span> <span data-ttu-id="ea7af-164">Este contenido de Introducción proporciona el fondo contextual para la serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="ea7af-164">This introductory content provides the contextual background for the tutorial series.</span></span> <span data-ttu-id="ea7af-165">Describe el escenario del tutorial, y muestra cómo encajan las tareas y se describe en toda la serie de tutoriales en un proceso más amplio de Application Lifecycle Management (ALM).</span><span class="sxs-lookup"><span data-stu-id="ea7af-165">It describes the tutorial scenario, and it illustrates how the tasks and walkthroughs described throughout the series fit into a broader Application Lifecycle Management (ALM) process.</span></span>
- <span data-ttu-id="ea7af-166">[Implementación de la empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).</span><span class="sxs-lookup"><span data-stu-id="ea7af-166">[Web Deployment in the Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).</span></span> <span data-ttu-id="ea7af-167">Este tutorial proporciona una introducción conceptual a los archivos de proyecto de MSBuild, el WPP, Web Deploy y otras tecnologías relacionadas.</span><span class="sxs-lookup"><span data-stu-id="ea7af-167">This tutorial provides a conceptual introduction to MSBuild project files, the WPP, Web Deploy, and other related technologies.</span></span> <span data-ttu-id="ea7af-168">Se explica cómo puede usar conjuntamente estas herramientas para administrar los procesos de implementación complejos.</span><span class="sxs-lookup"><span data-stu-id="ea7af-168">It explains how you can use these tools together to manage complex deployment processes.</span></span>
- <span data-ttu-id="ea7af-169">[Configurar entornos de servidor para la implementación Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ea7af-169">[Configuring Server Environments for Web Deployment](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).</span></span> <span data-ttu-id="ea7af-170">Este tutorial describe cómo configurar servidores de Windows para admitir diferentes escenarios de implementación, incluida la implementación de paquete de web remoto mediante el servicio Web Deployment Agent (agente remoto) o el controlador de implementar de Web y la implementación de la base de datos remota.</span><span class="sxs-lookup"><span data-stu-id="ea7af-170">This tutorial describes how to configure Windows servers to support various deployment scenarios, including remote web package deployment using the Web Deployment Agent Service (the remote agent) or the Web Deploy Handler and remote database deployment.</span></span> <span data-ttu-id="ea7af-171">Proporciona instrucciones sobre cómo elegir el método de implementación adecuada para su propio entorno, y se describe cómo usar Web Farm Framework (WFF) para replicar las aplicaciones web implementadas a través de todos los servidores web en una granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="ea7af-171">It provides guidance on choosing the appropriate deployment method for your own environment, and it describes how to use the Web Farm Framework (WFF) to replicate deployed web applications across all the web servers in a server farm.</span></span>
- <span data-ttu-id="ea7af-172">[Configurar Team Foundation Server para la implementación Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ea7af-172">[Configuring Team Foundation Server for Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span></span> <span data-ttu-id="ea7af-173">Este tutorial describe cómo configurar TFS para admitir diferentes escenarios de implementación, incluida la implementación automatizada como parte de un proceso de integración continua y desencadena manualmente las implementaciones de compilaciones concretas.</span><span class="sxs-lookup"><span data-stu-id="ea7af-173">This tutorial describes how to configure TFS to support various deployment scenarios, including automated deployment as part of a CI process and manually triggered deployments of specific builds.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="ea7af-174">Siguiente</span><span class="sxs-lookup"><span data-stu-id="ea7af-174">Next</span></span>](performing-a-what-if-deployment.md)