---
title: "Agregar una búsqueda"
author: rick-anderson
description: "Se muestra cómo agregar la búsqueda a una aplicación sencilla de ASP.NET Core MVC"
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: e237b432e411faf6e8a1fe8c907c5daaf6eeef9e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

Nota: SQLlite distingue mayúsculas de minúsculas, por lo que tendrá que buscar "Ghost" y no "ghost".

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

Cambie la etiqueta `<form>` en la vista de Razor *Views\movie\Index.cshtml* para especificar `method="get"`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[Anterior: Vistas y métodos de controlador](controller-methods-views.md)
[Siguiente: Agregar un campo](new-field.md)  
