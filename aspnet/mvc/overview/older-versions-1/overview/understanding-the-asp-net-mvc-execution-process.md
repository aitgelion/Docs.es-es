---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: "Descripción del proceso de ejecución de ASP.NET MVC | Documentos de Microsoft"
author: microsoft
description: "Obtenga información acerca de cómo el marco de MVC de ASP.NET procesa una solicitud de explorador paso a paso."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Descripción del proceso de ejecución de ASP.NET MVC
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información acerca de cómo el marco de MVC de ASP.NET procesa una solicitud de explorador paso a paso.


Las solicitudes a una aplicación Web basada en ASP.NET MVC atravesar primero el **UrlRoutingModule** objeto, que es un módulo HTTP. Este módulo analiza la solicitud y realiza la selección de la ruta. El **UrlRoutingModule** objeto selecciona el primer objeto de ruta que coincida con la solicitud actual. (Un objeto de ruta es una clase que implementa **RouteBase**, y normalmente es una instancia de la **ruta** clase.) Si ninguna ruta coincide, el **UrlRoutingModule** objeto no hace nada y permite que la solicitud recurra a la solicitud ASP.NET o IIS normal al procesamiento.

Desde seleccionado **ruta** objeto, el **UrlRoutingModule** objeto obtiene la **IRouteHandler** objeto que está asociado el **ruta**objeto. Normalmente, en una aplicación MVC, será una instancia de **MvcRouteHandler**. El **IRouteHandler** instancia crea un **IHttpHandler** objeto y lo pasa la **IHttpContext** objeto. De forma predeterminada, el **IHttpHandler** instancia para MVC es el **MvcHandler** objeto. El **MvcHandler** objeto, a continuación, selecciona el controlador que en última instancia controlará la solicitud.

> [!NOTE]
> Cuando se ejecuta una aplicación Web de MVC de ASP.NET en IIS 7.0, extensión de nombre de archivo no es necesaria para los proyectos MVC. Sin embargo, en IIS 6.0, el controlador requiere que asigne la extensión de nombre de archivo .mvc a la DLL de ISAPI de ASP.NET.


El módulo y el controlador son los puntos de entrada para el marco de MVC de ASP.NET. Realizan las siguientes acciones:

- Seleccionar el controlador adecuado en una aplicación Web de MVC.
- Obtener una instancia de un controlador específico.
- Llamadas del controlador **Execute** método.

A continuación enumeran las fases de ejecución de un proyecto Web de MVC:

- Recibir la primera solicitud para la aplicación 

    - En el archivo Global.asax, **ruta** se agregan objetos a la **RouteTable** objeto.
- Realizar el enrutamiento 

    - El **UrlRoutingModule** módulo usa la primera coincidencia **ruta** objeto en el **RouteTable** colección para crear el **RouteData** objeto, que después se usa para crear un **RequestContext** (**IHttpContext**) objeto.
- Crear el controlador de solicitudes MVC 

    - El **MvcRouteHandler** objeto crea una instancia de la **MvcHandler** clase y le pasa el **RequestContext** instancia.
- Crear controlador 

    - El **MvcHandler** de objeto usa la **RequestContext** instancia para identificar la **IControllerFactory** objeto (normalmente una instancia de la  **DefaultControllerFactory** clase) para crear la instancia del controlador.
- Controlador Execute: el **MvcHandler** instancia invoca el controlador de s **Execute** método. |
- Invocar acción 

    - Mayoría de los controladores que se hereda de la **controlador** clase base. Para los controladores que hacerlo, el **ControllerActionInvoker** objeto que está asociado con el controlador determina qué método de acción de la clase de controlador va a llamar y, a continuación, llama a ese método.
- Ejecutar el resultado 

    - Un método de acción típico podría recibir proporcionados por el usuario, preparar los datos de respuesta adecuados y, a continuación, ejecute el resultado devolviendo un tipo de resultado. Los tipos de resultado integrados que se pueden ejecutar incluyen los siguientes: **ViewResult** (que presenta una vista y es el tipo de resultado utilizado con mayor frecuencia), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, y **EmptyResult**.
