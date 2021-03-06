---
title: "Adición de un nuevo campo a una página de Razor"
author: rick-anderson
description: "Muestra cómo agregar un nuevo campo a una página de Razor con Entity Framework Core"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 36412e9d1f3143f0d1999d0e754e6627f0984ad5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-new-field-to-a-razor-page"></a>Adición de un nuevo campo a una página de Razor

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En esta sección se usa Migraciones de [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First para agregar un nuevo campo al modelo y migrar ese cambio a la base de datos.

Cuando se usa EF Code First para crear una base de datos de forma automática, Code First agrega una tabla a la base de datos para ayudar a saber si el esquema de la base de datos está sincronizado con las clases del modelo a partir del que se ha generado. Si no está sincronizado, EF produce una excepción. Esto facilita la detección de problemas de código o base de datos incoherentes.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Abra el archivo *Models/Movie.cs* y agregue una propiedad `Rating`:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

Compile la aplicación (Ctrl + Mayús + B).

Edite *Pages/Movies/Index.cshtml* y agregue un campo `Rating`:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

Agregue el campo `Rating` a las páginas Delete y Details.

Actualice *Create.cshtml* con un campo `Rating`. Puede copiar o pegar el elemento `<div>` anterior y permitir que IntelliSense le ayude a actualizar los campos. IntelliSense funciona con [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).

![El desarrollador ha escrito la letra R para el valor del atributo de asp-for en el segundo elemento de etiqueta de la vista. Ha aparecido un menú contextual de IntelliSense que muestra los campos disponibles, incluido Rating, que se resalta en la lista automáticamente. Cuando el programador hace clic en el campo o presiona Entrar en el teclado, el valor se establece en Rating.](new-field/_static/cr.png)

El código siguiente muestra *Create.cshtml* con un campo `Rating`:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

Agregue el campo `Rating` a la página de edición.

La aplicación no funciona hasta que la base de datos se actualiza para incluir el nuevo campo. Si se ejecuta ahora, la aplicación produce una `SqlException`:

```
SqlException: Invalid column name 'Rating'.
```

Este error se debe a que la clase del modelo Movie actualizado es diferente al esquema de la tabla Movie de la base de datos. (No hay ninguna columna `Rating` en la tabla de la base de datos).

Este error se puede resolver de varias maneras:

1. Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear con el nuevo esquema de la clase del modelo. Este enfoque resulta conveniente al principio del ciclo de desarrollo; permite desarrollar a la vez y de manera rápida el esquema del modelo y la base de datos. El inconveniente es que se pierden los datos existentes en la base de datos. No use este enfoque en una base de datos de producción. El quitar la base de datos en los cambios de esquema y el usar un inicializador para inicializar automáticamente la base de datos con datos de prueba suele ser una forma productiva de desarrollar una aplicación.

2. Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.

3. Use Migraciones de Code First para actualizar el esquema de la base de datos.

Para este tutorial, use Migraciones de Code First.

Actualice la clase `SeedData` para que proporcione un valor para la nueva columna. A continuación se muestra un cambio de ejemplo, aunque es conveniente realizar este cambio para cada bloque `new Movie`.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

Vea el [archivo completado SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).

Compile la solución.

<a name="pmc"></a> En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.
En PCM, escriba los siguientes comandos:

```powershell
Add-Migration Rating
Update-Database
```

El comando `Add-Migration` indica al marco de trabajo que:

* Compare el modelo `Movie` con el esquema de base de datos `Movie`.
* Cree código para migrar el esquema de la base de datos al nuevo modelo.

El nombre "Rating" es arbitrario y se usa para asignar nombre al archivo de migración. Resulta útil emplear un nombre descriptivo para el archivo de migración.

<a name="ssox"></a> Si elimina todos los registros de la base de datos, el inicializador inicializa la base de datos e incluye el campo `Rating`. Puede hacerlo con los vínculos de eliminación en el explorador o desde el [Explorador de objetos de SQL Server](xref:tutorials/razor-pages/sql#ssox) (SSOX). Para eliminar la base de datos desde SSOX:

* Seleccione la base de datos en SSOX.
* Haga clic con el botón derecho en la base de datos y seleccione *Eliminar*.
* Active **Cerrar las conexiones existentes**.
* Seleccione **Aceptar**.
* En la [PMC](xref:tutorials/razor-pages/new-field#pmc), actualice la base de datos:

  ```powershell
  Update-Database
  ```

Ejecute la aplicación y compruebe que puede crear, editar o mostrar películas con un campo `Rating`. Si la base de datos no está inicializada, detenga IIS Express y luego ejecute la aplicación.

>[!div class="step-by-step"]
[Anterior: Adición de búsqueda](xref:tutorials/razor-pages/search)
[Siguiente: Adición de una validación](xref:tutorials/razor-pages/validation)
