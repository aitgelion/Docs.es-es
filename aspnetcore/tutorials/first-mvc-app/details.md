---
title: "Inspección de los métodos Details y Delete"
author: rick-anderson
description: "La vista y el método de controlador Details en una aplicación básica de ASP.NET Core MVC."
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 4a0004fc79f8e1d334e3acb96b28b2954d19f0a1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="examining-the-details-and-delete-methods"></a>Inspección de los métodos Details y Delete

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Abra el controlador Movie y examine el método `Details`:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

El motor de scaffolding de MVC que creó este método de acción agrega un comentario en el que se muestra una solicitud HTTP que invoca el método. En este caso se trata de una solicitud GET con tres segmentos de dirección URL, el controlador `Movies`, el método `Details` y un valor `id`. Recuerde que estos segmentos se definen en *Startup.cs*.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

EF facilita el proceso de búsqueda de datos mediante el método `SingleOrDefaultAsync`. Una característica de seguridad importante integrada en el método es que el código comprueba que el método de búsqueda haya encontrado una película antes de intentar hacer nada con ella. Por ejemplo, un pirata informático podría introducir errores en el sitio cambiando la dirección URL creada por los vínculos de `http://localhost:xxxx/Movies/Details/1` a algo parecido a `http://localhost:xxxx/Movies/Details/12345` (o algún otro valor que no represente una película real). Si no comprobara una película null, la aplicación generaría una excepción.

Examine los métodos `Delete` y `DeleteConfirmed`.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

Tenga en cuenta que el método `HTTP GET Delete` no elimina la película especificada, sino que devuelve una vista de la película donde puede enviar (HttpPost) la eliminación. La acción de efectuar una operación de eliminación en respuesta a una solicitud GET (o con este propósito efectuar una operación de edición, creación o cualquier otra operación que modifique los datos) genera una vulnerabilidad de seguridad.

El método `[HttpPost]` que elimina los datos se denomina `DeleteConfirmed` para proporcionar al método HTTP POST una firma o nombre únicos. Las dos firmas de método se muestran a continuación:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


Common Language Runtime (CLR) requiere métodos sobrecargados para disponer de una firma de parámetro única (mismo nombre de método, pero lista de parámetros diferente). En cambio, aquí necesita dos métodos `Delete` (uno para GET y otro para POST) que tienen la misma firma de parámetro (ambos deben aceptar un número entero como parámetro).

Hay dos enfoques para este problema. Uno consiste en proporcionar nombres diferentes a los métodos, que es lo que hizo el mecanismo de scaffolding en el ejemplo anterior. Pero esto implica un pequeño problema: ASP.NET asigna segmentos de una dirección URL a los métodos de acción por nombre y, si cambia el nombre de un método, normalmente el enrutamiento no podría encontrar ese método. La solución es la que ve en el ejemplo, que consiste en agregar el atributo `ActionName("Delete")` al método `DeleteConfirmed`. Ese atributo efectúa la asignación para el sistema de enrutamiento para que una dirección URL que incluya /Delete/ para una solicitud POST busque el método `DeleteConfirmed`.

Otra solución alternativa común para los métodos que tienen nombres y firmas idénticos consiste en cambiar la firma del método POST artificialmente para incluir un parámetro adicional (sin usar). Es lo que hicimos en una publicación anterior, cuando agregamos el parámetro `notUsed`. Podría hacer lo mismo aquí para el método `[HttpPost] Delete`:

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a>Publicar en Azure

Vea [Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obtener instrucciones sobre cómo publicar esta aplicación en Azure con Visual Studio.  También se puede publicar la aplicación desde la [línea de comandos](xref:tutorials/publish-to-azure-webapp-using-cli).

Gracias por seguir esta introducción a ASP.NET Core MVC. Le agradeceremos todos los comentarios que quiera hacernos. [Introducción a MVC y EF Core](xref:data/ef-mvc/intro) es un excelente seguimiento de este tutorial.

>[!div class="step-by-step"]
[Anterior](validation.md)
