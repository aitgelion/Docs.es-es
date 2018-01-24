---
title: Crear una API web con ASP.NET Core y Visual Studio para Windows
author: rick-anderson
description: Crear una API web con ASP.NET Core MVC y Visual Studio para Windows
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 234dbf73f9751ad4f995d6e7471d94f060fb15bf
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="1416c-103">Crear una API web con ASP.NET Core y Visual Studio para Windows</span><span class="sxs-lookup"><span data-stu-id="1416c-103">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="1416c-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="1416c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="1416c-105">En este tutorial se compila una API web para administrar una lista de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="1416c-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="1416c-106">No se crea una interfaz de usuario (UI).</span><span class="sxs-lookup"><span data-stu-id="1416c-106">A user interface (UI) is not created.</span></span>

<span data-ttu-id="1416c-107">Hay tres versiones de este tutorial:</span><span class="sxs-lookup"><span data-stu-id="1416c-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="1416c-108">Windows: API web con Visual Studio para Windows (este tutorial)</span><span class="sxs-lookup"><span data-stu-id="1416c-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="1416c-109">macOS: [API web con Visual Studio para Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="1416c-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="1416c-110">macOS, Linux y Windows: [API web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="1416c-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="1416c-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1416c-111">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="1416c-112">Vea [este PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) para la versión 1.1 de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1416c-112">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="1416c-113">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="1416c-113">Create the project</span></span>

<span data-ttu-id="1416c-114">En Visual Studio, seleccione el menú **Archivo**, > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="1416c-114">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="1416c-115">Seleccione la plantilla de proyecto **Aplicación web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="1416c-115">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="1416c-116">Asigne al proyecto el nombre `TodoApi` y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="1416c-116">Name the project `TodoApi` and select **OK**.</span></span>

![Cuadro de diálogo Nuevo proyecto](first-web-api/_static/new-project.png)

<span data-ttu-id="1416c-118">En el cuadro de diálogo **Nueva aplicación web ASP.NET Core - TodoApi**, seleccione la plantilla **API web**.</span><span class="sxs-lookup"><span data-stu-id="1416c-118">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="1416c-119">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="1416c-119">Select **OK**.</span></span> <span data-ttu-id="1416c-120">**No** seleccione **Habilitar compatibilidad con Docker**.</span><span class="sxs-lookup"><span data-stu-id="1416c-120">Do **not** select **Enable Docker Support**.</span></span>

![Cuadro de diálogo Nueva aplicación web ASP.NET con la plantilla de proyecto API web seleccionada en las plantillas de ASP.NET Core](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="1416c-122">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="1416c-122">Launch the app</span></span>

<span data-ttu-id="1416c-123">En Visual Studio, presione CTRL+F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1416c-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="1416c-124">Visual Studio inicia un explorador y navega hasta `http://localhost:port/api/values`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="1416c-124">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="1416c-125">En Chrome, Microsoft Edge y Firefox se muestra la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="1416c-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="1416c-126">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="1416c-126">Add a model class</span></span>

<span data-ttu-id="1416c-127">Un modelo es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1416c-127">A model is an object that represents the data in the app.</span></span> <span data-ttu-id="1416c-128">En este caso, el único modelo es una tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="1416c-128">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="1416c-129">Agregue una carpeta denominada "Modelos".</span><span class="sxs-lookup"><span data-stu-id="1416c-129">Add a folder named "Models".</span></span> <span data-ttu-id="1416c-130">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="1416c-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="1416c-131">Seleccione **Agregar** > **Nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="1416c-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="1416c-132">Asigne a la carpeta el nombre *Models*.</span><span class="sxs-lookup"><span data-stu-id="1416c-132">Name the folder *Models*.</span></span>

<span data-ttu-id="1416c-133">Nota: Las clases del modelo van en cualquier parte del proyecto.</span><span class="sxs-lookup"><span data-stu-id="1416c-133">Note: The model classes go anywhere in the project.</span></span> <span data-ttu-id="1416c-134">La carpeta *Models* se usa por convención para las clases de modelos.</span><span class="sxs-lookup"><span data-stu-id="1416c-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="1416c-135">Agregue una clase `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="1416c-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="1416c-136">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="1416c-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="1416c-137">Asigne a la clase el nombre `TodoItem` y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="1416c-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="1416c-138">Actualice la clase `TodoItem` por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="1416c-138">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="1416c-139">La base de datos genera el `Id` cuando se crea `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="1416c-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="1416c-140">Crear el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="1416c-140">Create the database context</span></span>

<span data-ttu-id="1416c-141">El *contexto de base de datos* es la clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="1416c-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="1416c-142">Esta clase se crea derivándola de la clase `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1416c-142">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="1416c-143">Agregue una clase `TodoContext`.</span><span class="sxs-lookup"><span data-stu-id="1416c-143">Add a `TodoContext` class.</span></span> <span data-ttu-id="1416c-144">Haga clic con el botón derecho en la carpeta *Modelos* y seleccione **Agregar** > **Clase**.</span><span class="sxs-lookup"><span data-stu-id="1416c-144">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="1416c-145">Asigne a la clase el nombre `TodoContext` y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="1416c-145">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="1416c-146">Reemplace la clase por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="1416c-146">Replace the class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="1416c-147">Adición de un controlador</span><span class="sxs-lookup"><span data-stu-id="1416c-147">Add a controller</span></span>

<span data-ttu-id="1416c-148">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta *Controladores*.</span><span class="sxs-lookup"><span data-stu-id="1416c-148">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="1416c-149">Seleccione **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="1416c-149">Select **Add** > **New Item**.</span></span> <span data-ttu-id="1416c-150">En el cuadro de diálogo **Agregar nuevo elemento**, seleccione la plantilla **Clase de controlador de API web**.</span><span class="sxs-lookup"><span data-stu-id="1416c-150">In the **Add New Item** dialog, select the **Web API Controller Class** template.</span></span> <span data-ttu-id="1416c-151">Asigne a la clase el nombre `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="1416c-151">Name the class `TodoController`.</span></span>

![Cuadro de diálogo Agregar nuevo elemento con la palabra "controlador" en el cuadro de búsqueda y la opción Clase de controlador de API web seleccionada](first-web-api/_static/new_controller.png)

<span data-ttu-id="1416c-153">Reemplace la clase por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="1416c-153">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="1416c-154">Iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="1416c-154">Launch the app</span></span>

<span data-ttu-id="1416c-155">En Visual Studio, presione CTRL+F5 para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1416c-155">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="1416c-156">Visual Studio inicia un explorador y navega hasta `http://localhost:port/api/values`, donde *port* es un número de puerto elegido aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="1416c-156">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="1416c-157">Vaya al controlador `Todo` en `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="1416c-157">Navigate to the `Todo` controller at `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
