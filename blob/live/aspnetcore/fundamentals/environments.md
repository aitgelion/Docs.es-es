---
title: Trabajar con varios entornos de ASP.NET Core
author: ardalis
description: "Obtenga información acerca de cómo ASP.NET Core proporciona compatibilidad para controlar el comportamiento de la aplicación en varios entornos."
keywords: "Núcleo de ASP.NET, la configuración del entorno, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b5bba985-be12-4464-9a01-df3599b2a6f1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 9127c3d7180422c0e3dbd813340dd485bf360c81
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="80ce6-104">Trabajar con varios entornos</span><span class="sxs-lookup"><span data-stu-id="80ce6-104">Working with multiple environments</span></span>

<span data-ttu-id="80ce6-105">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="80ce6-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="80ce6-106">ASP.NET Core proporciona compatibilidad para controlar el comportamiento de la aplicación en varios entornos, como desarrollo, ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="80ce6-106">ASP.NET Core provides support for controlling app behavior across multiple environments, such as development, staging, and production.</span></span> <span data-ttu-id="80ce6-107">Las variables de entorno se utilizan para indicar el entorno en tiempo de ejecución, que permite a la aplicación que se configurará para ese entorno.</span><span class="sxs-lookup"><span data-stu-id="80ce6-107">Environment variables are used to indicate the runtime environment, allowing the app to be configured for that environment.</span></span>

<span data-ttu-id="80ce6-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="80ce6-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="development-staging-production"></a><span data-ttu-id="80ce6-109">Desarrollo, prueba, producción</span><span class="sxs-lookup"><span data-stu-id="80ce6-109">Development, Staging, Production</span></span>

