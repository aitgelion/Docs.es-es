---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: Crear clases de modelo con LINQ to SQL (VB) | Documentos de Microsoft
author: microsoft
description: "El objetivo de este tutorial es explicar un método de creación de clases del modelo para una aplicación ASP.NET MVC. En este tutorial, aprenderá a crear modelo c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 972d5b11049825e84e070ef1c4b2b90116654397
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-model-classes-with-linq-to-sql-vb"></a>Crear clases de modelo con LINQ to SQL (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> El objetivo de este tutorial es explicar un método de creación de clases del modelo para una aplicación ASP.NET MVC. En este tutorial, aprenderá cómo crear clases de modelo y realizar el acceso de la base de datos aprovechando las ventajas de Microsoft LINQ to SQL.


El objetivo de este tutorial es explicar un método de creación de clases del modelo para una aplicación ASP.NET MVC. En este tutorial, aprenderá cómo crear clases de modelo y realizar el acceso de la base de datos aprovechando las ventajas de Microsoft LINQ to SQL.

En este tutorial, creamos una aplicación básica de la base de datos de película. Comenzamos creando la aplicación de base de datos de la película de la manera más rápida y fácil posible. Llevamos a cabo todo el acceso a los datos directamente desde las acciones del controlador.

A continuación, se muestra cómo utilizar el modelo de repositorio. Uso del patrón de repositorio, requiere un poco más trabajo. Sin embargo, la ventaja de adoptar este patrón es que le permite crear aplicaciones que son adaptables a cambiar y se puede probar fácilmente.

## <a name="what-is-a-model-class"></a>¿Qué es una clase de modelo?

Un modelo MVC contiene toda la lógica de aplicación que no se encuentra en una vista de MVC o el controlador de MVC. En concreto, un modelo MVC contiene todos los negocios de la aplicación y la lógica de acceso a datos.

Puede utilizar una variedad de tecnologías diferentes para implementar la lógica de acceso a datos. Por ejemplo, puede generar las clases de acceso a datos utilizando las clases de Microsoft Entity Framework, NHibernate, Subsonic o ADO.NET.

En este tutorial, usar LINQ to SQL para consultar y actualizar la base de datos. LINQ to SQL proporciona un método muy fácil de interactuar con una base de datos de Microsoft SQL Server. Sin embargo, es importante comprender que el marco de MVC de ASP.NET no está asociado con LINQ to SQL de ninguna manera. ASP.NET MVC es compatible con cualquier tecnología de acceso a datos.

## <a name="create-a-movie-database"></a>Crear una base de datos de la película

En este tutorial, para ilustrar cómo puede crear clases de modelo, creamos una sencilla aplicación de base de datos de la película. El primer paso es crear una nueva base de datos. Haga clic en la aplicación\_carpeta de datos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento**. Seleccione la plantilla de la base de datos de SQL Server, asígnele el nombre MoviesDB.mdf y haga clic en el **agregar** botón (consulte la figura 1).


