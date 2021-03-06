---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: "Conceptos básicos de ASP.NET MVC 4 | Documentos de Microsoft"
author: rick-anderson
description: "Este laboratorio práctico se basa en la tienda de música MVC (Model View Controller), una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MV..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 468f6d5dabb645b1c005680dc5a1ffc4debd63b6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-fundamentals"></a>Conceptos básicos de ASP.NET MVC 4
====================
por [Web colonias equipo](https://twitter.com/webcamps)

> Este laboratorio práctico se basa en la tienda de música MVC (Model View Controller), una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio. En el laboratorio, aprenderá la simplicidad, todavía power de usar estas tecnologías conjuntamente. Se iniciará con una aplicación sencilla y compilará hasta que haya una aplicación de Web de ASP.NET MVC 4 totalmente funcional.
> 
> Esta práctica funciona con ASP.NET MVC 4.
> 
> Si desea explorar la versión de ASP.NET MVC 3 de la aplicación del tutorial, puede encontrarlo en [tienda de música de MVC](https://github.com/evilDave/MVC-Music-Store).
> 
> > [!NOTE]
> > Este laboratorio práctico se da por supuesto que el desarrollador tiene experiencia en tecnologías de desarrollo Web, como HTML y JavaScript.
> 
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).


<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>La aplicación de tienda de música

La aplicación web de tienda de música que se generarán a lo largo de este laboratorio consta de tres partes principales: la compra, la desprotección y administración. Los visitantes podrán examinar álbumes por género, agregar álbumes a su carro, revise su selección y por último, ir a la caja para iniciar sesión y completar el pedido. Además, los administradores del almacén podrá administrar los álbumes disponibles, así como sus propiedades principales.

![Pantallas de la tienda de música](aspnet-mvc-4-fundamentals/_static/image1.png "pantallas de la tienda de música")

*Pantallas de la tienda de música*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>Essentials de ASP.NET MVC 4

Aplicación de la tienda de música se generarán utilizando **Model View Controller (MVC)**, un modelo de arquitectura que separa una aplicación en tres componentes principales:

- **Modelos**: objetos de modelo son las partes de la aplicación que implementan la lógica del dominio. A menudo, los objetos del modelo también recuperar y almacenan el estado del modelo en una base de datos.
- **Vistas:** vistas son los componentes que muestra la interfaz de usuario (UI) de la aplicación. Normalmente, esta interfaz de usuario se crea a partir de los datos del modelo. Un ejemplo sería la vista de edición de álbumes que muestra cuadros de texto y una lista desplegable en función del estado actual de un objeto de álbum.
- **Controladores:** controladores son los componentes que controlen la interacción del usuario, manipulan el modelo y por último seleccionan una vista para representar la interfaz de usuario. En una aplicación MVC, la vista solo muestra información; el controlador administra y responde a los proporcionados por el usuario y la interacción.

El modelo de MVC le ayuda a crear aplicaciones que separan los diferentes aspectos de la aplicación (lógica de entrada, lógica comercial y lógica de la interfaz de usuario), y proporciona un vago acoplamiento entre estos elementos. Esta separación le ayuda a administrar la complejidad al compilar una aplicación, ya que permite centrarse en un aspecto de la implementación a la vez. Además, el modelo de MVC facilita probar aplicaciones, también fomenta el uso de desarrollo controlado por pruebas (TDD) para crear aplicaciones.

El **ASP.NET MVC** framework proporciona una alternativa al modelo de formularios Web Forms de ASP.NET para crear aplicaciones Web basadas en ASP.NET MVC. El **ASP.NET MVC** framework es un marco de presentación ligera, alta comprobable que (al igual que con las aplicaciones basadas en formularios Web) se integra con las características de ASP.NET existentes, como páginas maestras y basada en pertenencia autenticación de modo que lleguen todas las posibilidades de .NET framework core. Esto es útil si ya está familiarizado con ASP.NET Web Forms porque todas las bibliotecas que ya usa también están disponibles en ASP.NET MVC 4.

Además, el acoplamiento flexible entre los tres componentes principales de una aplicación MVC también favorece el desarrollo paralelo. Por ejemplo, un desarrollador de software puede trabajar en la vista, un segundo desarrollador puede trabajar en la lógica del controlador y una tercera puede Centrar en la lógica de negocios en el modelo.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Crear una aplicación de ASP.NET MVC desde el principio, basado en el tutorial de la aplicación de la tienda de música
- Agregar controladores para controlar las direcciones URL a la página principal del sitio y para examinar su funcionalidad principal
- Agregar una vista para personalizar el contenido que se muestra junto con su estilo
- Agregar clases de modelo para contener y administrar la lógica de datos y el dominio
- Usar el modelo de vista (MVC) para pasar información de las acciones de controlador a las plantillas de vista
- Explorar la nueva plantilla de ASP.NET MVC 4 para las aplicaciones de internet

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los elementos siguientes para completar esta práctica:

- [Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

**Instalación de fragmentos de código**

Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio. Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.

Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice C: Using Code Snippets](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico se compone de los ejercicios siguientes:

1. [Ejercicio 1: Crear el proyecto de aplicación Web de MusicStore MVC de ASP.NET](#Exercise1)
2. [Ejercicio 2: Creación de un controlador](#Exercise2)
3. [Ejercicio 3: Pasar parámetros a un controlador](#Exercise3)
4. [Ejercicio 4: Crear una vista](#Exercise4)
5. [Ejercicio 5: Crear un modelo de vista](#Exercise5)
6. [Ejercicio 6: Usar parámetros en la vista](#Exercise6)
7. [Ejercicio 7: Aspectos básicos de nueva plantilla de ASP.NET MVC 4](#Exercise7)

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


Tiempo estimado para completar esta práctica: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Ejercicio 1: Crear el proyecto de aplicación Web de MusicStore MVC de ASP.NET

En este ejercicio, obtendrá información sobre cómo crear una aplicación ASP.NET MVC en Visual Studio 2012 Express para Web, así como su organización de la carpeta principal. Además, obtendrá información sobre cómo agregar un nuevo controlador y que se muestre una cadena simple en la página principal de la aplicación.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Tarea 1: crear el proyecto de aplicación Web de ASP.NET MVC

1. En esta tarea, creará un proyecto vacío de aplicación de ASP.NET MVC mediante la plantilla de MVC Visual Studio. Iniciar **VS Express para Web**.
2. En el menú **Archivo**, haga clic en **Nuevo proyecto**.
3. En el **nuevo proyecto** cuadro de diálogo, seleccione la **aplicación Web de ASP.NET MVC 4** proyecto tipo, ubicado en **Visual C#,** **Web** plantilla lista.
4. Cambiar el **nombre** a *MvcMusicStore*.
5. Establecer la ubicación de la solución dentro de una nueva **comenzar** carpeta en la carpeta de origen de este ejercicio, por ejemplo **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-HOL-PATH]**. Haga clic en **Aceptar**.

    ![Crear cuadro de diálogo nuevo proyecto](aspnet-mvc-4-fundamentals/_static/image2.png "Crear cuadro de diálogo nuevo proyecto")

    *Crear cuadro de diálogo nuevo proyecto*
6. En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **básica** plantilla y asegúrese de que el **motor de vista** seleccionado es **Razor**. Haga clic en **Aceptar**.

    ![Nuevo cuadro de diálogo de proyecto de MVC de ASP.NET 4](aspnet-mvc-4-fundamentals/_static/image3.png "nuevo cuadro de diálogo de proyecto de MVC de ASP.NET 4")

    *Nuevo cuadro de diálogo de proyecto de MVC de ASP.NET 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Tarea 2: explorar la estructura de la solución

El marco de MVC de ASP.NET incluye una plantilla de proyecto de Visual Studio que le ayuda a crear aplicaciones Web que admiten el modelo de MVC. Esta plantilla crea una nueva aplicación Web de MVC de ASP.NET con las carpetas necesarias, plantillas de elementos y entradas del archivo de configuración.

En esta tarea, examinará la estructura de la solución para comprender los elementos que están implicados y sus relaciones. Las siguientes carpetas se incluyen en la aplicación de MVC de ASP.NET porque usa el marco de MVC de ASP.NET de forma predeterminada un &quot;Convención sobre configuración&quot; enfoque y hace algunas suposiciones predeterminado se basa en nombres de carpeta convenciones.

1. Una vez creado el proyecto, revise la estructura de carpetas que se ha creado en el Explorador de soluciones en el lado derecho:

    ![Estructura de carpetas de MVC de ASP.NET en el Explorador de soluciones](aspnet-mvc-4-fundamentals/_static/image4.png "estructura de carpetas de MVC de ASP.NET en el Explorador de soluciones")

    *Estructura de carpetas de MVC de ASP.NET en el Explorador de soluciones*

    1. **Controladores de**. Esta carpeta contiene las clases de controlador. En una aplicación de MVC en función, controladores están responsables de controlar la interacción del usuario final, manipular el modelo y, en última instancia elige una vista para representar la interfaz de usuario.

        > [!NOTE]
        > El marco de MVC requiere que los nombres de todos los controladores terminen con &quot;controlador&quot;-por ejemplo, HomeController, LoginController o ProductController.
    2. **Modelos**. Esta carpeta está disponible para las clases que representan el modelo de aplicación para la aplicación Web de MVC. Normalmente, esto incluye código que define los objetos y la lógica para interactuar con el almacén de datos. Normalmente, los objetos del modelo real será en bibliotecas de clases independientes. Sin embargo, cuando se crea una nueva aplicación, se podría incluir clases y, a continuación, muévalos a bibliotecas de clases independientes en un momento posterior del ciclo de desarrollo.
    3. **Vistas**. Esta carpeta es la ubicación recomendada para las vistas, los componentes responsables de mostrar la interfaz de usuario de la aplicación. Vistas usan archivos .aspx, .ascx, .cshtml y. master, además de otros archivos relacionados con la representación de vistas. Carpeta Views contiene una carpeta para cada controlador; la carpeta se denomina con el prefijo del nombre del controlador. Por ejemplo, si tiene un controlador denominado **HomeController**, la carpeta Views contiene una carpeta denominada Home. De forma predeterminada, cuando el marco de MVC de ASP.NET carga una vista, busca un archivo .aspx con el nombre de la vista solicitada en la carpeta Views\controllerName (**vistas [ControllerName] [acción] .aspx**) o (**vistas [ControllerName] [Acción] .cshtml**) para las vistas de Razor.

    > [!NOTE]
    > Además de las carpetas indicadas anteriormente, una aplicación Web de MVC usa el **Global.asax** valores predeterminados del archivo para establecer el enrutamiento global de direcciones URL, y utiliza el **Web.config** archivo para configurar la aplicación.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Tarea 3: agregar un HomeController

En las aplicaciones ASP.NET que no utilizan el marco de MVC, interacción del usuario se organiza alrededor de las páginas y provocar y controlar eventos desde esas páginas. En cambio, la interacción del usuario con aplicaciones ASP.NET MVC está organizada según controladores y sus métodos de acción.

Por otro lado, el marco de MVC de ASP.NET asigna las direcciones URL a las clases que se denominan controladores. Controladores de procesar las solicitudes entrantes, controlar proporcionados por el usuario y las interacciones, ejecutar lógica de aplicación adecuada y determinar la respuesta para enviar al cliente (Mostrar HTML, descargar un archivo, redirija a una dirección URL diferente, etcetera). En el caso de mostrar HTML, una clase de controlador llama normalmente a un componente de vista independiente para generar el marcado HTML para la solicitud. En una aplicación MVC, la vista solo muestra información; el controlador administra y responde a los proporcionados por el usuario y la interacción.

En esta tarea, agregará una clase de controlador que controlará las direcciones URL a la página principal del sitio de la tienda de música.

1. Haga clic en **controladores** carpeta en el Explorador de soluciones, seleccione **agregar** y, a continuación, **controlador** comando:

    ![Agregar un comando controlador](aspnet-mvc-4-fundamentals/_static/image5.png "agregar un comando de controlador")

    *Agregar controlador (comando)*
2. El **Agregar controlador** aparece el cuadro de diálogo. Nombre del controlador *HomeController* y presione **agregar**.

    ![Agregar cuadro de diálogo controlador](aspnet-mvc-4-fundamentals/_static/image6.png "Agregar cuadro de diálogo de controlador")

    *Agregar cuadro de diálogo de controlador*
3. El archivo **HomeController.cs** se crea en el **controladores** carpeta. Para tener la **HomeController** devuelven una cadena en su acción del índice, reemplace la **índice** método por el código siguiente:

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex1 HomeController índice*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecutar la aplicación

En esta tarea, se pruebe la aplicación en un explorador web.

1. Presione **F5** para ejecutar la aplicación. El proyecto se compila y servidor Web de IIS local se inicia. El equipo local, servidor Web de IIS se abrirá automáticamente un explorador web que apunte a la dirección URL del servidor Web.

    ![Aplicación que se ejecuta en un explorador web](aspnet-mvc-4-fundamentals/_static/image7.png "aplicación que se ejecuta en un explorador web")

    *Aplicación que se ejecuta en un explorador web*

    > [!NOTE]
    > El servidor Web de IIS local se ejecutará el sitio Web en un número de puerto libre aleatorio. En la ilustración anterior, el sitio se ejecuta en `http://localhost:50103/`, por lo que está utilizando el puerto 50103. El número de puerto puede variar.
2. Cierre el explorador.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Ejercicio 2: Creación de un controlador

En este ejercicio, obtendrá información sobre cómo actualizar el controlador para implementar funcionalidad simple de la aplicación de tienda de música. Ese controlador definirá métodos de acción para administrar cada una de las solicitudes específicas siguientes:

- Una página de lista de los géneros de música en la tienda de música
- Examinar página que enumera todos los álbumes de música de un género determinado
- Una página de detalles que muestra información sobre un álbum de música específico

Para el ámbito de este ejercicio, esas acciones simplemente devolverá una cadena de hasta ahora.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Tarea 1: agregar una nueva clase de StoreController

En esta tarea, agregará un nuevo controlador.

1. Si no está abierto, inicie **VS Express para Web 2012**.
2. En el **archivo** menú, elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex02 CreatingAController\Begin**, seleccione **Begin.sln** y haga clic en **abiertos**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

    1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
    2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
    3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

    > [!NOTE]
    > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. Agregue el nuevo controlador. Para ello, haga clic en el **controladores** carpeta en el Explorador de soluciones, seleccione **agregar** y, a continuación, el **controlador** comando. Cambiar el **nombre de controlador** a *StoreController*y haga clic en **agregar**.

    ![Agregar cuadro de diálogo controlador](aspnet-mvc-4-fundamentals/_static/image8.png "Agregar cuadro de diálogo de controlador")

    *Agregar cuadro de diálogo de controlador*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Tarea 2: modificar las acciones del StoreController

En esta tarea, modificará los métodos de controlador que se denominan **acciones**. Las acciones son responsables de controlar las solicitudes de dirección URL y determinar el contenido que debe enviarse en el explorador o el usuario que invocó la dirección URL.

1. El **StoreController** clase ya tiene un **índice** método. Se usará más adelante en esta práctica para implementar la página que enumera todos los juegos de la tienda de música. Por ahora, solo tiene que reemplazar el **índice** método por el código siguiente que devuelve una cadena &quot;Hola de Store.Index()&quot;:

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex2 StoreController índice*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Agregar **examinar** y **detalles** métodos. Para ello, agregue el código siguiente a la **StoreController**:

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecutar la aplicación

En esta tarea, se pruebe la aplicación en un explorador web.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en el **inicio** página. Cambiar la dirección URL para comprobar la implementación de cada acción.

    1. **/ Almacenar**. Verá  **&quot;Hola de Store.Index()&quot;**.
    2. **/ Almacén/examinar**. Verá  **&quot;Hola de Store.Browse()&quot;**.
    3. **/ / Detalles del almacén**. Verá  **&quot;Hola de Store.Details()&quot;**.

        ![Exploración StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse de exploración")

        *Exploración /Store/Browse*
3. Cierre el explorador.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Ejercicio 3: Pasar parámetros a un controlador

Hasta ahora, ha sido devolver cadenas constantes desde los controladores de. En este ejercicio obtendrá información sobre cómo pasar parámetros a un controlador de usar la dirección URL y la cadena de consulta y, a continuación, realizar las acciones de método responder con texto en el explorador.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Tarea 1: Agregar parámetro género StoreController

En esta tarea, va a usar el **querystring** enviar parámetros a la **examinar** método de acción en el **StoreController**.

1. Si no está abierto, inicie **VS Express para Web**.
2. En el **archivo** menú, elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex03 PassingParametersToAController\Begin**, seleccione **Begin.sln** y haga clic en **abiertos**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

    1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
    2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
    3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

    > [!NOTE]
    > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. Abra **StoreController** clase. Para ello, en el **el Explorador de soluciones**, expanda la **controladores** carpeta y haga doble clic en **StoreController.cs**.
4. Cambiar el **examinar** método, agregar un parámetro de cadena para solicitar un género específico. ASP.NET MVC automáticamente pasará cualquier cadena de consulta o parámetros con nombre de envío de formulario **género** a este método de acción cuando se invoca. Para ello, reemplace el **examinar** método por el código siguiente:

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

    > [!NOTE]
    > ¿Usa el **HttpUtility.HtmlEncode** método de utilidad que impide que los usuarios insertar Javascript en la vista con un vínculo como   **/almacén/examinar? Género =&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.
    > 
    > Para obtener más información, visite [este artículo de msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarea 2: ejecutar la aplicación

En esta tarea, va a probar la aplicación en un explorador web y usar el **género** parámetro.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en el **inicio** página. ¿Cambiar la dirección URL de   */almacenar/examinar? Género = Disco* para comprobar que la acción recibe el parámetro de género.

    ![Exploración StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "exploración StoreBrowseGenre = Disco")

    *¿Exploración /Store/Browse? Género = Disco*
3. Cierre el explorador.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Tarea 3: agregar un parámetro de identificador que se incrustan en la dirección URL

En esta tarea, va a usar el **URL** para pasar un **Id. de** parámetro a la **detalles** método de acción de la **StoreController**. Predeterminado de ASP.NET MVC convención de enrutamiento consiste en tratar el segmento de una dirección URL después del nombre de método de acción como un parámetro denominado **identificador**. Si el método de acción tiene el parámetro denominado Id, a continuación, ASP.NET MVC automáticamente pasará el segmento de dirección URL para usted como un parámetro. En la dirección URL **almacén/detalles/5**, **identificador** se interpretará como **5**.

1. Cambiar el **detalles** método de la **StoreController**, agregando una **int** parámetro denominado **Id. de**. Para ello, reemplace el **detalles** método por el código siguiente:

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecutar la aplicación

En esta tarea, va a probar la aplicación en un explorador web y usar el **identificador** parámetro.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en el **inicio** página. Cambiar la dirección URL de */Store/Details/5* para comprobar que la acción recibe el parámetro id.

    ![Exploración StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 de exploración")

    *Exploración /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Ejercicio 4: Crear una vista

Hasta ahora ha sido devolver cadenas de acciones del controlador. Aunque es una forma útil de conocer cómo funcionan los controladores, no es cómo se generan las aplicaciones Web reales. Las vistas son componentes que proporcionan un enfoque más adecuado para generar HTML al explorador con el uso de archivos de plantilla.

En este ejercicio obtendrá información sobre cómo agregar una página maestra de diseño para configurar una plantilla para el contenido HTML común, una hoja de estilos para mejorar la apariencia y funcionamiento del sitio y, por último, una plantilla de vista para habilitar HomeController devolver HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Tarea 1: modificar el archivo \_layout.cshtml

El archivo **~/Views/Shared/\_layout.cshtml** le permite configurar una plantilla para HTML comunes utilizar en todo el sitio Web. En esta tarea agregará una página maestra de diseño con un encabezado común con vínculos al área de página principal y el almacén.

1. Si no está abierto, inicie **VS Express para Web**.
2. En el **archivo** menú, elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex04 CreatingAView\Begin**, seleccione **Begin.sln** y haga clic en **abiertos**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

    1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
    2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
    3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

    > [!NOTE]
    > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. El archivo  **\_layout.cshtml** contiene el diseño de contenedor HTML para todas las páginas en el sitio. Incluye el  **&lt;html&gt;**  (elemento) para la respuesta HTML, así como el  **&lt;head&gt;**  y  **&lt;cuerpo&gt;**  elementos. **@RenderBody()** dentro del código HTML cuerpo identificar regiones esa vista plantillas podrá rellenar con contenido dinámico.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Agregue un encabezado común con vínculos al área de página principal y el almacén en todas las páginas en el sitio. Para ello, agregue el código siguiente &lt;cuerpo&gt; instrucción.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Incluir un elemento div para representar la sección de cuerpo de cada página. Reemplace  **@RenderBody()** con el siguiente código higlighted: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > ¿Sabía que? Visual Studio 2012 tiene fragmentos de código que resulta fácil agregar código de uso general en HTML, archivos de código y mucho más! Pruébelo alejar escribiendo  **&lt;div&gt;**  y presionando **ficha** dos veces para insertar una completa **div** etiqueta.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Tarea 2: agregar hojas de estilo CSS

La plantilla proyecto vacío de servidor incluye un archivo CSS muy simplificado que solo incluye estilos utilizados para mostrar mensajes de validación y formas básicas. Usará adicionales CSS e imágenes (potencialmente proporcionadas por un diseñador) con el fin de mejorar la apariencia y funcionamiento del sitio.

En esta tarea, agregará una hoja de estilos CSS para definir los estilos del sitio.

1. El archivo CSS y las imágenes se incluyen en el **Source\Assets\Content** carpeta de este laboratorio. Con el fin de agregarlos a la aplicación, arrastre su contenido desde un **el Explorador de Windows** ventana en el **el Explorador de soluciones** en Visual Studio Express para Web, tal y como se muestra a continuación:

    ![Arrastre el contenido de estilo](aspnet-mvc-4-fundamentals/_static/image12.png "arrastrando contenido de estilo")

    *Arrastre el contenido de estilo*
2. Un cuadro de diálogo de advertencia aparecerá, le pide confirmación reemplazar **Site.css** archivo y algunas imágenes existentes. Comprobar **aplicar a todos los elementos** y haga clic en **Sí**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Tarea 3: agregar una plantilla de vista

En esta tarea, agregará una plantilla de vista para generar la respuesta HTML que se va a usar la página maestra de diseño y CSS que se agregan en este ejercicio.

1. Para usar una plantilla de vista al examinar la página principal del sitio, primero deberá indicar que en lugar de devolver una cadena, la **HomeController índice** método devolverá un **vista**. Abra **HomeController** clase y cambiar su **índice** método para devolver un **ActionResult**, y que devuelva **View()**.

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex4 HomeController índice*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Ahora, debe agregar una plantilla de vista adecuada. Para ello, **haga** dentro de la **índice** método de acción y seleccione **agregar vista**. Esto le llevará a la **agregar vista** cuadro de diálogo.

    ![Agregar una vista desde dentro del método de índice](aspnet-mvc-4-fundamentals/_static/image13.png "agregar una vista desde dentro del método de índice")

    *Agregar una vista desde dentro del método de índice*
3. El **agregar vista** aparecerá el cuadro de diálogo para generar un archivo de plantilla de la vista. De forma predeterminada, este cuadro de diálogo rellena previamente el nombre de la plantilla de vista para que coincida con el método de acción que va a usar. Debido a que utilizó el **agregar vista** menú contextual en el **índice** método de acción en HomeController, el **agregar vista** cuadro de diálogo tiene un índice con el nombre de la vista predeterminada. Haga clic en **Agregar**.

    ![Agregar cuadro de diálogo Vista](aspnet-mvc-4-fundamentals/_static/image14.png "diálogo Agregar vista")

    *Agregar cuadro de diálogo de vista*
4. Visual Studio genera un **Index.cshtml** plantilla vista dentro de la **Views\Home** carpeta y, a continuación, lo abre.

    ![Crea la vista de índice de inicio](aspnet-mvc-4-fundamentals/_static/image15.png "vista de índice de inicio creada")

    *Vista de índice principal creada*

    > [!NOTE]
    > nombre y ubicación de la **Index.cshtml** archivo es pertinente y sigue las convenciones de nomenclatura de MVC de ASP.NET de forma predeterminada.
    > 
    > La carpeta \Views\**inicio** coincide con el nombre del controlador (**inicio** controlador). El nombre de la plantilla de vista (**índice**), coincide con el método de acción de controlador que se va a mostrar la vista.
    > 
    > De esta manera, ASP.NET MVC evita tener que especificar explícitamente el nombre o la ubicación de una plantilla de vista cuando se usa esta convención de nomenclatura para obtener una vista.
5. La plantilla de vista generada se basa en el  **\_layout.cshtml** anteriormente definida por la plantilla. Actualizar la propiedad ViewBag.Title a **inicio**y cambiar el contenido principal a **se trata de la página principal**, tal y como se muestra en el código siguiente:


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Seleccione **MvcMusicStore** proyecto en el Explorador de soluciones y presione **F5** para ejecutar la aplicación.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Tarea 4: comprobación

Para comprobar que ha realizado correctamente todos los pasos en el ejercicio anterior, haga lo siguiente:

Con la aplicación abierta en un explorador, debe tener en cuenta:

1. Método de acción de índice del HomeController encuentra y muestra el **\Views\Home\Index.cshtml** ver plantilla, aunque el código llame **devolver View()**, porque la plantilla de vista seguido el convención de nomenclatura estándar.
2. La página principal muestra el mensaje de bienvenida definido dentro de la **\Views\Home\Index.cshtml** plantilla de vista.
3. Está usando la página principal de la  **\_layout.cshtml** plantilla, de modo que el mensaje de bienvenida se encuentra en el diseño de sitio estándar HTML.

    ![Vista de índice mediante el LayoutPage definido y el estilo de inicio](aspnet-mvc-4-fundamentals/_static/image16.png "inicio la vista del índice mediante el LayoutPage y el estilo definido")

    *Vista de índice principal mediante el LayoutPage y el estilo definido*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Ejercicio 5: Crear un modelo de vista

Hasta ahora, realizado las vistas Mostrar codificado de forma rígida HTML, pero, para crear aplicaciones web dinámicas, la plantilla de vista debe recibir información desde el controlador. Una técnica común que se usará para ese fin es el **ViewModel** patrón, que permite que un controlador en un paquete toda la información necesaria para generar la respuesta HTML adecuada.

En este ejercicio, obtendrá información sobre cómo crear una clase ViewModel y agregar las propiedades necesarias: el número de géneros en el almacén y una lista de esas géneros. También se actualizará el StoreController para usar el modelo de vista creada y, por último, creará una nueva plantilla de vista que se mostrará las propiedades mencionadas en la página.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Tarea 1: crear una clase ViewModel

En esta tarea, creará una clase ViewModel que va a implementar el escenario de lista de género de almacén.

1. Si no está abierto, inicie **VS Express para Web**.
2. En el **archivo** menú, elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex05 CreatingAViewModel\Begin**, seleccione **Begin.sln** y haga clic en **abiertos**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

    1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
    2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
    3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

    > [!NOTE]
    > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. Crear un **ViewModels** carpeta que contendrá el modelo de vista. Para ello, haga clic en el nivel superior **MvcMusicStore** proyecto, seleccione **agregar** y, a continuación, **nueva carpeta**.

    ![Agregar una nueva carpeta](aspnet-mvc-4-fundamentals/_static/image17.png "agregar una nueva carpeta")

    *Agregar una nueva carpeta*
4. Nombre de la carpeta *ViewModels*.

    ![Carpeta ViewModels en el Explorador de soluciones](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels carpeta en el Explorador de soluciones")

    *Carpeta ViewModels en el Explorador de soluciones*
5. Crear un **ViewModel** clase. Para ello, haga doble clic en el **ViewModels** carpeta recientemente creado, seleccione **agregar** y, a continuación, **nuevo elemento**. En **código**, elija la **clase** de elemento y un nombre al archivo *StoreIndexViewModel.cs*, a continuación, haga clic en **agregar**.

    ![Agregar una nueva clase](aspnet-mvc-4-fundamentals/_static/image19.png "agregar una nueva clase")

    *Agregar una nueva clase*

    ![Crear clase de StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel de creación de clase")

    *Crear clase de StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Tarea 2: agregar propiedades a la clase ViewModel

Hay dos parámetros que se pasan desde el StoreController a la plantilla de vista con el fin de generar la respuesta esperada de HTML: el número de géneros en el almacén y una lista de esas géneros.

En esta tarea, agregará esas 2 propiedades la **StoreIndexViewModel** clase: **NumberOfGenres** (un entero) y **géneros** (una lista de cadenas).

1. Agregar **NumberOfGenres** y **géneros** propiedades para la **StoreIndexViewModel** clase. Para ello, agregue las siguientes líneas de 2 a la definición de clase:

    (Código de fragmento de código: *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel propiedades*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

    > [!NOTE]
    > El **{get; estableció;}**  notación hace uso de C# la característica de propiedades autoimplementadas. Proporciona las ventajas de una propiedad sin necesidad de nosotros declarar un campo de respaldo.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Tarea 3: actualización StoreController utilizar el StoreIndexViewModel

El **StoreIndexViewModel** clase encapsula la información necesaria para pasar de **StoreController**del **índice** método a una plantilla de vista con el fin de generar una respuesta .

En esta tarea, actualizará la **StoreController** para usar el **StoreIndexViewModel**.

1. Abra **StoreController** clase.

    ![Abriendo la clase StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController de apertura (clase)")

    *Clase de StoreController de apertura*
2. Para poder usar el **StoreIndexViewModel** clase desde el **StoreController**, agregue el siguiente espacio de nombres en la parte superior de la **StoreController** código:

    (Código de fragmento de código: *ASP.NET MVC 4 Fundamentals - StoreIndexViewModel Ex5 mediante ViewModels*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Cambiar el **StoreController**del **índice** método de acción para que crea y rellena una **StoreIndexViewModel** objeto y, a continuación, pasa a una plantilla de vista para generar una respuesta HTML con él.

    > [!NOTE]
    > En el laboratorio &quot;acceso a datos y modelos de ASP.NET MVC&quot; escribirá el código que recupera la lista de géneros de almacén de una base de datos. En el código siguiente, creará un **lista** de géneros datos ficticios que rellenarán el **StoreIndexViewModel**.
    > 
    > Después de crear y configurar la **StoreIndexViewModel** de objeto, se pasarán como argumento a la **vista** método. Esto indica que la plantilla de vista usará ese objeto para generar una respuesta HTML con él.
4. Reemplace el **índice** método por el código siguiente:

    (Código de fragmento de código: *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index (método)*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

    > [!NOTE]
    > Si no está familiarizado con C#, puede suponer que el uso **var** significa que la **viewModel** variable está enlazada a un tiempo de ejecución. No es correcto: el compilador de C# utiliza la inferencia de tipos en función de lo que se asigna a la variable para determinar que **viewModel** es de tipo **StoreIndexViewModel**. Además, si compila local **viewModel** variable como un **StoreIndexViewModel** escriba get comprobación en tiempo de compilación y soporte técnico del editor de código de Visual Studio.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Tarea 4: crear una plantilla de vista que usa StoreIndexViewModel

En esta tarea, creará una plantilla de vista que usará un objeto StoreIndexViewModel pasado desde el controlador para mostrar una lista de géneros.

1. Antes de crear la nueva plantilla de vista, vamos a compilar el proyecto para que la **Agregar cuadro de diálogo de vista** conoce la **StoreIndexViewModel** clase. Compile el proyecto seleccionando la **generar** elemento de menú y, a continuación, **MvcMusicStore generar**.

    ![Compilar el proyecto](aspnet-mvc-4-fundamentals/_static/image22.png "compilar el proyecto")

    *Compilar el proyecto*
2. Crear una nueva plantilla de vista. Para ello, pulse el botón derecho dentro de la **índice** método y seleccione **agregar vista**.

    ![Agregar una vista](aspnet-mvc-4-fundamentals/_static/image23.png "agregar una vista")

    *Agregar una vista*
3. Dado que la **Agregar cuadro de diálogo de vista** se invoca desde la **StoreController**, agregará la plantilla de vista de forma predeterminada en un **\Views\Store\Index.cshtml** archivo. Compruebe el **crear una fuertemente tipados-vista** casilla de verificación y, a continuación, seleccione **StoreIndexViewModel** como el **clase modelo**. Además, asegúrese de que el motor de vista seleccionado es **Razor**. Haga clic en **Agregar**.

    ![Agregar cuadro de diálogo Vista](aspnet-mvc-4-fundamentals/_static/image24.png "diálogo Agregar vista")

    *Agregar cuadro de diálogo de vista*

    El **\Views\Store\Index.cshtml** ver el archivo de plantilla se crea y se abre. En función de la información proporcionada para el **agregar vista** cuadro de diálogo en el último paso, la vista plantilla esperará un **StoreIndexViewModel** instancia como los datos que se va a usar para generar una respuesta HTML. Observará que la plantilla hereda un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` en C#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Tarea 5: actualizar la plantilla de vista

En esta tarea, actualizará la plantilla de vista creada en la última tarea para recuperar el número de géneros y sus nombres en la página.

> [!NOTE]
> Va a usar sintaxis @ (suele denominarse &quot;fragmentos de código&quot;) para ejecutar el código dentro de la plantilla de vista.


1. En el **Index.cshtml** de archivos, en la **almacén** carpeta, reemplace el código con lo siguiente:


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > En cuanto termine de escribir el período después de la palabra **modelo**, Intellisense de Visual Studio mostrará una lista de posibles propiedades y métodos que puede elegir.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Obtener propiedades del modelo y los métodos con IntelliSense de Visual Studio*
    > 
    > El **modelo** referencias de propiedad el **StoreIndexViewModel** objeto de que el controlador pasa a la plantilla de vista. Esto significa que puede tener acceso a todos los datos pasados desde el controlador a la plantilla de vista a través de la **modelo** propiedad y dele formato en una respuesta adecuada de HTML dentro de la plantilla de vista.
    > 
    > Basta con seleccionar el **NumberOfGenres** lista de propiedades de las características de Intellisense en lugar de escribir en y, a continuación, se le Autocompletar, presionando la **tecla tab**.
2. Recorrer la lista género en **StoreIndexViewModel** y crear un elemento HTML  **&lt;ul&gt;**  lista mediante un **foreach** bucle.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Presione **F5** para ejecutar la aplicación y examinar **/almacén**. Verá la lista de géneros pasa el **StoreIndexViewModel** objeto desde el **StoreController** a la plantilla de vista.

    ![Vista que muestra una lista de géneros](aspnet-mvc-4-fundamentals/_static/image26.png "vista muestra una lista de géneros")

    *Vista que muestra una lista de géneros*
4. Cierre el explorador.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Ejercicio 6: Usar parámetros en la vista

En el ejercicio 3 aprendido cómo pasar parámetros al controlador. En este ejercicio, aprenderá a usar esos parámetros en la plantilla de vista. A tal efecto, se introducirán en primer lugar para las clases de modelo que le ayudará a administrar su lógica de datos y el dominio. Además, obtendrá información sobre cómo crear vínculos a páginas dentro de la aplicación de MVC de ASP.NET sin preocuparse de cosas como las rutas de acceso de dirección URL de codificación.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Tarea 1: agregar clases de modelo

A diferencia de ViewModels, que se crean exclusivamente para devolver información del controlador a la vista, se generan clases de modelo para contener y administrar la lógica de datos y el dominio. En esta tarea agregará dos clases de modelo para representar estos conceptos: **género** y **álbum**.

1. Si no está abierto, inicie **VS Express para Web**
2. En el **archivo** menú, elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex06 UsingParametersInView\Begin**, seleccione **Begin.sln** y haga clic en **abiertos**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

    1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
    2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
    3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

    > [!NOTE]
    > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. Agregar un **género** clase de modelo. Para ello, haga clic en el **modelos** carpeta en el **el Explorador de soluciones**, seleccione **agregar** y, a continuación, el **nuevo elemento** opción. En **código**, elija la **clase** de elemento y un nombre al archivo *Genre.cs*, a continuación, haga clic en **agregar**.

    ![Agregar una clase](aspnet-mvc-4-fundamentals/_static/image27.png "agregar una clase")

    *Agregar un nuevo elemento*

    ![Agregar clase de modelo de género](aspnet-mvc-4-fundamentals/_static/image28.png "Agregar clase de modelo de género")

    *Agregar clase de modelo de género*
4. Agregar un **nombre** propiedad a la clase de género. Para ello, agregue el código siguiente:

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex6 género*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Siguiendo el mismo procedimiento que antes, agregue un **álbum** clase. Para ello, haga clic en el **modelos** carpeta en el **el Explorador de soluciones**, seleccione **agregar** y, a continuación, el **nuevo elemento** opción. En **código**, elija la **clase** de elemento y un nombre al archivo *Album.cs*, a continuación, haga clic en **agregar**.
6. Agregue dos propiedades a la clase de álbum: **género** y **título**. Para ello, agregue el código siguiente:

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - álbum Ex6*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Tarea 2: agregar un StoreBrowseViewModel

A **StoreBrowseViewModel** se utilizarán en esta tarea para mostrar los álbumes que coinciden con un género seleccionado. En esta tarea, creará esta clase y, a continuación, agregue dos propiedades para controlar la **género** y su **álbum**de la lista.

1. Agregar un **StoreBrowseViewModel** clase. Para ello, haga clic en el **ViewModels** carpeta en el **el Explorador de soluciones**, seleccione **agregar** y, a continuación, el **nuevo elemento** opción. En **código**, elija la **clase** de elemento y un nombre al archivo *StoreBrowseViewModel.cs*, a continuación, haga clic en **agregar**.
2. Agregue una referencia a los modelos en **StoreBrowseViewModel** clase. Para ello, agregue lo siguiente con el espacio de nombres:

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex6 UsingModel*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Agregar dos propiedades que **StoreBrowseViewModel** clase: **género** y **álbumes**. Para ello, agregue el código siguiente:

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex6 ModelProperties*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

    > [!NOTE]
    > ¿Qué es **lista&lt;álbum&gt;**  ?: está usando esta definición de la **lista&lt;T&gt;**  tipo, donde **T** restringe el tipo de los elementos de esta **lista** pertenecen, en este caso **álbum** (o cualquiera de sus descendientes).
    > 
    > Esta capacidad para diseñar clases y métodos que aplazan la especificación de uno o más tipos hasta que la clase o el método se declara y crea una instancia de un código de cliente es una característica del lenguaje C# llamado **genéricos**.
    > 
    > **Lista&lt;T&gt;**  es el equivalente genérico de la **ArrayList** escriba y está disponible en la **System.Collections.Generic** espacio de nombres. Una de las ventajas de usar **genéricos** es que porque no se especifica el tipo, no es necesario ocuparse de operaciones como la conversión de los elementos en la comprobación de tipo **álbum** como lo haría con un **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Tarea 3: usar el nuevo modelo de vista en el StoreController

En esta tarea, modificará el **StoreController**del **examinar** y **detalles** métodos de acción para utilizar el nuevo **StoreBrowseViewModel** .

1. Agregue una referencia a la carpeta de modelos en **StoreController** clase. Para ello, expanda el **controladores** carpeta en el **el Explorador de soluciones** y abra el **StoreController** clase. A continuación, agregue el código siguiente:

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex6 UsingModelInController*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Reemplace el **examinar** método de acción para usar el **StoreViewBrowseController** clase. Creará un género y dos nuevos objetos de álbumes con datos ficticios (en el laboratorio de prácticas siguiente consumirá datos reales de una base de datos). Para ello, reemplace el **examinar** método por el código siguiente:

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex6 BrowseMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Reemplace el **detalles** método de acción para usar el **StoreViewBrowseController** clase. Se creará una nueva **álbum** objeto que se devolverá a la **vista**. Para ello, reemplace el **detalles** método por el código siguiente:

    (Código de fragmento de código: *Fundamentos de ASP.NET MVC 4 - Ex6 DetailsMethod*)


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Tarea 4: agregar una plantilla de vista de exploración

En esta tarea, agregará un **examinar** vista para mostrar los álbumes que encuentra para un género específico.

1. Antes de crear la nueva plantilla de vista, debe compilar el proyecto para que la **agregar vista** diálogo conoce la **ViewModel** clase se debe utilizar. Compile el proyecto seleccionando la **generar** elemento de menú y, a continuación, **MvcMusicStore generar**.
2. Agregar un **examinar** vista. Para ello, haga clic en el **examinar** método de acción de la **StoreController** y haga clic en **agregar vista**.
3. En el **agregar vista** diálogo cuadro, compruebe que el nombre de vista es **examinar**. Compruebe el **crear una vista fuertemente tipada** casilla de verificación y seleccione **StoreBrowseViewModel** desde el **clase modelo** lista desplegable. Deje los demás campos con sus valores predeterminados. A continuación, haga clic en **Agregar**.

    ![Agregar una vista Examinar](aspnet-mvc-4-fundamentals/_static/image29.png "agregar una vista de exploración")

    *Agregar una vista de exploración*
4. Modificar el **Browse.cshtml** para mostrar información del género, obtener acceso a la **StoreBrowseViewModel** objeto que se pasa a la plantilla de vista. Para ello, reemplace el contenido por lo siguiente: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecutar la aplicación

En esta tarea, realizará las pruebas que el **examinar** método recupera álbumes de la **examinar** acción del método.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. ¿Cambiar la dirección URL de   **/almacenar/examinar? Género = Disco** para comprobar que la acción devuelve dos álbumes.

    ![Examen de Disco álbumes de almacén](aspnet-mvc-4-fundamentals/_static/image30.png "exploración álbumes de Disco de almacenamiento")

    *Exploración álbumes de Disco de almacenamiento*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Tarea 6: mostrar información sobre un álbum determinado

En esta tarea, implementará el **/detalles del almacén** vista para mostrar información sobre un álbum determinado. En este laboratorio práctico, todo lo que se va a mostrar sobre el álbum ya esté incluido en el **vista** plantilla. Así, en lugar de crear un **StoreDetailsViewModel** (clase), se usará el actual **StoreBrowseViewModel** plantilla pasándole el álbum.

1. Cierre el explorador si es necesario, para volver a la ventana de Visual Studio. Agregue un nuevo **detalles** ver para el **StoreController**del **detalles** método de acción. Para ello, haga clic en el **detalles** método en el **StoreController** clase y haga clic en **agregar vista**.
2. En el **agregar vista** cuadro de diálogo, compruebe que la **nombre de la vista** es **detalles**. Compruebe el **crear una vista fuertemente tipada** casilla de verificación y seleccione **álbum** desde el **clase modelo** lista desplegable. Deje los demás campos con sus valores predeterminados. A continuación, haga clic en **Agregar**. Esto creará y abrirá una **\Views\Store\Details.cshtml** archivo.

    ![Agregar una vista de detalles](aspnet-mvc-4-fundamentals/_static/image31.png "agregar una vista de detalles")

    *Agregar una vista de detalles*
3. Modificar el **Details.cshtml** archivo para mostrar información del álbum, obtener acceso a la **álbum** objeto que se pasa a la plantilla de vista. Para ello, reemplace el contenido por lo siguiente: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tarea 7: ejecutar la aplicación

En esta tarea, realizará las pruebas que el **detalles** vista recupera información del álbum desde la **detalles acción** método.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en el **inicio** página. Cambiar la dirección URL de **/Store/Details/5** para comprobar la información del álbum.

    ![Examinar los detalles de álbumes](aspnet-mvc-4-fundamentals/_static/image32.png "exploración álbumes detalle")

    *Examinar los detalles del álbum*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Tarea 8: agregar vínculos entre páginas

En esta tarea, agregará un vínculo en la vista de almacén para tener un vínculo en cada nombre de género correspondientes **/almacén/examinar** dirección URL. Así, al hacer clic en un género, por ejemplo **Disco**, se le remitirá a **/almacén/examinar? género = Disco** dirección URL.

1. Cierre el explorador si es necesario, para volver a la ventana de Visual Studio. Actualización de la **índice** página para agregar un vínculo a la **examinar** página. Para ello, en el **el Explorador de soluciones** expandir la **vistas** carpeta, la **almacén** carpeta y haga doble clic en el **Index.cshtml** página.
2. Agregar un vínculo a la vista de exploración que indica el género seleccionado. Para ello, reemplace el código que aparece resaltado en el  **&lt;li&gt;**  etiquetas: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

    > [!NOTE]
    > otro enfoque podría ser vincular directamente a la página, con un código similar al siguiente:
    > 
    > &lt;un href =&quot;/almacén/examinar? género =@genreName&quot;&gt;@genreName&lt;/a&gt;
    > 
    > Aunque este enfoque funciona, depende de una cadena codificada. Si posteriormente se cambia el nombre del controlador, tendrá que cambiar manualmente esta instrucción. Una alternativa mejor es usar un **aplicación auxiliar HTML** método. ASP.NET MVC incluye un método de aplicación auxiliar HTML que está disponible para estas tareas. El **Html.ActionLink()** método auxiliar resulta muy sencillo crear HTML  **&lt;una&gt;**  vínculos, asegurándose de que las rutas de acceso de dirección URL están correctamente la dirección URL codificada.
    > 
    > Htlm.ActionLink tiene varias sobrecargas. En este ejercicio usará una que toma tres parámetros:
    > 
    > 1. Texto del vínculo, que mostrará el nombre de género
    > 2. Nombre de acción de controlador (**examinar**)
    > 3. Valores de parámetro, especificando el nombre de ruta (**género**) y el valor (**nombre de género**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Tarea 9 - ejecutar la aplicación

En esta tarea, probará que se muestra cada género con un vínculo a la correspondiente **/almacén/examinar** dirección URL.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/almacenar** para comprobar que cada género vincula a la correspondiente **/almacén/examinar** dirección URL.

    ![Exploración géneros con vínculos a la página Examinar](aspnet-mvc-4-fundamentals/_static/image33.png "géneros de exploración con vínculos a la página Examinar")

    *Géneros de exploración con vínculos a la página Examinar*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Tarea 10 - utilizando la colección de ViewModel dinámica para pasar valores

En esta tarea, obtendrá información sobre un método simple y efectivo para pasar valores entre el controlador y la vista sin realizar ningún cambio en el modelo. ASP.NET MVC 4 proporciona la colección &quot;ViewModel&quot;, que se puede asignar a cualquier valor dinámico y acceder a ellos dentro de los controladores y vistas también.

Ahora usará la colección dinámica ViewBag para pasar una lista de &quot; **destacan géneros** &quot; desde el controlador a la vista. Tendrá acceso la vista de índice de almacén para **ViewModel** y mostrar la información.

1. Cierre el explorador si es necesario, para volver a la ventana de Visual Studio. Abra **StoreController.cs** y modificar **índice** método para crear una lista de destacan géneros en ViewModel colección:


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > También puede usar la sintaxis **ViewBag [&quot;Starred&quot;]** para tener acceso a las propiedades.
2. El icono de estrella  **&quot;starred.png&quot;**  se incluye en el **Source\Assets\Images** carpeta de este laboratorio. Para agregarlo a la aplicación, arrastre su contenido desde un **el Explorador de Windows** ventana en el **el Explorador de soluciones** en Visual Web Developer Express, tal y como se muestra a continuación:

    ![Imagen de estrella de agregar a la solución](aspnet-mvc-4-fundamentals/_static/image34.png "imagen de estrella de agregar a la solución")

    *Agregar imagen de estrella a la solución*
3. Abra la vista **Store/Index.cshtml** y modificar el contenido. Va a leer el &quot;destacan&quot; propiedad en el **ViewBag** colección y pregunte si el nombre de género actual está en la lista. En ese caso, se mostrará un icono de estrella de derecha al vínculo de género.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Tarea 11: ejecutar la aplicación

En esta tarea, probará que el marcado géneros mostrar un icono de estrella.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en el **inicio** página. Cambiar la dirección URL de **/almacén** para comprobar que cada género destacado tiene la etiqueta respetar:

    ![Exploración géneros con elementos de marcado](aspnet-mvc-4-fundamentals/_static/image35.png "exploración géneros con elementos de marcado a continuación:")

    *Exploración géneros con elementos de marcado a continuación:*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Ejercicio 7: Aspectos básicos de nueva plantilla de ASP.NET MVC 4

En este ejercicio, explorará las mejoras en las plantillas de proyecto de ASP.NET MVC 4, echar un vistazo las características más relevantes de la nueva plantilla.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Tarea 1: Explorar la plantilla de aplicación de Internet de ASP.NET MVC 4

1. Si no está abierto, inicie **VS Express para Web**
2. Seleccione el **archivo | Nuevos | Proyecto** comando de menú. En el **nuevo proyecto** cuadro de diálogo, seleccione la **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija la **aplicación Web de ASP.NET MVC 4**. **Nombre** el proyecto *MusicStore* y actualizar la **nombre de la solución** a *comenzar*, a continuación, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar**.

    ![Crear un nuevo proyecto de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "crear un nuevo proyecto de ASP.NET MVC 4")

    *Crear un nuevo proyecto de ASP.NET MVC 4*
3. En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **aplicación de Internet** plantilla de proyecto y haga clic en **Aceptar**. Tenga en cuenta que puede seleccionar como el motor de vista Razor o ASPX.

    ![Crear una nueva aplicación de Internet de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "crear una nueva aplicación de Internet de ASP.NET MVC 4")

    *Crear una nueva aplicación de Internet de ASP.NET MVC 4*

    > [!NOTE]
    > Se ha introducido la sintaxis Razor en ASP.NET MVC 3. Su objetivo es reducir el número de caracteres y pulsaciones de teclas necesarias en un archivo, lo que permite una rápida y fluido de flujo de trabajo de codificación. Razor aprovecha existente C# /VB (u otro) dominio del idioma y ofrece una sintaxis de marcado de plantilla que permite a un flujo de trabajo de construcción de HTML Maravilla.
4. Presione **F5** para ejecutar la solución y ver la plantilla renovada. Puede consultar las siguientes características:

    1. **Plantillas de estilo moderno**

        Las plantillas que se han renovado, proporcionando más estilos de aspecto moderno.

        ![Plantillas de MVC de ASP.NET 4 estilo](aspnet-mvc-4-fundamentals/_static/image38.png "estilo plantillas ASP.NET MVC 4")

        *Plantillas de estilo de ASP.NET MVC 4*
    2. **Representación adaptable**

        Consulte Cambiar el tamaño de la ventana del explorador y observe cómo el diseño de página se adapta dinámicamente al tamaño de la ventana nueva. Estas plantillas usan la técnica de representación adaptable para representar correctamente en plataformas de escritorio y móviles sin ninguna personalización.

        ![Plantilla de proyecto de ASP.NET MVC 4 de tamaños diferentes del explorador](aspnet-mvc-4-fundamentals/_static/image39.png "plantilla de proyecto de ASP.NET MVC 4 de tamaños diferentes del explorador")

        *Plantilla de proyecto de ASP.NET MVC 4 de tamaños diferentes del explorador*
5. Cierre el explorador para detener al depurador y vuelva a Visual Studio.
6. Ahora es posible explorar la solución y extraer del repositorio algunas de las nuevas características presentadas en ASP.NET MVC 4 en la plantilla de proyecto.

    ![ASP.NET MVC4-internet---plantilla de proyecto aplicación](aspnet-mvc-4-fundamentals/_static/image40.png "la plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4")

    *La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4*

    1. **Marcado de HTML5**

        Examinar vistas de plantilla para averiguar el marcado de tema nuevo, por ejemplo abierto **About.cshtml** ver dentro de **inicio** carpeta.

        ![Nueva plantilla, usando un marcado Razor y HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nueva plantilla, usando un marcado Razor y HTML5")

        *Nueva plantilla, usando un marcado Razor y HTML5*
    2. **Bibliotecas de JavaScript incluidas**

        1. **jQuery**: jQuery simplifica atraviesa de documento HTML, control de eventos, animación y las interacciones de Ajax.
        2. **jQuery UI**: esta biblioteca proporciona abstracciones de interacción de bajo nivel y animación, avanzada efectos y widgets aptas para temas, creados a partir de la biblioteca de JavaScript de jQuery.

            > [!NOTE]
            > Puede obtener información acerca de jQuery y jQuery UI en [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).
        3. **KnockoutJS**: plantilla predeterminada de las 4 de ASP.NET MVC incluye ahora **KnockoutJS**, un marco de JavaScript MVVM que le permite crear aplicaciones completas y de alta capacidad de respuesta web con JavaScript y HTML. Al igual que en ASP.NET MVC 3, jQuery y jQuery bibliotecas de interfaz de usuario también se incluyen en ASP.NET MVC 4.

            > [!NOTE]
            > Puede obtener más información acerca de la biblioteca de KnockOutJS en este vínculo: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).
        4. **Modernizr**: esta biblioteca se ejecuta automáticamente, hacer que su sitio compatible con los exploradores más antiguos cuando se empleen tecnologías HTML5 y CSS3.

            > [!NOTE]
            > Puede obtener más información acerca de la biblioteca de Modernizr pertenecientes a este vínculo: [http://www.modernizr.com/](http://www.modernizr.com/).
    3. **SimpleMembership incluido en la solución**

        SimpleMembership se ha diseñado como un reemplazo para el sistema de proveedor de funciones ASP.NET y la pertenencia a anterior. Tiene muchas características nuevas que facilitan el desarrollador a páginas web seguras de manera más flexible.

        La plantilla de Internet ya ha configurado algunas cosas para integrar SimpleMembership, por ejemplo, el AccountController está preparado para usar OAuthWebSecurity (para el registro de la cuenta de OAuth, inicio de sesión, administración, etc.) y la seguridad de la Web.

        ![SimpleMembership incluidos en la solución](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership incluidos en la solución")

        *SimpleMembership incluidos en la solución*

        > [!NOTE]
        > Obtener más información acerca de [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) en MSDN.

> [!NOTE]
> Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico ha aprendido los fundamentos de MVC de ASP.NET:

- Los elementos básicos de una aplicación MVC y cómo interactúan
- Cómo crear una aplicación de MVC de ASP.NET
- Cómo agregar y configurar controladores para controlar los parámetros se pasa a través de la dirección URL y la cadena de consulta
- Cómo agregar una página maestra de diseño para configurar una plantilla para el contenido HTML común, una hoja de estilos para mejorar la apariencia y funcionamiento y una plantilla de vista para mostrar el contenido HTML
- Cómo usar el patrón ViewModel para pasar propiedades a la plantilla de vista para mostrar información dinámica
- Cómo usar los parámetros pasados a los controladores en la plantilla de vista
- Cómo agregar vínculos a páginas dentro de la aplicación de ASP.NET MVC
- Cómo agregar y utilizar propiedades dinámicas en una vista
- Las mejoras en las plantillas de proyecto de ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la  **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.

1. Vaya a [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; *Visual Studio Express 2012 for Web con SDK de Windows Azure*&quot;.
2. Haga clic en **instalar ahora**. Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Se completó la instalación](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Se completó la instalación*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.

    ![Express de VS para icono Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    *Express de VS para icono Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

Este apéndice le mostrará cómo crear un nuevo sitio web del Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación Web Deploy proporcionada por Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarea 1: crear un nuevo sitio Web desde las ventanas de Portal de Azure

1. Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.

    > [!NOTE]
    > Con Windows Azure puede hospedar 10 sitios Web de ASP.NET de forma gratuita y, a continuación, ajustar la escala a medida que crece el tráfico. Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).

    ![Inicie sesión en el portal de Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "inicie sesión en el portal de Windows Azure")

    *Inicie sesión en el Portal de administración de Azure*
2. Haga clic en **New** en la barra de comandos.

    ![Crear un nuevo sitio Web](aspnet-mvc-4-fundamentals/_static/image49.png "crear un nuevo sitio Web")

    *Crear un nuevo sitio Web*
3. Haga clic en **proceso** | **sitio Web**. A continuación, seleccione **creación rápida** opción. Proporcione una dirección de URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.

    > [!NOTE]
    > Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar. La opción Creación rápida permite implementar una aplicación web completada al Windows Azure sitio Web desde fuera del portal. No se incluyen pasos para configurar una base de datos.

    ![Crear un nuevo sitio Web mediante Creación rápida](aspnet-mvc-4-fundamentals/_static/image50.png "crear un nuevo sitio Web mediante Creación rápida")

    *Crear un nuevo sitio Web mediante Creación rápida*
4. Espere hasta que el nuevo **sitio Web** se crea.
5. Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna. Compruebe que funciona el nuevo sitio Web.

    ![En el nuevo sitio web](aspnet-mvc-4-fundamentals/_static/image51.png "exploración al sitio web nuevo")

    *Examinar el sitio web nuevo*

    ![Sitio Web que ejecute](aspnet-mvc-4-fundamentals/_static/image52.png "sitio Web que se ejecuta")

    *Sitio Web que se ejecuta*
6. Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración del sitio web](aspnet-mvc-4-fundamentals/_static/image53.png "abrir las páginas de administración del sitio web")

    *Abrir las páginas de administración del sitio Web*
7. En el **panel** página, en la **vista rápida** sección, haga clic en el **descargar perfil de publicación** vínculo.

    > [!NOTE]
    > El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web a los sitios Web de Windows Azure.

    ![Descargar el sitio web de perfil de publicación](aspnet-mvc-4-fundamentals/_static/image54.png "descargar el sitio web de perfil de publicación")

    *Descargar el sitio Web de perfil de publicación*
8. Descargue el archivo de perfil de publicación en una ubicación conocida. Aún más en este ejercicio verá cómo utilizar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de perfil de publicación](aspnet-mvc-4-fundamentals/_static/image55.png "guardar el perfil de publicación")

    *Guardar el archivo de perfil de publicación*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si la aplicación realiza el uso de SQL Server debe crear un servidor de base de datos SQL de bases de datos. Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.

1. Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación. Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**. Si no tiene un servidor que creó, puede crear uno mediante la **agregar** botón en la barra de comandos. Tome nota de la **nombre del servidor y la dirección URL, nombre de inicio de sesión de administrador y contraseña**, tal y como se va a utilizar en las siguientes tareas. No cree la base de datos, tal y como se creará en una fase posterior.

    ![Panel de servidor de base de datos SQL](aspnet-mvc-4-fundamentals/_static/image56.png "panel de base de datos SQL Server")

    *Panel de base de datos SQL Server*
2. En la siguiente tarea probará la conexión de base de datos de Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**. Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguela en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) botón.

    ![Agregar dirección IP del cliente](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Agregar dirección IP del cliente*
3. Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas lista, haga clic en **guardar** para confirmar los cambios.

    ![Confirmar cambios](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Confirmar cambios*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

1. Vuelva a la solución de ASP.NET MVC 4. En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.

    ![Publicar la aplicación](aspnet-mvc-4-fundamentals/_static/image60.png "publicar la aplicación")

    *Publicar el sitio web*
2. Importar el perfil de publicación que se ha guardado en la primera tarea.

    ![Importar el perfil de publicación](aspnet-mvc-4-fundamentals/_static/image61.png "importar el perfil de publicación")

    *Importar perfil de publicación*
3. Haga clic en **validar conexión**. Una vez completada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.

    ![Validación de la conexión](aspnet-mvc-4-fundamentals/_static/image62.png "validación de la conexión")

    *Validación de la conexión*
4. En el **configuración** página, en la **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de la base de datos (es decir, **DefaultConnection**).

    ![Configuración de Web deploy](aspnet-mvc-4-fundamentals/_static/image63.png "configuración de Web deploy")

    *Configuración de Web deploy*
5. Configure la conexión de base de datos de la manera siguiente:

    - En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante la *tcp:* prefijo.
    - En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.
    - En **contraseña** escriba la contraseña de inicio de sesión de administrador de servidor.
    - Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.

    ![Configurar la cadena de conexión de destino](aspnet-mvc-4-fundamentals/_static/image64.png "configurar la cadena de conexión de destino")

    *Configurar la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le solicite para crear la base de datos, haga clic en **Sí**.

    ![Crear la base de datos](aspnet-mvc-4-fundamentals/_static/image65.png "crear la cadena de la base de datos")

    *Crear la base de datos*
7. La cadena de conexión que se va a usar para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunte a la base de datos SQL](aspnet-mvc-4-fundamentals/_static/image66.png "cadena de conexión que apunte a la base de datos SQL")

    *Cadena de conexión que apunte a la base de datos SQL*
8. En el **vista previa** página, haga clic en **publicar**.

    ![Publicar la aplicación web](aspnet-mvc-4-fundamentals/_static/image67.png "publicar la aplicación web")

    *Publicar la aplicación web*
9. Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.

    ![Publica la aplicación en Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "aplicación se publicó en Windows Azure")

    *Aplicación que se publican en Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apéndice C: usar fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-fundamentals/_static/image69.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento (sin espacios ni guiones).
3. Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](aspnet-mvc-4-fundamentals/_static/image70.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-fundamentals/_static/image71.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-fundamentals/_static/image72.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1. Haga clic en donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-fundamentals/_static/image73.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Seleccione el fragmento de código relevante de la lista haciendo clic en él](aspnet-mvc-4-fundamentals/_static/image74.png "elegir el fragmento de código relevante de la lista haciendo clic en él")

*Seleccione el fragmento de código relevante de la lista haciendo clic en él*
