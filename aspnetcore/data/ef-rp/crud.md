---
title: "Páginas de Razor con EF Core: CRUD (2 de 8)"
author: rick-anderson
description: "Se muestra cómo crear, leer, actualizar y eliminar con EF Core"
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/crud
ms.openlocfilehash: 757aeb713b645cea0fe633b150784184d2d3571e
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2018
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a>Crear, leer, actualizar y eliminar: EF Core con páginas de Razor (2 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) y [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

En este tutorial, se revisa y personaliza el código CRUD (crear, leer, actualizar y eliminar) con scaffolding.

Nota: Para minimizar la complejidad y mantener estos tutoriales centrados en EF Core, en los modelos de página de las páginas de Razor se usa código de EF Core. Algunos desarrolladores usan un patrón de capa o repositorio de servicio para crear una capa de abstracción entre la interfaz de usuario (las páginas de Razor) y la capa de acceso a datos.

En este tutorial, se modifican las páginas de Razor Create, Edit, Delete y Details de la carpeta *Student*.

En el código con scaffolding se usa el modelo siguiente para las páginas Create, Edit y Delete:

* Obtenga y muestre los datos solicitados con el método HTTP GET `OnGetAsync`.
* Guarde los cambios en los datos con el método HTTP POST `OnPostAsync`.

Las páginas Index y Details obtienen y muestran los datos solicitados con el método HTTP GET `OnGetAsync`

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a>Reemplazar SingleOrDefaultAsync por FirstOrDefaultAsync

En el código generado se usa [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) para capturar la entidad solicitada. [FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) es más eficaz para capturar una entidad:

* A menos que el código necesite comprobar que no hay más de una entidad devuelta por la consulta. 
* `SingleOrDefaultAsync` captura más datos y realiza trabajo innecesario.
* `SingleOrDefaultAsync` inicia una excepción si hay más de una entidad que se ajuste a la parte del filtro.
*  `FirstOrDefaultAsync` no inicia una excepción si hay más de una entidad que se ajuste a la parte del filtro.

Reemplace globalmente `SingleOrDefaultAsync` con `FirstOrDefaultAsync`. `SingleOrDefaultAsync` se usa en cinco lugares:

* `OnGetAsync` en la página Details.
* `OnGetAsync` y `OnPostAsync` en las páginas Edit y Delete.

<a name="FindAsync"></a>
### <a name="findasync"></a>FindAsync

En gran parte del código con scaffolding, se puede usar [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) en lugar de `FirstOrDefaultAsync` o `SingleOrDefaultAsync`. 

`FindAsync`:

* Busca una entidad con la clave principal (PK). Si el contexto realiza el seguimiento de una entidad con la clave principal, se devuelve sin una solicitud a la base de datos.
* Es sencillo y conciso.
* Está optimizado para buscar una sola entidad.
* Puede tener ventajas de rendimiento en algunas situaciones, pero rara vez entra en juego para escenarios web normales.
* Usa implícitamente [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) en lugar de [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).
Pero si quiere incluir otras entidades, Find ya no resulta apropiado. Esto significa que puede que necesite descartar Find y cambiar a una consulta cuando la aplicación progrese.

## <a name="customize-the-details-page"></a>Personalizar la página de detalles

Vaya a la página `Pages/Students`. Los vínculos **Edit**, **Details** y **Delete** son generados por la [Aplicación auxiliar de etiquetas delimitadoras](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) del archivo *Pages/Students/Index.cshtml*.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

Haga clic en un vínculo Details. La dirección URL tiene el formato `http://localhost:5000/Students/Details?id=2`. Se pasa Student ID mediante una cadena de consulta (`?id=2`).

Actualice las páginas de Razor Edit, Details y Delete para usar la plantilla de ruta `"{id:int}"`. Cambie la directiva de página de cada una de estas páginas de `@page` a `@page "{id:int}"`.

Una solicitud a la página con la plantilla de ruta "{id:int}" que **no** incluya un valor de ruta entero devolverá un error HTTP 404 (no encontrado). Por ejemplo, `http://localhost:5000/Students/Details` devuelve un error 404. Para que el identificador sea opcional, anexe `?` a la restricción de ruta:

 ```cshtml
@page "{id:int?}"
```

Ejecute la aplicación, haga clic en un vínculo Details y compruebe que la dirección URL pasa el identificador como datos de ruta (`http://localhost:5000/Students/Details/2`).

No cambie globalmente `@page` por `@page "{id:int}"`; esta acción rompería los vínculos a las páginas Home y Create.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Agregar datos relacionados

El código con scaffolding de la página Students Index no incluye la propiedad `Enrollments`. En esta sección, se mostrará el contenido de la colección `Enrollments` en la página Details.

El método `OnGetAsync` de *Pages/Students/Details.cshtml.cs* usa el método `FirstOrDefaultAsync` para recuperar una única entidad `Student`. Agregue el código resaltado siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

Los métodos `Include` y `ThenInclude` hacen que el contexto cargue la propiedad de navegación `Student.Enrollments` y, dentro de cada inscripción, la propiedad de navegación `Enrollment.Course`. Estos métodos se examinan con detalle en el tutorial de lectura de datos relacionados.

El método `AsNoTracking` mejora el rendimiento en casos en los que las entidades devueltas no se actualizan en el contexto actual. `AsNoTracking` se describe posteriormente en este tutorial.

### <a name="display-related-enrollments-on-the-details-page"></a>Mostrar las inscripciones relacionadas en la página Details

Abra *Pages/Students/Details.cshtml*. Agregue el siguiente código resaltado para mostrar una lista de las inscripciones:

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

Si la sangría de código no es correcta después de pegar el código, presione CTRL-K-D para corregirlo.

El código anterior recorre en bucle las entidades de la propiedad de navegación `Enrollments`. Para cada inscripción, se muestra el título del curso y la calificación. El título del curso se recupera de la entidad Course almacenada en la propiedad de navegación `Course` de la entidad Enrollments.

Ejecute la aplicación, haga clic en la pestaña **Students** y después en el vínculo **Details** de un estudiante. Se muestra la lista de cursos y calificaciones para el alumno seleccionado.

## <a name="update-the-create-page"></a>Actualizar la página Create

Actualice el método `OnPostAsync` de *Pages/Students/Create.cshtml.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Examine el código de [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_):

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

En el código anterior, `TryUpdateModelAsync<Student>` intenta actualizar el objeto `emptyStudent` mediante los valores de formulario enviados desde la propiedad [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) del [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0). `TryUpdateModelAsync` solo actualiza las propiedades enumeradas (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

En el ejemplo anterior:

* El segundo argumento (` "student", // Prefix`) es el prefijo que se usa para buscar valores. No distingue mayúsculas de minúsculas.
* Los valores de formulario enviados se convierten a los tipos del modelo `Student` mediante el [enlace de modelos](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>
### <a name="overposting"></a>Publicación excesiva

El uso de `TryUpdateModel` para actualizar campos con valores enviados es un procedimiento recomendado de seguridad porque evita la publicación excesiva. Por ejemplo, suponga que la entidad Student incluye una propiedad `Secret` que esta página web no debe actualizar ni agregar:

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Incluso si la aplicación no tiene un campo `Secret` en la de página de Razor de creación o actualización, un hacker podría establecer el valor de `Secret` mediante publicación excesiva. Un hacker podría usar una herramienta como Fiddler, o bien escribir código de JavaScript, para publicar un valor de formulario `Secret`. El código original no limita los campos que el enlazador de modelos usa cuando crea una instancia Student.

El valor que haya especificado el hacker para el campo de formulario `Secret` se actualiza en la base de datos. En la imagen siguiente se muestra cómo la herramienta Fiddler agrega el campo `Secret` (con el valor "OverPost") a los valores de formulario enviados.

![Campo Secret agregado por Fiddler](../ef-mvc/crud/_static/fiddler.png)

El valor "OverPost" se ha agregado correctamente a la propiedad `Secret` de la fila insertada. El diseñador de aplicaciones no había previsto que la propiedad `Secret` se estableciera con la página Create.

<a name="vm"></a>
### <a name="view-model"></a>Modelo de vista

Normalmente, un modelo de vista contiene un subconjunto de las propiedades incluidas en el modelo que usa la aplicación. El modelo de aplicación se suele denominar modelo de dominio. El modelo de dominio normalmente contiene todas las propiedades requeridas por la entidad correspondiente en la base de datos. El modelo de vista contiene solo las propiedades necesarias para la capa de interfaz de usuario (por ejemplo, la página Create). Además del modelo de vista, en algunas aplicaciones se usa un modelo de enlace o de entrada para pasar datos entre la clase del modelo de página de las páginas de Razor y el explorador. Tenga en cuenta el modelo de vista `Student` siguiente:

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

Los modelos de vista ofrecen una forma alternativa de evitar la publicación excesiva. El modelo de vista contiene solo las propiedades que se van a ver (mostrar) o actualizar.

En el código siguiente se usa el modelo de vista `StudentVM` para crear un alumno:

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

El método [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) establece los valores de este objeto mediante la lectura de otro objeto [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues). `SetValues` usa la coincidencia de nombres de propiedad. No es necesario que el tipo de modelo de vista esté relacionado con el tipo de modelo, basta con que tenga propiedades que coincidan.

El uso de `StudentVM` requiere que se actualice [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) para usar `StudentVM` en lugar de `Student`.

En las páginas de Razor, la clase derivada `PageModel` es el modelo de vista. 

## <a name="update-the-edit-page"></a>Actualizar la página Edit

Actualice el modelo de página para la página Edit:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Los cambios de código son similares a la página Create con algunas excepciones:

* `OnPostAsync` tiene un parámetro `id` opcional.
* El estudiante actual se obtiene de la base de datos, en lugar de crear un estudiante vacío.
* `FirstOrDefaultAsync` se ha reemplazado con [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0). `FindAsync` es una buena elección cuando se selecciona una entidad de la clave principal. Vea [FindAsync](#FindAsync) para obtener más información.

### <a name="test-the-edit-and-create-pages"></a>Probar las páginas Edit y Create

Cree y modifique algunas entidades Student.

## <a name="entity-states"></a>Estados de entidad

El contexto de base de datos realiza el seguimiento de si las entidades en memoria están sincronizadas con sus filas correspondientes en la base de datos. La información de sincronización del contexto de base de datos determina qué ocurre cuando se llama a `SaveChanges`. Por ejemplo, cuando se pasa una nueva entidad al método `Add`, el estado de esa entidad se establece en `Added`. Cuando se llama a `SaveChanges`, el contexto de base de datos emite un comando INSERT de SQL.

Una entidad puede estar en uno de los estados siguientes:

* `Added`: la entidad no existe todavía en la base de datos. El método `SaveChanges` emite una instrucción INSERT.

* `Unchanged`: no es necesario guardar cambios con esta entidad. Una entidad tiene este estado cuando se lee desde la base de datos.

* `Modified`: se han modificado algunos o todos los valores de propiedad de la entidad. El método `SaveChanges` emite una instrucción UPDATE.

* `Deleted`: la entidad se ha marcado para su eliminación. El método `SaveChanges` emite una instrucción DELETE.

* `Detached`: el contexto de base de datos no está realizando el seguimiento de la entidad.

En una aplicación de escritorio, los cambios de estado normalmente se establecen de forma automática. Se lee una entidad, se realizan cambios y el estado de la entidad se cambia automáticamente a `Modified`. La llamada a `SaveChanges` genera una instrucción UPDATE de SQL que solo actualiza las propiedades modificadas.

En una aplicación web, el `DbContext` que lee una entidad y muestra los datos se elimina después de representar una página. Cuando se llama al método `OnPostAsync` de una página, se realiza una nueva solicitud web con una instancia nueva de `DbContext`. Volver a leer la entidad en ese contexto nuevo simula el procesamiento de escritorio.

## <a name="update-the-delete-page"></a>Actualizar la página Delete

En esta sección, se agrega código para implementar un mensaje de error personalizado cuando se produce un error en la llamada a `SaveChanges`. Agregue una cadena para contener los posibles mensajes de error:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Reemplace el método `OnGetAsync` con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

El código anterior contiene el parámetro opcional `saveChangesError`. `saveChangesError` indica si se llamó al método después de un error al eliminar el objeto Student. Es posible que se produzca un error en la operación de eliminación debido a problemas de red transitorios. Los errores de red transitorios son más probables en la nube. `saveChangesError` es false cuando se llama a `OnGetAsync` de la página Delete desde la interfaz de usuario. Cuando `OnPostAsync` llama a `OnGetAsync` (debido a un error en la operación de eliminación), el parámetro `saveChangesError` es true.

### <a name="the-delete-pages-onpostasync-method"></a>El método OnPostAsync de las páginas Delete

Reemplace `OnPostAsync` por el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

En el código anterior se recupera la entidad seleccionada y después se llama al método `Remove` para establecer el estado de la entidad en `Deleted`. Cuando se llama a `SaveChanges`, se genera un comando DELETE de SQL. Si se produce un error en `Remove`:

* Se detecta la excepción de base de datos.
* Se llama al método `OnGetAsync` de las páginas Delete con `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Actualizar la página de Razor Delete

Agregue el siguiente mensaje de error resaltado a la página de Razor Delete.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Pruebe Delete.

## <a name="common-errors"></a>Errores comunes

Student/Home u otros vínculos no funcionan:

Compruebe que la página de Razor contiene la directiva `@page` correcta. Por ejemplo, la página de Razor Student/Home **no** debe contener una plantilla de ruta:

```cshtml
@page "{id:int}"
```

Cada página de Razor debe incluir la directiva `@page`.

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/intro)
[Siguiente](xref:data/ef-rp/sort-filter-page)
