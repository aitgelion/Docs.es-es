---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: "Agregar contenido a Control de código fuente | Documentos de Microsoft"
author: jrjlee
description: "Este tema explica cómo agregar contenido a control de código fuente de Team Foundation Server (TFS) 2010. Describe cómo agregar soluciones y proyectos a un proyecto de equipo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: d46e2697d10ca27f8e08533350a6e7f2354b4a43
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="adding-content-to-source-control"></a>Agregar contenido a Control de código fuente
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema explica cómo agregar contenido a control de código fuente de Team Foundation Server (TFS) 2010. Describe cómo agregar soluciones y proyectos a un proyecto de equipo en TFS, y se explica cómo agregar dependencias externas como .NET Framework o ensamblados al control de código fuente.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales que utiliza una solución de ejemplo & #x 2014; la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; para representar una aplicación web con un nivel de complejidad, incluso una aplicación de ASP.NET MVC 3, Windows realista Servicio de Communication Foundation (WCF) y un proyecto de base de datos.

## <a name="task-overview"></a>Información general sobre tareas

En la mayoría de los casos, todos los miembros del equipo de desarrollo deben ser capaz de agregar contenido al control de código fuente. Para agregar una solución al control de código fuente en TFS, debe completar estos pasos de alto nivel:

- Conectarse a un proyecto de equipo.
- Asignar la estructura de carpetas del proyecto de equipo en el servidor a una estructura de carpetas en el equipo local.
- Agregar la solución y su contenido al control de código fuente.
- Agregar las dependencias externas a control de código fuente.

En este tema le mostrará cómo realizar estos procedimientos.