<span data-ttu-id="80ce6-110">ASP.NET Core hace referencia a una variable de entorno en particular, `ASPNETCORE_ENVIRONMENT` para describir el entorno se está ejecutando actualmente en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="80ce6-110">ASP.NET Core references a particular environment variable, `ASPNETCORE_ENVIRONMENT` to describe the environment the application is currently running in.</span></span> <span data-ttu-id="80ce6-111">Esta variable se puede establecer en cualquier valor que desee, pero por convención, se utilizan tres valores: `Development`, `Staging`, y `Production`.</span><span class="sxs-lookup"><span data-stu-id="80ce6-111">This variable can be set to any value you like, but three values are used by convention: `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="80ce6-112">Encontrará estos valores utilizados en los ejemplos y las plantillas proporcionadas con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="80ce6-112">You will find these values used in the samples and templates provided with ASP.NET Core.</span></span>

<span data-ttu-id="80ce6-113">Se pueden detectar mediante programación la configuración del entorno actual desde dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="80ce6-113">The current environment setting can be detected programmatically from within your application.</span></span> <span data-ttu-id="80ce6-114">Además, puede usar el entorno de [etiqueta auxiliar](../mvc/views/tag-helpers/index.md) para incluir determinadas secciones en su [vista](../mvc/views/index.md) según el entorno de aplicación actual.</span><span class="sxs-lookup"><span data-stu-id="80ce6-114">In addition, you can use the Environment [tag helper](../mvc/views/tag-helpers/index.md) to include certain sections in your [view](../mvc/views/index.md) based on the current application environment.</span></span>

<span data-ttu-id="80ce6-115">Nota: En Windows y Mac OS, distingue mayúsculas de minúsculas del nombre de entorno especificado.</span><span class="sxs-lookup"><span data-stu-id="80ce6-115">Note: On Windows and macOS, the specified environment name is case insensitive.</span></span> <span data-ttu-id="80ce6-116">Si se establece la variable `Development` o `development` o `DEVELOPMENT` los resultados serán los mismos.</span><span class="sxs-lookup"><span data-stu-id="80ce6-116">Whether you set the variable to `Development` or `development` or `DEVELOPMENT` the results will be the same.</span></span> <span data-ttu-id="80ce6-117">Sin embargo, Linux es un **entre mayúsculas y minúsculas** sistema operativo de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="80ce6-117">However, Linux is a **case sensitive** OS by default.</span></span> <span data-ttu-id="80ce6-118">Las variables de entorno, los nombres de archivo y configuración requiere entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="80ce6-118">Environment variables, file names and settings require case sensitivity.</span></span>

### <a name="development"></a><span data-ttu-id="80ce6-119">Desarrollo</span><span class="sxs-lookup"><span data-stu-id="80ce6-119">Development</span></span>

<span data-ttu-id="80ce6-120">Debe ser el entorno que se utiliza al desarrollar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="80ce6-120">This should be the environment used when developing an application.</span></span> <span data-ttu-id="80ce6-121">Normalmente se utiliza para habilitar las características que no desea que estén disponibles cuando se ejecute la aplicación en producción, como el [página de excepción para desarrolladores](xref:fundamentals/error-handling#the-developer-exception-page).</span><span class="sxs-lookup"><span data-stu-id="80ce6-121">It is typically used to enable features that you wouldn't want to be available when the app runs in production, such as the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page).</span></span>

<span data-ttu-id="80ce6-122">Si está utilizando Visual Studio, el entorno se puede configurar en perfiles de depuración del proyecto.</span><span class="sxs-lookup"><span data-stu-id="80ce6-122">If you're using Visual Studio, the environment can be configured in your project's debug profiles.</span></span> <span data-ttu-id="80ce6-123">Depurar perfiles especificar el [server](xref:fundamentals/servers/index) a utilizar al iniciar la aplicación y las variables de entorno para establecerse.</span><span class="sxs-lookup"><span data-stu-id="80ce6-123">Debug profiles specify the [server](xref:fundamentals/servers/index) to use when launching the application and any environment variables to be set.</span></span> <span data-ttu-id="80ce6-124">El proyecto puede tener varios perfiles de depuración que establecen variables de entorno de manera diferente.</span><span class="sxs-lookup"><span data-stu-id="80ce6-124">Your project can have multiple debug profiles that set environment variables differently.</span></span> <span data-ttu-id="80ce6-125">Estos perfiles se administran mediante el **depurar** ficha de su proyecto de aplicación web **propiedades** menú.</span><span class="sxs-lookup"><span data-stu-id="80ce6-125">You manage these profiles by using the **Debug** tab of your web application project's **Properties** menu.</span></span> <span data-ttu-id="80ce6-126">Se conservan los valores establecidos en Propiedades del proyecto en el *launchSettings.json* archivo y también puede configurar perfiles editando directamente el archivo.</span><span class="sxs-lookup"><span data-stu-id="80ce6-126">The values you set in project properties are persisted in the *launchSettings.json* file, and you can also configure profiles by editing that file directly.</span></span>

<span data-ttu-id="80ce6-127">El perfil de IIS Express se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="80ce6-127">The profile for IIS Express is shown here:</span></span>

![Variables de entorno de configuración de propiedades de proyecto](environments/_static/project-properties-debug.png)

<span data-ttu-id="80ce6-129">Este es un `launchSettings.json` archivo que incluye los perfiles para `Development` y `Staging`:</span><span class="sxs-lookup"><span data-stu-id="80ce6-129">Here is a `launchSettings.json` file that includes profiles for `Development` and `Staging`:</span></span>

[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]

<span data-ttu-id="80ce6-130">Podrán que los cambios realizados en los perfiles de los proyectos no surtan efecto hasta que se reinicie el servidor web usado (en particular, Kestrel debe reiniciarse para que se detectarán los cambios realizados en su entorno).</span><span class="sxs-lookup"><span data-stu-id="80ce6-130">Changes made to project profiles may not take effect until the web server used is restarted (in particular, Kestrel must be restarted before it will detect changes made to its environment).</span></span>

>[!WARNING]
> <span data-ttu-id="80ce6-131">Las variables de entorno se almacenan en *launchSettings.json* no están protegidos de cualquier forma y va a formar parte del repositorio de código fuente para el proyecto, si utiliza uno.</span><span class="sxs-lookup"><span data-stu-id="80ce6-131">Environment variables stored in *launchSettings.json* are not secured in any way and will be part of the source code repository for your project, if you use one.</span></span> <span data-ttu-id="80ce6-132">**Nunca almacenan credenciales ni otros datos secretos en este archivo.**</span><span class="sxs-lookup"><span data-stu-id="80ce6-132">**Never store credentials or other secret data in this file.**</span></span> <span data-ttu-id="80ce6-133">Si necesita un lugar para almacenar estos datos, use la *Manager secreto* herramienta se describe en [almacenamiento seguro para la ejecución de secretos de aplicación durante el desarrollo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="80ce6-133">If you need a place to store such data, use the *Secret Manager* tool described in [Safe storage of app secrets during development](xref:security/app-secrets).</span></span>

### <a name="staging"></a><span data-ttu-id="80ce6-134">Almacenamiento provisional</span><span class="sxs-lookup"><span data-stu-id="80ce6-134">Staging</span></span>

<span data-ttu-id="80ce6-135">Por convención, un `Staging` entorno es un entorno de preproducción utilizado para la prueba final antes de la implementación en producción.</span><span class="sxs-lookup"><span data-stu-id="80ce6-135">By convention, a `Staging` environment is a pre-production environment used for final testing before deployment to production.</span></span> <span data-ttu-id="80ce6-136">Idealmente, sus características físicas deben reflejar de producción, para que se produzcan los problemas que pueden surgir en producción primero en el entorno de ensayo, donde pueden tratarse sin afectar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="80ce6-136">Ideally, its physical characteristics should mirror that of production, so that any issues that may arise in production occur first in the staging environment, where they can be addressed without impact to users.</span></span>

### <a name="production"></a><span data-ttu-id="80ce6-137">Producción</span><span class="sxs-lookup"><span data-stu-id="80ce6-137">Production</span></span>

<span data-ttu-id="80ce6-138">El `Production` entorno es el entorno en el que se ejecuta la aplicación cuando está activa y que se va a utilizar los usuarios finales.</span><span class="sxs-lookup"><span data-stu-id="80ce6-138">The `Production` environment is the environment in which the application runs when it is live and being used by end users.</span></span> <span data-ttu-id="80ce6-139">Este entorno debe configurarse para maximizar la seguridad, rendimiento y eficacia de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="80ce6-139">This environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="80ce6-140">Algunas configuraciones comunes que podría tener un entorno de producción que serían diferentes entornos de desarrollo incluyen:</span><span class="sxs-lookup"><span data-stu-id="80ce6-140">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="80ce6-141">Activar el almacenamiento en caché</span><span class="sxs-lookup"><span data-stu-id="80ce6-141">Turn on caching</span></span>

* <span data-ttu-id="80ce6-142">Asegúrese de todos los recursos de cliente están agrupados, reducidos y potencialmente provienen de una CDN</span><span class="sxs-lookup"><span data-stu-id="80ce6-142">Ensure all client-side resources are bundled, minified, and potentially served from a CDN</span></span>

* <span data-ttu-id="80ce6-143">Desactivar ErrorPages diagnóstico</span><span class="sxs-lookup"><span data-stu-id="80ce6-143">Turn off diagnostic ErrorPages</span></span>

* <span data-ttu-id="80ce6-144">Activar las páginas de error descriptivo</span><span class="sxs-lookup"><span data-stu-id="80ce6-144">Turn on friendly error pages</span></span>

* <span data-ttu-id="80ce6-145">Habilitar la supervisión y el registro de producción (por ejemplo, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))</span><span class="sxs-lookup"><span data-stu-id="80ce6-145">Enable production logging and monitoring (for example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))</span></span>

<span data-ttu-id="80ce6-146">Esto es no pretende ser una lista completa.</span><span class="sxs-lookup"><span data-stu-id="80ce6-146">This is by no means meant to be a complete list.</span></span> <span data-ttu-id="80ce6-147">Es mejor evitar esparcirlo comprobaciones de entorno en muchas partes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="80ce6-147">It's best to avoid scattering environment checks in many parts of your application.</span></span> <span data-ttu-id="80ce6-148">En su lugar, el enfoque recomendado es realizar tales comprobaciones dentro de la aplicación `Startup` clases siempre que sea posible</span><span class="sxs-lookup"><span data-stu-id="80ce6-148">Instead, the recommended approach is to perform such checks within the application's `Startup` class(es) wherever possible</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="80ce6-149">Configurar el entorno</span><span class="sxs-lookup"><span data-stu-id="80ce6-149">Setting the environment</span></span>

<span data-ttu-id="80ce6-150">El método para establecer el entorno depende del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="80ce6-150">The method for setting the environment depends on the operating system.</span></span>

### <a name="windows"></a><span data-ttu-id="80ce6-151">Windows</span><span class="sxs-lookup"><span data-stu-id="80ce6-151">Windows</span></span>
<span data-ttu-id="80ce6-152">Para establecer el `ASPNETCORE_ENVIRONMENT` para la sesión actual, si la aplicación se inicia con `dotnet run`, se utilizan los siguientes comandos</span><span class="sxs-lookup"><span data-stu-id="80ce6-152">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="80ce6-153">**Línea de comandos**</span><span class="sxs-lookup"><span data-stu-id="80ce6-153">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="80ce6-154">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="80ce6-154">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="80ce6-155">Estos comandos sólo tendrán efecto en la ventana actual.</span><span class="sxs-lookup"><span data-stu-id="80ce6-155">These commands take effect only for the current window.</span></span> <span data-ttu-id="80ce6-156">Cuando se cierra la ventana, la configuración de ASPNETCORE_ENVIRONMENT vuelve a la configuración predeterminada o el valor de la máquina.</span><span class="sxs-lookup"><span data-stu-id="80ce6-156">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="80ce6-157">Para establecer el valor global en ventanas abiertas del **el Panel de Control** > **System** > **configuración avanzada del sistema** y agregar o editar la `ASPNETCORE_ENVIRONMENT` valor.</span><span class="sxs-lookup"><span data-stu-id="80ce6-157">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Propiedades avanzadas del sistema](environments/_static/systemsetting_environment.png)

![Variable de entorno de ASPNET Core](environments/_static/windows_aspnetcore_environment.png) 

<span data-ttu-id="80ce6-160">**Web.config**</span><span class="sxs-lookup"><span data-stu-id="80ce6-160">**web.config**</span></span>

<span data-ttu-id="80ce6-161">Consulte la *establecer variables de entorno* sección de la [referencia de configuración de ASP.NET Core módulo](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) tema.</span><span class="sxs-lookup"><span data-stu-id="80ce6-161">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="80ce6-162">**Por grupo de aplicaciones de IIS**</span><span class="sxs-lookup"><span data-stu-id="80ce6-162">**Per IIS Application Pool**</span></span>

<span data-ttu-id="80ce6-163">Si tiene que establecer variables de entorno para aplicaciones individuales que se ejecutan en grupos de aplicaciones aislados (se admite en IIS 10.0+), vea la sección *Comando AppCmd.exe* del tema [Variables de entorno \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) de la documentación de referencia de IIS.</span><span class="sxs-lookup"><span data-stu-id="80ce6-163">If you need to set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

### <a name="macos"></a><span data-ttu-id="80ce6-164">macOS</span><span class="sxs-lookup"><span data-stu-id="80ce6-164">macOS</span></span>
<span data-ttu-id="80ce6-165">Configurar el entorno actual para macOS puede realizarse en línea cuando se ejecuta la aplicación;</span><span class="sxs-lookup"><span data-stu-id="80ce6-165">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="80ce6-166">o con `export` establecer antes de ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="80ce6-166">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
``` 
<span data-ttu-id="80ce6-167">Las variables de entorno de nivel de equipo se establecen en los *.bashrc* o *. bash_profile* archivo.</span><span class="sxs-lookup"><span data-stu-id="80ce6-167">Machine level environment variables are set in the *.bashrc*  or *.bash_profile* file.</span></span> <span data-ttu-id="80ce6-168">Edite el archivo con cualquier editor de texto y agregue la siguiente instrucción.</span><span class="sxs-lookup"><span data-stu-id="80ce6-168">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```  

### <a name="linux"></a><span data-ttu-id="80ce6-169">Linux</span><span class="sxs-lookup"><span data-stu-id="80ce6-169">Linux</span></span>
<span data-ttu-id="80ce6-170">Para distribuciones de Linux, use la `export` comando en la línea de comandos para la configuración de la variable basado en sesiones y *bash_profile* archivo de configuración del entorno de nivel de máquina.</span><span class="sxs-lookup"><span data-stu-id="80ce6-170">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

## <a name="determining-the-environment-at-runtime"></a><span data-ttu-id="80ce6-171">Determinar el entorno en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="80ce6-171">Determining the environment at runtime</span></span>

<span data-ttu-id="80ce6-172">El `IHostingEnvironment` servicio proporciona la abstracción básica para trabajar con entornos.</span><span class="sxs-lookup"><span data-stu-id="80ce6-172">The `IHostingEnvironment` service provides the core abstraction for working with environments.</span></span> <span data-ttu-id="80ce6-173">Este servicio es proporcionado por ASP.NET hospeda las capas y puede insertarse en la lógica de inicio a través de [inyección de dependencia](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="80ce6-173">This service is provided by the ASP.NET hosting layer, and can be injected into your startup logic via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="80ce6-174">La plantilla de sitio web de ASP.NET Core en Visual Studio usa este enfoque para cargar los archivos de configuración específicos del entorno (si existe) y para personalizar la configuración de control de errores de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="80ce6-174">The ASP.NET Core web site template in Visual Studio uses this approach to load environment-specific configuration files (if present) and to customize the app's error handling settings.</span></span> <span data-ttu-id="80ce6-175">En ambos casos, este comportamiento se logra consultando el entorno especificado actualmente mediante una llamada a `EnvironmentName` o `IsEnvironment` en la instancia de `IHostingEnvironment` pasada en el método adecuado.</span><span class="sxs-lookup"><span data-stu-id="80ce6-175">In both cases, this behavior is achieved by referring to the currently specified environment by calling `EnvironmentName` or `IsEnvironment` on the instance of `IHostingEnvironment` passed into the appropriate method.</span></span>

> [!NOTE]
> <span data-ttu-id="80ce6-176">Si tiene que comprobar si la aplicación se ejecuta en un entorno concreto, use `env.IsEnvironment("environmentname")` desde correctamente omitirá caso (en lugar de comprobar si `env.EnvironmentName == "Development"` por ejemplo).</span><span class="sxs-lookup"><span data-stu-id="80ce6-176">If you need to check whether the application is running in a particular environment, use `env.IsEnvironment("environmentname")` since it will correctly ignore case (instead of checking if `env.EnvironmentName == "Development"` for example).</span></span>

<span data-ttu-id="80ce6-177">Por ejemplo, puede usar el siguiente código en el método de configuración para configurar el control de errores específicos de entorno:</span><span class="sxs-lookup"><span data-stu-id="80ce6-177">For example, you can use the following code in your Configure method to setup environment specific error handling:</span></span>

[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]

<span data-ttu-id="80ce6-178">Si la aplicación se ejecuta en un `Development` entorno, a continuación, se habilita la compatibilidad en tiempo de ejecución necesaria para utilizar la característica de "BrowserLink" en Visual Studio, las páginas de error específico de desarrollo (que normalmente no se deben ejecutar en producción) y error especiales de la base de datos páginas (que proporcionan una manera de aplicar las migraciones y, por tanto, solo deben usarse en el desarrollo).</span><span class="sxs-lookup"><span data-stu-id="80ce6-178">If the app is running in a `Development` environment, then it enables the runtime support necessary to use the "BrowserLink" feature in Visual Studio, development-specific error pages (which typically should not be run in production) and special database error pages (which provide a way to apply migrations and should therefore only be used in development).</span></span> <span data-ttu-id="80ce6-179">En caso contrario, si la aplicación no se está ejecutando en un entorno de desarrollo, una página de control de errores estándar está configurada para mostrarse en respuesta a las excepciones no controladas.</span><span class="sxs-lookup"><span data-stu-id="80ce6-179">Otherwise, if the app is not running in a development environment, a standard error handling page is configured to be displayed in response to any unhandled exceptions.</span></span>

<span data-ttu-id="80ce6-180">Debe determinar qué contenido para enviar al cliente en tiempo de ejecución, dependiendo del entorno actual.</span><span class="sxs-lookup"><span data-stu-id="80ce6-180">You may need to determine which content to send to the client at runtime, depending on the current environment.</span></span> <span data-ttu-id="80ce6-181">Por ejemplo, en un entorno de desarrollo generalmente actúan no minimiza las secuencias de comandos y hojas de estilos, lo que simplifica la depuración.</span><span class="sxs-lookup"><span data-stu-id="80ce6-181">For example, in a development environment you generally serve non-minimized scripts and style sheets, which makes debugging easier.</span></span> <span data-ttu-id="80ce6-182">Entornos de prueba y producción deben servir las versiones reducidas y genera normalmente a partir de una CDN.</span><span class="sxs-lookup"><span data-stu-id="80ce6-182">Production and test environments should serve the minified versions and generally from a CDN.</span></span> <span data-ttu-id="80ce6-183">Puede hacerlo mediante el entorno de [auxiliar de etiquetas](../mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="80ce6-183">You can do this using the Environment [tag helper](../mvc/views/tag-helpers/intro.md).</span></span> <span data-ttu-id="80ce6-184">La aplicación auxiliar de etiquetas de entorno solo representará su contenido si el entorno actual coincide con uno de los entornos especificados mediante el `names` atributo.</span><span class="sxs-lookup"><span data-stu-id="80ce6-184">The Environment tag helper will only render its contents if the current environment matches one of the environments specified using the `names` attribute.</span></span>

[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]

<span data-ttu-id="80ce6-185">Para empezar a trabajar con el uso de aplicaciones auxiliares de etiquetas en la aplicación, vea [Introducción a las aplicaciones auxiliares de etiquetas](../mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="80ce6-185">To get started with using tag helpers in your application see [Introduction to Tag Helpers](../mvc/views/tag-helpers/intro.md).</span></span>

## <a name="startup-conventions"></a><span data-ttu-id="80ce6-186">Convenciones de inicio</span><span class="sxs-lookup"><span data-stu-id="80ce6-186">Startup conventions</span></span>

<span data-ttu-id="80ce6-187">ASP.NET Core es compatible con un enfoque basado en la convención a la configuración de inicio de la aplicación según el entorno actual.</span><span class="sxs-lookup"><span data-stu-id="80ce6-187">ASP.NET Core supports a convention-based approach to configuring an application's startup based on the current environment.</span></span> <span data-ttu-id="80ce6-188">Mediante programación, puede controlar cómo comportará su aplicación, según a qué entorno está en, lo que le permite crear y administrar sus propias convenciones.</span><span class="sxs-lookup"><span data-stu-id="80ce6-188">You can also programmatically control how your application behaves according to which environment it is in, allowing you to create and manage your own conventions.</span></span>

<span data-ttu-id="80ce6-189">Cuando se inicia una aplicación de ASP.NET Core, el `Startup` clase se utiliza para arrancar la aplicación, cargar su configuración, etc. ([más información acerca del inicio de ASP.NET](startup.md)).</span><span class="sxs-lookup"><span data-stu-id="80ce6-189">When an ASP.NET Core application starts, the `Startup` class is used to bootstrap the application, load its configuration settings, etc. ([learn more about ASP.NET startup](startup.md)).</span></span> <span data-ttu-id="80ce6-190">Sin embargo, si existe una clase denominada `Startup{EnvironmentName}` (por ejemplo `StartupDevelopment`) y el `ASPNETCORE_ENVIRONMENT` variable de entorno coincide con ese nombre, a continuación, que `Startup` clase se utiliza en su lugar.</span><span class="sxs-lookup"><span data-stu-id="80ce6-190">However, if a class exists named `Startup{EnvironmentName}` (for example `StartupDevelopment`), and the `ASPNETCORE_ENVIRONMENT` environment variable matches that name, then that `Startup` class is used instead.</span></span> <span data-ttu-id="80ce6-191">Por lo tanto, puede configurar `Startup` para el desarrollo, pero tiene otro `StartupProduction` que se utilizará cuando se ejecute la aplicación en producción.</span><span class="sxs-lookup"><span data-stu-id="80ce6-191">Thus, you could configure `Startup` for development, but have a separate `StartupProduction` that would be used when the app is run in production.</span></span> <span data-ttu-id="80ce6-192">O viceversa.</span><span class="sxs-lookup"><span data-stu-id="80ce6-192">Or vice versa.</span></span>

> [!NOTE]
> <span data-ttu-id="80ce6-193">Al llamar a `WebHostBuilder.UseStartup<TStartup>()` invalida las secciones de configuración.</span><span class="sxs-lookup"><span data-stu-id="80ce6-193">Calling `WebHostBuilder.UseStartup<TStartup>()` overrides configuration sections.</span></span>

<span data-ttu-id="80ce6-194">Además de usar un totalmente independientes `Startup` clase basándose en el entorno actual, también puede realizar ajustes a cómo la aplicación está configurada en un `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="80ce6-194">In addition to using an entirely separate `Startup` class based on the current environment, you can also make adjustments to how the application is configured within a `Startup` class.</span></span> <span data-ttu-id="80ce6-195">El `Configure()` y `ConfigureServices()` métodos admiten versiones específicas del entorno similar a la `Startup` propia del formulario de clase `Configure{EnvironmentName}()` y `Configure{EnvironmentName}Services()`.</span><span class="sxs-lookup"><span data-stu-id="80ce6-195">The `Configure()` and `ConfigureServices()` methods support environment-specific versions similar to the `Startup` class itself, of the form `Configure{EnvironmentName}()` and `Configure{EnvironmentName}Services()`.</span></span> <span data-ttu-id="80ce6-196">Si se define un método `ConfigureDevelopment()` se llamará en lugar de `Configure()` cuando se establece el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="80ce6-196">If you define a method `ConfigureDevelopment()` it will be called instead of `Configure()` when the environment is set to development.</span></span> <span data-ttu-id="80ce6-197">Del mismo modo, `ConfigureDevelopmentServices()` se denominaría en lugar de `ConfigureServices()` en el mismo entorno.</span><span class="sxs-lookup"><span data-stu-id="80ce6-197">Likewise, `ConfigureDevelopmentServices()` would be called instead of `ConfigureServices()` in the same environment.</span></span>