[![Agregar una nueva base de datos de SQL Server](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Figura 01**: agregar una nueva base de datos de SQL Server ([haga clic aquí para ver la imagen a tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))


Después de crear la nueva base de datos, puede abrir la base de datos haciendo doble clic en el archivo MoviesDB.mdf en la aplicación\_carpeta de datos. Haga doble clic en el archivo MoviesDB.mdf se abre la ventana Explorador de servidores (consulte la figura 2).

|  | La ventana Explorador de servidores se llama a la ventana del explorador de base de datos cuando se utiliza Visual Web Developer. |
| --- | --- |


[![Mediante la ventana del explorador de servidores](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Figura 02**: mediante la ventana del explorador de servidores ([haga clic aquí para ver la imagen a tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))


Tenemos que agregar una tabla a nuestra base de datos que representa nuestro películas. Haga clic en la carpeta de tablas y seleccione la opción de menú **agregar nueva tabla**. Al seleccionar esta opción de menú abre el Diseñador de tablas (consulte la figura 3).


[![Mediante la ventana del explorador de servidores](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Figura 03**: el Diseñador de tablas ([haga clic aquí para ver la imagen a tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))


Tenemos que agregar las columnas siguientes a la tabla de base de datos:

| **Nombre de columna** | **Tipo de datos** | **Permitir valores null** |
| --- | --- | --- |
| Id. | Valor int. | False |
| Título | Nvarchar (200) | False |
| Director de | nvarchar (50) | False |

Debe hacer dos cosas especial a la columna de identificador. En primer lugar, debe marcar la columna Id. como una columna de clave principal, seleccionando la columna en el Diseñador de tablas y haga clic en el icono de una clave. LINQ to SQL requiere especificar las columnas de clave principales al realizar inserta o actualiza la base de datos.

A continuación, debe marcar la columna de identificador como una columna de identidad asignando el valor Sí a la **identidad** propiedad (consulte la figura 3). Una columna de identidad es una columna que se asigna a un nuevo número automáticamente cada vez que agregue una nueva fila de datos a una tabla.

Después de realizar estos cambios, guarde la tabla con el nombre tblMovie. Puede guardar la tabla haciendo clic en el botón Guardar.

## <a name="create-linq-to-sql-classes"></a>Crear clases LINQ to SQL

Nuestro modelo MVC contiene LINQ a las clases SQL que representan la tabla de base de datos de tblMovie. La manera más fácil para crear estas clases de LINQ to SQL es haga clic en la carpeta Models, seleccione **agregar, nuevo elemento**, seleccione la plantilla de LINQ to SQL Classes, asigne el nombre Movie.dbml a las clases y haga clic en el **agregar**botón (consulte la figura 4).


[![Creación de LINQ a las clases SQL](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Figura 04**: crear clases de LINQ to SQL ([haga clic aquí para ver la imagen a tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))


Inmediatamente después de crear la película clases de LINQ to SQL, aparece el Object Relational Designer. Puede arrastrar las tablas de base de datos desde la ventana Explorador de servidores a Object Relational Designer para crear clases LINQ to SQL que representan tablas de base de datos determinada. Tenemos que agregar la tabla de base de datos de tblMovie en Object Relational Designer (consulte la figura 4).


[![Uso de Object Relational Designer](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Figura 05**: con Object Relational Designer ([haga clic aquí para ver la imagen a tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))


De forma predeterminada, el Object Relational Designer crea una clase con el mismo muy nombre que la tabla de base de datos que arrastre al diseñador. Sin embargo, no debe llamar a nuestro tblMovie de clase. Por lo tanto, haga clic en el nombre de la clase en el diseñador y cambie el nombre de la clase a la película.

Por último, recuerde que hacer clic en el **guardar** botón (la imagen del disquete) para guardar LINQ to SQL Classes. De lo contrario, las clases de LINQ to SQL no se generará por Object Relational Designer.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Usar LINQ to SQL en una acción de controlador

Ahora que tenemos nuestras clases de LINQ to SQL, podemos usar estas clases para recuperar datos de la base de datos. En esta sección, aprenderá a usar LINQ a las clases SQL directamente dentro de una acción de controlador. Se mostrará la lista de películas de la tabla de base de datos de tblMovies en una vista de MVC.

En primer lugar, es necesario modificar la clase HomeController. Esta clase puede encontrarse en la carpeta de controladores de la aplicación. Modifique la clase para que aparezca la clase en la lista 1.

**Lista 1:`Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

La acción de Index() en el listado 1 utiliza una clase de LINQ to SQL DataContext (el MovieDataContext) para representar la base de datos de MoviesDB. La clase MoveDataContext lo generó Visual Studio Object Relational Designer.

Se realiza una consulta LINQ en DataContext para recuperar todas las películas de la tabla de base de datos de tblMovies. La lista de películas se asigna a una variable local denominada películas. Por último, la lista de películas se pasa a la vista a través de los datos de vista.

Para mostrar las películas, a continuación es necesario modificar la vista de índice. Puede encontrar la vista de índice en la carpeta Views\Home\. Actualizar la vista de índice para que tenga un aspecto similar a la vista en el listado 2.

**La lista 2:`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Tenga en cuenta que la vista de índice modificada incluye un &lt;% @ import % de espacio de nombres&gt; la directiva en la parte superior de la vista. Esta directiva importa el espacio de nombres MvcApplication1. Este espacio de nombres es necesario para poder trabajar con las clases del modelo: en concreto, las clases de película, en la vista.

La vista en el listado 2 contiene For cada bucle que recorre en iteración todos los elementos representados por la propiedad ViewData.Model. El valor de la propiedad Title se muestra para cada película.

Tenga en cuenta que el valor de la propiedad ViewData.Model se convierte en un objeto IEnumerable. Esto es necesario para recorrer el contenido de ViewData.Model. Otra opción es crear una vista fuertemente tipada. Cuando se crea una vista fuertemente tipada, convierte la propiedad ViewData.Model a un tipo determinado en la clase de código subyacente de la vista.

Si se ejecuta la aplicación después de modificar la clase HomeController y la vista de índice, se producirán una página en blanco. Obtendrá una página en blanco porque no hay ningún registro de la película en la tabla de base de datos de tblMovies.

Para poder agregar registros a la tabla de base de datos de tblMovies, haga clic en la tabla de base de datos de tblMovies en la ventana Explorador de servidores (ventana de explorador de base de datos en Visual Web Developer) y seleccione la opción de menú **mostrar datos de tabla**. Puede insertar registros de película mediante el uso de la cuadrícula que aparece (Véase la figura 5).


[![Inserción de películas](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Figura 06**: insertar películas ([haga clic aquí para ver la imagen a tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))


Después de agregar algunas entradas de la base de datos a la tabla tblMovies y ejecute la aplicación, verá la página en la figura 7. Todos los registros de base de datos de la película se muestran en una lista con viñetas.


[![Mostrar películas con la vista de índice](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Figura 07**: mostrar películas con la vista de índice ([haga clic aquí para ver la imagen a tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Utilizando el modelo de repositorio

En la sección anterior, hemos usado LINQ a las clases SQL directamente dentro de una acción de controlador. Se usa la clase MovieDataContext directamente desde la acción del controlador Index(). No hay ningún problema con esto en el caso de una aplicación sencilla. Sin embargo, para trabajar directamente con LINQ to SQL en una clase de controlador crea problemas cuando se necesita para crear una aplicación más compleja.

Usar LINQ to SQL dentro de una clase de controlador hace difícil cambiar tecnologías de acceso a datos en el futuro. Por ejemplo, podría decidir cambia de utilizar Microsoft LINQ to SQL con Microsoft Entity Framework como la tecnología de acceso a datos. En ese caso, necesitaría volver a escribir todos los controladores que tiene acceso a la base de datos dentro de la aplicación.

Usar LINQ to SQL dentro de una clase de controlador también dificulta generar pruebas unitarias para la aplicación. Normalmente, no desea interactuar con una base de datos al realizar pruebas unitarias. Desea usar las pruebas unitarias para probar la lógica de aplicación y no el servidor de base de datos.

Para compilar una aplicación MVC es más adaptables para futuros cambios y que puede probar con mayor facilidad, puede utilizar el modelo de repositorio. Cuando se usa el modelo de repositorio, cree una clase de repositorio independiente que contiene toda la lógica de acceso de la base de datos.

Cuando se crea la clase de repositorio, crear una interfaz que representa todos los métodos utilizados por la clase de repositorio. Dentro de los controladores, escribir el código con la interfaz en lugar del repositorio. De este modo, puede implementar el repositorio mediante tecnologías de acceso a datos diferentes en el futuro.

La interfaz en el listado 3 se denomina IMovieRepository y representa un método único denominado ListAll().

**Enumerar 3:`Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

La clase de repositorio en el listado 4 implementa la interfaz IMovieRepository. Observe que contiene un método denominado ListAll() que se corresponde con el método requerido por la interfaz IMovieRepository.

**Enumerar 4:`Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Finalmente, la clase MoviesController en el listado 5 usa el modelo de repositorio. Ya no se utiliza LINQ a clases SQL directamente.

**Enumerar 5:`Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Tenga en cuenta que la clase MoviesController en el listado 5 tiene dos constructores. El primer constructor, el constructor sin parámetros, se llama cuando se ejecuta la aplicación. Este constructor crea una instancia de la clase MovieRepository y lo pasa al constructor de segundo.

El segundo constructor con un único parámetro: un parámetro IMovieRepository. Este constructor simplemente asigna el valor del parámetro a un campo de nivel de clase denominado \_repositorio.

La clase MoviesController es aprovechar un modelo de diseño de software denominado el modelo de inserción de dependencias. En concreto, que está usando lo que se denomina inyección de dependencia de Constructor. Puede leer más sobre este patrón, lea el artículo siguiente de Martin Fowler:

[http://martinfowler.com/articles/Injection.HTML](http://martinfowler.com/articles/injection.html)

Tenga en cuenta que todo el código de la clase MoviesController (salvo el primer constructor) interactúa con la interfaz de IMovieRepository en lugar de la clase MovieRepository real. El código interactúa con una interfaz abstracta en lugar de una implementación concreta de la interfaz.

Si desea modificar la tecnología de acceso a datos utilizada por la aplicación simplemente puede implementar la interfaz IMovieRepository con una clase que usa la tecnología de acceso de base de datos alternativo. Por ejemplo, podría crear una clase EntityFrameworkMovieRepository o una clase SubSonicMovieRepository. Dado que la clase de controlador se programa con la interfaz, puede pasar una nueva implementación de IMovieRepository a la clase de controlador y la clase seguiría funcionando.

Además, si desea probar la clase MoviesController, puede pasar una clase de repositorio de película falsa para la MoviesController. Puede implementar la clase IMovieRepository con una clase que no tienen acceso realmente la base de datos, pero que contenga todos los métodos necesarios de la interfaz IMovieRepository. De este modo, puede prueba unitaria de la clase MoviesController sin realmente tener acceso a una base de datos real.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era demostrar cómo puede crear clases de modelo MVC aprovechando las ventajas de Microsoft LINQ to SQL. Se examinan dos estrategias para mostrar datos de la base de datos en una aplicación ASP.NET MVC. En primer lugar, se crea LINQ a las clases SQL y se utilizan las clases directamente dentro de una acción de controlador. Uso de LINQ para las clases dentro de un controlador de SQL Server le permite rápidamente y mostrar fácilmente los datos de la base de datos en una aplicación MVC.

A continuación, se explora una ruta de acceso un poco más difícil, pero sin duda alguna más virtuoso, para mostrar datos de la base de datos. Se tardó aprovechar el modelo de repositorio y colocan todos nuestra lógica de acceso de la base de datos en una clase independiente de repositorio. En nuestro controlador, indicamos a todo el código en una interfaz en lugar de una clase concreta. La ventaja del modelo de repositorio es que permite cambiar fácilmente las tecnologías de acceso de base de datos en el futuro y que nos permite probar fácilmente las clases de controlador.

>[!div class="step-by-step"]
[Anterior](creating-model-classes-with-the-entity-framework-vb.md)
[Siguiente](displaying-a-table-of-database-data-vb.md)