Las tareas y los tutoriales en este tema se suponen que ya ha creado un nuevo proyecto de equipo TFS para administrar el contenido. Para obtener más información acerca de cómo crear un nuevo proyecto de equipo, consulte [crea un proyecto de equipo en TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>¿Que lleva a cabo estos procedimientos?

En la mayoría de los casos, todos los miembros del equipo del desarrollador de podrá agregar y modificar contenido dentro de los proyectos de equipo específico.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Conectarse a un proyecto de equipo y crear una asignación de carpeta

Antes de agregar cualquier tipo de contenido para el control de código fuente, debe conectarse a un proyecto de equipo y crear una asignación entre la estructura de carpetas en el servidor y el sistema de archivos en el equipo local.

**Para conectarse a un proyecto de equipo y asignar una ruta de acceso local**

1. En la estación de trabajo de desarrollador, abra Visual Studio 2010.
2. En Visual Studio, en el **equipo** menú, haga clic en **conectar con Team Foundation Server**.

    > [!NOTE]
    > Si ya ha configurado una conexión a un servidor TFS, puede omitir los pasos 3 a 6.
3. En el **conexión al proyecto de equipo** cuadro de diálogo, haga clic en **servidores**.
4. En el **agregar o quitar Team Foundation Server** cuadro de diálogo, haga clic en **agregar**.
5. En el **agregar Team Foundation Server** cuadro de diálogo, proporcione los detalles de la instancia de TFS y, a continuación, haga clic en **Aceptar**.

    ![](adding-content-to-source-control/_static/image1.png)
6. En el **agregar o quitar Team Foundation Server** cuadro de diálogo, haga clic en **cerrar**.
7. En el **conectar al proyecto de equipo** cuadro de diálogo, seleccione la instancia TFS que desea conectarse a la red, seleccione el equipo de colección de proyectos, seleccione el proyecto de equipo que desea agregar a y, a continuación, haga clic en **conectar**.

    ![](adding-content-to-source-control/_static/image2.png)
8. En el **Team Explorer** ventana, expanda el proyecto de equipo y, a continuación, haga doble clic en **Control de código fuente**.

    ![](adding-content-to-source-control/_static/image3.png)
9. En el **Explorador de Control de código fuente** , haga clic en **no asignado**.

    ![](adding-content-to-source-control/_static/image4.png)
10. En el **mapa** cuadro de diálogo, en la **carpeta Local** cuadro, vaya a (o crear) una carpeta local para actuar como la carpeta raíz del proyecto de equipo y, a continuación, haga clic en **mapa**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Cuando le pide que descargue archivos de origen, haga clic en **Sí**.

    ![](adding-content-to-source-control/_static/image6.png)

En este momento, ha asignado la carpeta de servidor para el proyecto de equipo a una carpeta local en la estación de trabajo de desarrollador. También ha descargado todo el contenido del proyecto de equipo a la estructura de la carpeta local. Ahora puede empezar a agregar su propio contenido al control de código fuente.

## <a name="add-projects-and-solutions-to-source-control"></a>Agregar proyectos y soluciones al Control de código fuente

Para agregar proyectos y soluciones al control de código fuente, primero debe mover a la carpeta del proyecto de equipo asignada en el equipo local. A continuación, puede comprobar en el contenido que se va a sincronizar las adiciones con el servidor.

**Para agregar proyectos al control de código fuente**

1. En la estación de trabajo de desarrollador, mueva los proyectos y soluciones a una ubicación adecuada en la estructura de carpetas asignadas para el proyecto de equipo.

    > [!NOTE]
    > Muchas organizaciones tienen un método preferido para cómo deben organizarse proyectos y soluciones en el control de código fuente. Para obtener instrucciones sobre cómo estructurar carpetas, consulte [How To: estructura de carpetas de Control de su origen en Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Abra la solución en Visual Studio 2010.
3. En el **el Explorador de soluciones** ventana, haga clic en la solución y, a continuación, haga clic en **Agregar solución al Control de código fuente**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > En algunos casos, dependiendo de cómo su organización le gusta contenido de la estructura en TFS, debe agregar proyectos al control de código fuente individualmente para proporcionar un mayor control sobre cómo se organiza el código fuente.
4. Compruebe que la **Explorador de Control de código fuente** pestaña muestra el contenido que haya agregado dentro de la estructura de carpetas del servidor para el proyecto de equipo.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > El **Explorador de Control de código fuente** pestaña muestra el contenido con ninguna otra preguntar porque se ha agregado la solución a una carpeta asignada en el sistema de archivos local. Si la solución se encontraba en una ubicación sin asignar, le solicita para especificar ubicaciones de carpeta en TFS y el sistema de archivos local.
5. En el **Explorador de Control de código fuente** ficha la **carpetas** panel, haga clic en el proyecto de equipo (por ejemplo, **ContactManager**) y, a continuación, haga clic en **insertar en el repositorio Los cambios pendientes**.
6. En el **insertar en el repositorio: archivos de código fuente** cuadro de diálogo, escriba un comentario y, a continuación, haga clic en **proteger**.

    ![](adding-content-to-source-control/_static/image9.png)

En este punto ha agregado la solución al control de código fuente en TFS.

## <a name="add-external-dependencies-to-source-control"></a>Agregar dependencias externas a Control de código fuente

Cuando se agrega un proyecto o solución al control de código fuente, también se agregará los archivos y carpetas en el proyecto o solución. Sin embargo, en muchos casos, proyectos y soluciones también se basan en las dependencias externas, como ensamblados locales para funcionar correctamente. Debe agregar cualquier estos recursos al control de código fuente para permitir que Team Build y otros miembros del equipo del desarrollador de compilan el código correctamente.

Por ejemplo, la estructura de carpetas para la solución de ejemplo Contact Manager incluye una carpeta denominada paquetes. Contiene el ensamblado y varios recursos de soporte para ADO.NET Entity Framework 4.1. La carpeta de paquetes no es parte de la solución póngase en contacto con el administrador, pero la solución no se compilará correctamente sin él. Para habilitar Team Build compilar la solución, debe agregar la carpeta de paquetes al control de código fuente.

> [!NOTE]
> La inclusión de una carpeta de paquetes es típica de lo que sucede al agregar el Entity Framework o recursos similares, a la solución utilizando la extensión de NuGet para Visual Studio 2010.


**Para agregar contenido no son del proyecto al control de código fuente**

1. Asegúrese de que los elementos que desee agregar (por ejemplo, la carpeta de paquetes) en una ubicación adecuada en una carpeta asignada en el sistema de archivos local.
2. En Visual Studio 2010, en la **Team Explorer** ventana, expanda el proyecto de equipo y, a continuación, haga doble clic en **Control de código fuente**.

    ![](adding-content-to-source-control/_static/image10.png)
3. En el **Explorador de Control de código fuente** ficha la **carpetas** panel, seleccione la carpeta que contiene el elemento o elementos desea agregar.
4. Haga clic en el **agregar elementos a la carpeta** botón.

    ![](adding-content-to-source-control/_static/image11.png)
5. En el **agregar al Control de código fuente** diálogo cuadro, seleccione la carpeta o los elementos que desea agregar y, a continuación, haga clic en **siguiente**.

    ![](adding-content-to-source-control/_static/image12.png)
6. En el **elementos excluidos** ficha, seleccione los elementos necesarios que se han excluido automáticamente (por ejemplo, ensamblados) y, a continuación, haga clic en **incluyen elementos**.

    ![](adding-content-to-source-control/_static/image13.png)
7. En el **elementos que desea agregar** , compruebe que se enumeran todos los archivos que van a incluir y, a continuación, haga clic en **finalizar**.

    ![](adding-content-to-source-control/_static/image14.png)
8. En el **Explorador de Control de código fuente** ventana, haga clic en el **proteger** botón.

    ![](adding-content-to-source-control/_static/image15.png)
9. En el **insertar en el repositorio: archivos de código fuente** cuadro de diálogo, escriba un comentario y, a continuación, haga clic en **proteger**.

En este momento, ha agregado las dependencias externas para la solución al control de código fuente.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo conectarse a un proyecto de equipo, asigne una estructura de carpetas y agregar contenido al control de código fuente. Para obtener más información sobre cómo trabajar con elementos en el control de código fuente, consulte [Control de versiones utilizando](https://msdn.microsoft.com/library/ms181368.aspx).

El siguiente tema, [configurar un servidor de compilación de TFS para la implementación Web](configuring-a-tfs-build-server-for-web-deployment.md), se describe cómo preparar un servidor TFS Team Build para compilar e implementar la solución.

## <a name="further-reading"></a>Información adicional

Para obtener información más completa sobre cómo trabajar con control de código fuente en TFS, consulte [Control de versiones utilizando](https://msdn.microsoft.com/library/ms181368.aspx).

>[!div class="step-by-step"]
[Anterior](creating-a-team-project-in-tfs.md)
[Siguiente](configuring-a-tfs-build-server-for-web-deployment.md)
