---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: "Controlar dinámicamente UpdatePanel animaciones (C#) | Documentos de Microsoft"
author: wenz
description: "El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Para el contenido de un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 0408556e322a46211388fbd557d7a967044129ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-controlling-updatepanel-animations-c"></a><span data-ttu-id="a9854-104">Controlar dinámicamente UpdatePanel animaciones (C#)</span><span class="sxs-lookup"><span data-stu-id="a9854-104">Dynamically Controlling UpdatePanel Animations (C#)</span></span>
====================
<span data-ttu-id="a9854-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a9854-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a9854-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a9854-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span></span>

> <span data-ttu-id="a9854-107">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="a9854-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a9854-108">Para el contenido de un UpdatePanel, existe un dispositivo extender especial que se basa principalmente en el marco de trabajo de animación: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="a9854-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="a9854-109">También puede trabajar junto con desencadenadores de UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="a9854-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="a9854-110">Información general</span><span class="sxs-lookup"><span data-stu-id="a9854-110">Overview</span></span>

<span data-ttu-id="a9854-111">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="a9854-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a9854-112">Para el contenido de un `UpdatePanel`, existe un dispositivo extender especial que se basa principalmente en el marco de trabajo de animación: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="a9854-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="a9854-113">También puede trabajar junto con `UpdatePanel` desencadenadores.</span><span class="sxs-lookup"><span data-stu-id="a9854-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="a9854-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="a9854-114">Steps</span></span>

<span data-ttu-id="a9854-115">El primer paso es como de costumbre incluir la `ScriptManager` en la página para que se carga la biblioteca de AJAX de ASP.NET y se puede utilizar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="a9854-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

<span data-ttu-id="a9854-116">La animación en este escenario se aplicarán a una presentación de la hora actual.</span><span class="sxs-lookup"><span data-stu-id="a9854-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="a9854-117">Esta información se puede escribir en una etiqueta con el `Page_Load()` (método), o (por simplicidad) se utiliza el código de la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="a9854-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

<span data-ttu-id="a9854-118">Además, se crea un botón para desencadenar la actualización de la hora:</span><span class="sxs-lookup"><span data-stu-id="a9854-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

<span data-ttu-id="a9854-119">Este código, a continuación, se coloca en el `<ContentTemplate>` sección de un `UpdatePanel` elemento.</span><span class="sxs-lookup"><span data-stu-id="a9854-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="a9854-120">El panel `UpdateMode` atributo debe establecerse en `"Conditional"`, ya que solo los desencadenadores pueden actualizar el contenido del panel.</span><span class="sxs-lookup"><span data-stu-id="a9854-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="a9854-121">En el `<Triggers>` sección de la `UpdatePanel`, un desencadenador de postback asincrónico creado y vinculado a la `Click` eventos del botón.</span><span class="sxs-lookup"><span data-stu-id="a9854-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="a9854-122">Por lo tanto, si el usuario hace clic en el botón, el `UpdatePanel` se actualiza.</span><span class="sxs-lookup"><span data-stu-id="a9854-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="a9854-123">Este es el marcado para el `UpdatePanel` control:</span><span class="sxs-lookup"><span data-stu-id="a9854-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

<span data-ttu-id="a9854-124">Por último, el `UpdatePanelAnimationExtender` debe configurarse: establecer el `TargetControlID` de atributo para el Id. del panel y definir una animación en el dispositivo extender.</span><span class="sxs-lookup"><span data-stu-id="a9854-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="a9854-125">Resaltar hace sentido, que crea un "nice" énfasis visual en el tiempo actualizado.</span><span class="sxs-lookup"><span data-stu-id="a9854-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="a9854-126">El marcado del extensor, a continuación, puede ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="a9854-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

<span data-ttu-id="a9854-127">Ejecute el archivo en el explorador.</span><span class="sxs-lookup"><span data-stu-id="a9854-127">Run the file in the browser.</span></span> <span data-ttu-id="a9854-128">Al hacer clic en el botón, se muestra la hora actual en el panel, siempre resaltar para la duración de un segundo.</span><span class="sxs-lookup"><span data-stu-id="a9854-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="a9854-129">[![La hora actual es difuminado](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a9854-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span></span>

<span data-ttu-id="a9854-130">La hora actual es difuminado ([haga clic aquí para ver la imagen a tamaño completo](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a9854-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a9854-131">[Anterior](animating-an-updatepanel-control-cs.md)
[Siguiente](adding-animation-to-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a9854-131">[Previous](animating-an-updatepanel-control-cs.md)
[Next](adding-animation-to-a-control-vb.md)</span></span>