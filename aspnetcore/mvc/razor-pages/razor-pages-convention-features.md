---
title: "Características de convención de aplicación y de ruta de páginas de Razor en ASP.NET Core"
author: guardrex
description: "Vea cómo las características de convención de proveedor de modelos de aplicación y de ruta sirven para controlar el enrutamiento, la detección y el procesamiento de páginas."
manager: wpickett
ms.author: riande
ms.date: 10/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: bf1c895fc972310d5541d0098226d58b8183e320
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>Características de convención de aplicación y de ruta de páginas de Razor en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Aquí encontrará información para saber cómo usar las características de convención de proveedor de modelos de aplicación y de ruta con objeto de controlar el enrutamiento, la detección y el procesamiento en aplicaciones de páginas de Razor. Si necesita configurar rutas de una página personalizadas para páginas concretas, configure el enrutamiento a esas páginas con la [convención AddPageRoute](#configure-a-page-route), descrita más adelante en este tema.

Use la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([cómo descargarla](xref:tutorials/index#how-to-download-a-sample)) para explorar las características descritas en este tema.

| Características | El ejemplo explica cómo... |
| -------- | --------------------------- |
| [Convenciones de modelo de aplicación y ruta](#add-route-and-app-model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Agregar un encabezado y una plantilla de ruta a las páginas de una aplicación. |
| [Convenciones de acción de ruta de página](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Agregar una plantilla de ruta a las páginas de una carpeta y a una sola página. |
| [Convenciones de acción del modelo de página](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (clase de filtro, expresión lambda o fábrica de filtros)</li></ul> | Agregar un encabezado a las páginas de una carpeta, agregar un encabezado a una sola página y configurar una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory) para agregar un encabezado a las páginas de una aplicación. |
| [Proveedor de modelos de aplicación de página predeterminado](#replace-the-default-page-app-model-provider) | Reemplazar el proveedor de modelos de página predeterminado para cambiar las convenciones de nomenclatura de controlador. |

## <a name="add-route-and-app-model-conventions"></a>Agregar convenciones de modelo de aplicación y de ruta

Agregue un delegado de [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) para agregar convenciones de modelo de aplicación y de ruta y aplicarlas a páginas de Razor.

**Agregar una convención de modelo de ruta a todas las páginas**

Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) para crear y agregar un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) a la colección de instancias [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) que se van a aplicar durante la construcción del modelo de página y de ruta.

En la aplicación de ejemplo se agrega una plantilla de ruta `{globalTemplate?}` a todas las páginas de la aplicación:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> La propiedad `Order` de `AttributeRouteModel` está establecida en `0` (cero). De este modo, se garantiza que esta plantilla tiene prioridad para colocarse en la primera posición de valor de datos de ruta cuando se proporcione un solo valor de ruta. Por ejemplo, en el ejemplo se agregará una plantilla de ruta `{aboutTemplate?}` más adentrado este tema. La plantilla `{aboutTemplate?}` tiene un `Order` con un valor de `1`. Cuando la página About se solicita en `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 0`) y no `RouteData.Values["aboutTemplate"]` (`Order = 1`), debido a la configuración de la propiedad `Order`.

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

Solicite la página About del ejemplo en `localhost:5000/About/GlobalRouteValue` y revise el resultado:

![La página About se solicita con un segmento de ruta de GlobalRouteValue. La página presentada refleja que el valor de datos de ruta se captura en el método OnGet de la página.](razor-pages-convention-features/_static/about-page-global-template.png)

**Agregar una convención de modelo de aplicación a todas las páginas**

Use [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) para crear y agregar un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) a la colección de instancias [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) que se van a aplicar durante la construcción del modelo de página y de ruta.

Para demostrar esta y otras convenciones más adelante en este tema, la aplicación de ejemplo incluye una clase `AddHeaderAttribute`. El constructor de esta clase acepta una cadena `name` y una matriz de cadenas `values`. Estos valores se usan en su método `OnResultExecuting` para establecer un encabezado de respuesta. La clase completa se muestra en la sección [Convenciones de acción del modelo de página](#page-model-action-conventions) más adelante en este tema.

En la aplicación de ejemplo se usa la clase `AddHeaderAttribute` para agregar un encabezado (`GlobalHeader`) a todas las páginas de la aplicación:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página About ponen de manifiesto que GlobalHeader se ha agregado.](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a>Convenciones de acción de ruta de página

El proveedor de modelos de ruta predeterminado que se deriva de [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) invoca convenciones diseñadas para proporcionar puntos de extensibilidad que permitan configurar rutas de página.

**Convención de modelo de ruta de carpeta**

Use [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) para crear y agregar un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) que invoque una acción en el modelo [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) de todas las páginas contenidas en la carpeta especificada.

En la aplicación de ejemplo se usa `AddFolderRouteModelConvention` para agregar una plantilla de ruta `{otherPagesTemplate?}` a las páginas de la carpeta *OtherPages*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> La propiedad `Order` de `AttributeRouteModel` está establecida en `1`. Esto garantiza que la plantilla de `{globalTemplate?}` (configurada anteriormente en este tema) tiene prioridad para colocarse en la primera posición de valor de datos de ruta cuando se proporcione un solo valor de ruta. Si la página Page1 se solicita en `/OtherPages/Page1/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 0`) y no `RouteData.Values["otherPagesTemplate"]` (`Order = 1`), debido a la configuración de la propiedad `Order`.

Solicite la página Page1 del ejemplo en `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` y revise el resultado:

![Page1 en la carpeta OtherPages solicitada con un segmento de ruta de GlobalRouteValue y OtherPagesRouteValue La página presentada refleja que los valores de datos de ruta se capturan en el método OnGet de la página.](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**Convención de modelo de ruta de página**

Use [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) para crear y agregar un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) que invoque una acción en el modelo [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) de la página con el nombre especificado.

En la aplicación de ejemplo se usa `AddPageRouteModelConvention` para agregar una plantilla de ruta `{aboutTemplate?}` a la página About:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> La propiedad `Order` de `AttributeRouteModel` está establecida en `1`. Esto garantiza que la plantilla de `{globalTemplate?}` (configurada anteriormente en este tema) tiene prioridad para colocarse en la primera posición de valor de datos de ruta cuando se proporcione un solo valor de ruta. Si la página About se solicita en `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 0`) y no `RouteData.Values["aboutTemplate"]` (`Order = 1`), debido a la configuración de la propiedad `Order`.

