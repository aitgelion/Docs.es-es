---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animar un Control UpdatePanel (C#) | Documentos de Microsoft
author: wenz
description: "El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control. Para el contenido de un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7e6d8954d2ec886994cdd723121e540b471131f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="animating-an-updatepanel-control-c"></a><span data-ttu-id="74877-104">Animar un Control UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="74877-104">Animating an UpdatePanel Control (C#)</span></span>
====================
<span data-ttu-id="74877-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="74877-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="74877-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="74877-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span></span>

> <span data-ttu-id="74877-107">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="74877-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="74877-108">Para el contenido de un UpdatePanel, existe un dispositivo extender especial que se basa principalmente en el marco de trabajo de animación: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="74877-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="74877-109">Este tutorial muestra cómo configurar una animación de este tipo para un UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="74877-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="74877-110">Información general</span><span class="sxs-lookup"><span data-stu-id="74877-110">Overview</span></span>

<span data-ttu-id="74877-111">El control de animación en el Kit de herramientas de Control de AJAX de ASP.NET no es simplemente un control sino un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="74877-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="74877-112">Para el contenido de un `UpdatePanel`, existe un dispositivo extender especial que se basa principalmente en el marco de trabajo de animación: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="74877-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="74877-113">Este tutorial muestra cómo configurar estas una animación para un `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="74877-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="74877-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="74877-114">Steps</span></span>

<span data-ttu-id="74877-115">El primer paso es como de costumbre incluir la `ScriptManager` en la página para que se carga la biblioteca de AJAX de ASP.NET y se puede utilizar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="74877-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

<span data-ttu-id="74877-116">La animación en este escenario se aplicarán a un ASP.NET `Wizard` control web que residen en un `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="74877-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="74877-117">Tres pasos (arbitrarios) proporcionan suficiente opciones para desencadenar las devoluciones de datos:</span><span class="sxs-lookup"><span data-stu-id="74877-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

<span data-ttu-id="74877-118">El marcado que sea necesario para la `UpdatePanelAnimationExtender` control es muy similar para el marcado usado para la `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="74877-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="74877-119">En el `TargetControlID` atributo proporcionamos el `ID` de la `UpdatePanel` a animar; dentro de la `UpdatePanelAnimationExtender` (control), el `<Animations>` elemento contiene el marcado XML de la animación.</span><span class="sxs-lookup"><span data-stu-id="74877-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="74877-120">Sin embargo, es una diferencia: la cantidad de eventos y controladores de eventos se limita en comparación con `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="74877-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="74877-121">Para `UpdatePanels`, sólo dos de ellos existe:</span><span class="sxs-lookup"><span data-stu-id="74877-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="74877-122">`<OnUpdated>`Cuando se ha actualizado el UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="74877-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="74877-123">`<OnUpdating>`Cuando inicia la actualización UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="74877-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="74877-124">En este escenario, el nuevo contenido de la `UpdatePanel` (después de la devolución de datos) serán fundido de entrada.</span><span class="sxs-lookup"><span data-stu-id="74877-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="74877-125">Éste es el formato necesario para:</span><span class="sxs-lookup"><span data-stu-id="74877-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

<span data-ttu-id="74877-126">Ahora cada vez que se produce un postback en UpdatePanel, el nuevo contenido del panel se atenúan sin problemas.</span><span class="sxs-lookup"><span data-stu-id="74877-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


<span data-ttu-id="74877-127">[![El siguiente paso del asistente se difuminado](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="74877-127">[![The next wizard step is fading in](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="74877-128">El siguiente paso del asistente se difuminado ([haga clic aquí para ver la imagen a tamaño completo](animating-an-updatepanel-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="74877-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="74877-129">[Anterior](changing-an-animation-using-client-side-code-cs.md)
[Siguiente](dynamically-controlling-updatepanel-animations-cs.md)</span><span class="sxs-lookup"><span data-stu-id="74877-129">[Previous](changing-an-animation-using-client-side-code-cs.md)
[Next](dynamically-controlling-updatepanel-animations-cs.md)</span></span>