## <a name="summary"></a><span data-ttu-id="80ce6-198">Resumen</span><span class="sxs-lookup"><span data-stu-id="80ce6-198">Summary</span></span>

<span data-ttu-id="80ce6-199">Núcleo de ASP.NET proporciona una serie de características y las convenciones que permiten a los desarrolladores controlar fácilmente el comportamiento de sus aplicaciones en entornos diferentes.</span><span class="sxs-lookup"><span data-stu-id="80ce6-199">ASP.NET Core provides a number of features and conventions that allow developers to easily control how their applications behave in different environments.</span></span> <span data-ttu-id="80ce6-200">Al publicar una aplicación desde el desarrollo hasta el ensayo a producción, conjunto de variables de entorno correctamente para el entorno de permitir para la optimización de la aplicación para fines de depuración, pruebas o producción, según corresponda.</span><span class="sxs-lookup"><span data-stu-id="80ce6-200">When publishing an application from development to staging to production, environment variables set appropriately for the environment allow for optimization of the application for debugging, testing, or production use, as appropriate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="80ce6-201">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="80ce6-201">Additional Resources</span></span>

* [<span data-ttu-id="80ce6-202">Configuración</span><span class="sxs-lookup"><span data-stu-id="80ce6-202">Configuration</span></span>](xref:fundamentals/configuration/index)

* [<span data-ttu-id="80ce6-203">Introducción a las aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="80ce6-203">Introduction to Tag Helpers</span></span>](../mvc/views/tag-helpers/intro.md)