Solicite la página About del ejemplo en `localhost:5000/About/GlobalRouteValue/AboutRouteValue` y revise el resultado:

![La página About se solicita con los segmentos de ruta de GlobalRouteValue y AboutRouteValue. La página presentada refleja que los valores de datos de ruta se capturan en el método OnGet de la página.](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Configurar una ruta de página

Use [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) para configurar una ruta a una página en la ruta de acceso de página especificada. Los vínculos generados que apuntan a esa página usan la ruta especificada. `AddPageRoute` usa `AddPageRouteModelConvention` para establecer la ruta.

En la aplicación de ejemplo se crea una ruta a `/TheContactPage` para *Contact.cshtml*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

A la página Contact también se puede llegar en `/Contact` a través de su ruta predeterminada.

La ruta personalizada de la aplicación de ejemplo que lleva a la página Contact tiene cabida para un segmento de ruta `text` opcional (`{text?}`). La página también incluye este segmento opcional en su directiva `@page` por si el usuario que la visita tiene acceso a la página en su ruta `/Contact`:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

Observe que la dirección URL generada para el vínculo **Contact** en la página presentada refleja la ruta actualizada:

![Vínculo Contact de la aplicación de ejemplo en la barra de navegación](razor-pages-convention-features/_static/contact-link.png)

![Si revisamos el vínculo Contact en el HTML presentado, vemos que href está establecido en "/TheContactPage".](razor-pages-convention-features/_static/contact-link-source.png)

Visite la página Contact a través de su ruta normal, `/Contact`, o de la ruta personalizada, `/TheContactPage`. Si se proporciona un segmento de ruta `text` adicional, la página muestra el segmento codificado en HTML que se ha proporcionado:

![Ejemplo del explorador Edge en el que se proporciona un segmento de ruta opcional "text" de "TextValue" en la dirección URL La página presentada muestra el valor del segmento "text".](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Convenciones de acción del modelo de página

El proveedor de modelos de página predeterminado que implementa [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) invoca convenciones que están diseñadas para proporcionar puntos de extensibilidad que permitan configurar modelos de página. Estas convenciones son útiles al crear y modificar características de detección y procesamiento de páginas.

En los ejemplos de esta sección, la aplicación de ejemplo usa una clase `AddHeaderAttribute` (que es un atributo [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute)) que se aplica un encabezado de respuesta:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

Si empleamos convenciones, el ejemplo indica cómo aplicar el atributo a todas las páginas de una carpeta y a una sola página.

**Convención de modelo de aplicación de carpeta**

Use [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) para crear y agregar un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) que invoque una acción en las instancias de [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) de todas las páginas contenidas en la carpeta especificada.

En el ejemplo se demuestra el uso de `AddFolderApplicationModelConvention` agregando un encabezado (`OtherPagesHeader`) a las páginas de la carpeta *OtherPages* de la aplicación:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

Solicite la página Page1 del ejemplo en `localhost:5000/OtherPages/Page1` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página OtherPages/Page1 ponen de manifiesto que OtherPagesHeader se ha agregado.](razor-pages-convention-features/_static/page1-otherpages-header.png)

**Convención de modelo de aplicación de página**

Use [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) para crear y agregar un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) que invoque una acción en el modelo [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) de la página con el nombre especificado.

En el ejemplo se demuestra el uso de `AddPageApplicationModelConvention` agregando un encabezado (`AboutHeader`) a la página About:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página About ponen de manifiesto que AboutHeader se ha agregado.](razor-pages-convention-features/_static/about-page-about-header.png)

**Configurar un filtro**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) permite configurar el filtro que se quiere aplicar. Se puede implementar una clase de filtro, pero en la aplicación de ejemplo se muestra cómo implementar un filtro en una expresión lambda, cosa que sucede en segundo plano como una fábrica que devuelve un filtro:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

El modelo de páginas de aplicación se usa para comprobar la ruta de acceso relativa de los segmentos que conducen a la página Page2 en la carpeta *OtherPages*. Si la condición se supera, se agrega un encabezado. Si no, se aplica `EmptyFilter`.

`EmptyFilter` es un [filtro de acciones](xref:mvc/controllers/filters#action-filters). Las páginas de Razor pasan por alto los filtros de acción, así que `EmptyFilter` no funciona del modo previsto si la ruta de acceso no contiene `OtherPages/Page2`.

Solicite la página Page2 del ejemplo en `localhost:5000/OtherPages/Page2` y revise los encabezados para ver el resultado:

![OtherPagesPage2Header se agrega a la respuesta de Page2.](razor-pages-convention-features/_static/page2-filter-header.png)

**Configurar una fábrica de filtros**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configura la fábrica especificada para aplicar [filtros](xref:mvc/controllers/filters) a todas las páginas de Razor.

En la aplicación de ejemplo se proporciona un ejemplo del uso de una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory), para lo cual se agrega un encabezado (`FilterFactoryHeader`) con dos valores a las páginas de la aplicación:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:

![Los encabezados de respuesta de la página About ponen de manifiesto que se han agregado dos encabezados FilterFactoryHeader.](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Reemplazar el proveedor de modelo de aplicación de página predeterminado

Las páginas de Razor usan la interfaz `IPageApplicationModelProvider` para crear un [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). Puede hacer uso del proveedor de modelo predeterminado para proporcionar su propia lógica de implementación de detección y procesamiento de controladores. La implementación predeterminada ([origen de referencia](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) establece convenciones para la nomenclatura de métodos de control *sin nombre* y *con nombre*, descrita más abajo.

**Métodos de control sin nombre predeterminados**

Los métodos de control de verbos HTTP (métodos de controlador "sin nombre") siguen la convención: `On<HTTP verb>[Async]` (anexar `Async` es opcional, pero recomendable en los métodos asincrónicos).

| Método de control sin nombre     | Operación                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Inicializar el estado de la página     |
| `OnPost`/`OnPostAsync`     | Controlar las solicitudes POST          |
| `OnDelete`/`OnDeleteAsync` | Controlar las solicitudes DELETE&#8224; |
| `OnPut`/`OnPutAsync`       | Controlar las solicitudes PUT&#8224;    |
| `OnPatch`/`OnPatchAsync`   | Controlar las solicitudes PATCH&#8224;  |

&#8224;Usadas para realizar llamadas API a la página.

**Métodos de control con nombre predeterminados**

Los métodos de control proporcionados por el desarrollador (métodos de control "con nombre") siguen una convención similar. El nombre del método de control aparece tras el verbo HTTP o entre el verbo HTTP y `Async`: `On<HTTP verb><handler name>[Async]` (anexar `Async` es opcional, pero recomendable en los métodos asincrónicos). Por ejemplo, los métodos que procesan mensajes pueden asimilar la nomenclatura se muestra en la siguiente tabla.

| Ejemplo de método de control con nombre             | Operación de ejemplo        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Obtener un mensaje        |
| `OnPostMessage`/`OnPostMessageAsync`     | Publicar un mensaje (POST)          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | Eliminar un mensaje (DELETE)&#8224; |
| `OnPutMessage`/`OnPutMessageAsync`       | Colocar un mensaje (PUT)&#8224;    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | Aplicar una revisión a un mensaje (PATCH)#8224;  |

&#8224;Usadas para realizar llamadas API a la página.

**Personalizar los nombres de los métodos de control**

Imaginemos que quiere cambiar la forma en que se denominan los métodos de control con y sin nombre. Un esquema de nomenclatura alternativo consiste en evitar que los nombres de método comiencen por "On" y usar el primer segmento de palabra para establecer el verbo HTTP. Puede realizar otros cambios, como convertir los verbos DELETE, PUT y PATCH en POST. Un esquema de este tipo proporcionaría los nombres de método mostrados en la siguiente tabla.

| Método de control                       | Operación                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Inicializar el estado de la página     |
| `Post`/`PostAsync`                   | Controlar las solicitudes POST          |
| `Delete`/`DeleteAsync`               | Controlar las solicitudes DELETE&#8224; |
| `Put`/`PutAsync`                     | Controlar las solicitudes PUT&#8224;    |
| `Patch`/`PatchAsync`                 | Controlar las solicitudes PATCH&#8224;  |
| `GetMessage`                         | Obtener un mensaje              |
| `PostMessage`/`PostMessageAsync`     | Publicar un mensaje (POST)                |
| `DeleteMessage`/`DeleteMessageAsync` | Publicar (POST) un mensaje para eliminarlo      |
| `PutMessage`/`PutMessageAsync`       | Publicar (POST) un mensaje para colocarlo         |
| `PatchMessage`/`PatchMessageAsync`   | Publicar (POST) un mensaje para aplicarle una revisión       |

&#8224;Usadas para realizar llamadas API a la página.

Para establecer este esquema, haga uso de la clase `DefaultPageApplicationModelProvider` e invalide el método [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) para proporcionar lógica personalizada que resuelva nombres de controlador de [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel). En la aplicación de ejemplo se muestra cómo llevar esto a cabo en su propia clase `CustomPageApplicationModelProvider`:

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Esta clase de caracteriza por lo siguiente:

* Hereda de `DefaultPageApplicationModelProvider`.
* `TryParseHandlerMethod` procesa un método de control para averiguar el verbo HTTP (`httpMethod`) y el nombre del método de control con nombre (`handlerName`) al crear la `PageHandlerModel`.
  * Si lo hay, el postfijo `Async` se omite.
  * Se distingue entre mayúsculas y minúsculas al analizar el verbo HTTP desde el nombre del método.
  * Cuando el nombre del método (sin `Async`) es igual que el nombre del verbo HTTP, no hay ningún método de control con nombre. `handlerName` está establecido en `null` y el nombre del método es `Get`, `Post`, `Delete`, `Put` o `Patch`.
  * Cuando el nombre del método (sin `Async`) es más largo que el nombre del verbo HTTP, sí hay un método de control con nombre. La propiedad `handlerName` se establece como `<method name (less 'Async', if present)>`. Por ejemplo, tanto "GetMessage" como "GetMessageAsync" producen el nombre de método de control "GetMessage".
  * Los verbos HTTP DELETE, PUT y PATCH se convierten en POST.

Registre `CustomPageApplicationModelProvider` en la clase `Startup`:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

El modelo de página en *Index.cshtml.cs* muestra cómo se cambian las convenciones de nomenclatura de métodos de control habituales de las páginas de la aplicación. La nomenclatura con el prefijo "On" típicamente usado en páginas de Razor se quita. El método que inicializa el estado de página ahora se denomina `Get`. Esta convención, usada a lo largo de la aplicación, se puede ver si se abre un modelo de página de cualquiera de las páginas.

El resto de métodos comienzan por el verbo HTTP, que describe su procesamiento. Los dos métodos que empiezan por `Delete` se tratarían normalmente como verbos HTTP DELETE, pero la lógica de `TryParseHandlerMethod` establece explícitamente el verbo POST para ambos métodos de control.

Recuerde que `Async` es opcional entre `DeleteAllMessages` y `DeleteMessageAsync`. Ambos son métodos asincrónicos, pero puede optar entre usar el postfijo `Async` o no. Le recomendamos que lo haga. `DeleteAllMessages` se usa aquí con fines de demostración, pero se recomienda denominar este método `DeleteAllMessagesAsync`. Aunque no afecta al procesamiento de la implementación de ejemplo, usar el postfijo `Async` resalta el hecho de que es un método asincrónico.

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Fíjese en que los nombres de control proporcionados en *Index.cshtml* coinciden con los métodos de control `DeleteAllMessages` y `DeleteMessageAsync`:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async` en el nombre del método de control `DeleteMessageAsync` se descarta por medio de `TryParseHandlerMethod` para que coincida con la solicitud POST al método. El nombre `asp-page-handler` de `DeleteMessage` coincide con el método de control `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtros de MVC y filtro de página (IPageFilter)

Las páginas de Razor no tienen en cuenta los [filtros de acciones](xref:mvc/controllers/filters#action-filters) de MVC, porque en este tipo de páginas se usan métodos de control. Hay disponibles otros tipos de filtro de MVC que puede usar: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters) y [Result](xref:mvc/controllers/filters#result-filters). Para más información, vea el tema [Filters](xref:mvc/controllers/filters) (Filtros).

El filtro de página ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) es un filtro que se aplica a las páginas de Razor. Rodea la ejecución de un método de control de la página. Permite procesar código personalizado en las fases de ejecución del método de control de página. Este es un ejemplo de la aplicación de ejemplo:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

Este filtro busca un valor de ruta de `globalTemplate` de "TriggerValue" y lo intercambia por "ReplacementValue".

El atributo `ReplaceRouteValueFilter` se puede aplicar directamente a un `PageModel`:

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

Solicite la página Page3 de la aplicación de ejemplo en `localhost:5000/OtherPages/Page3/TriggerValue`. Observe cómo el filtro reemplaza el valor de ruta:

![La solicitud a OtherPages/Page3 con un segmento de ruta TriggerValue tiene como resultado que el filtro reemplaza el valor de ruta por ReplacementValue.](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a>Vea también

* [Convenciones de autorización de las páginas de Razor](xref:security/authorization/razor-pages-authorization)
