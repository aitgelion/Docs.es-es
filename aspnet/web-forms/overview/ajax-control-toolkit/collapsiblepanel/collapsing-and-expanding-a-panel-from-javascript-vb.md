---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Contraer y expandir un Panel desde JavaScript (VB) | Documentos de Microsoft
author: wenz
description: El control de CollapsiblePanel en el Kit de herramientas de Control de AJAX de ASP.NET extiende un panel y le proporciona la capacidad de expandir o contraer su contenido un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 6adca6771042cad71139977496f985cb8dac63aa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a>Contraer y expandir un Panel desde JavaScript (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)

> El control de CollapsiblePanel en el Kit de herramientas de Control de AJAX de ASP.NET extiende un panel y le proporciona la capacidad para contraer su contenido y expanda de nuevo. Estas dos acciones también pueden realizarse desde código de JavaScript personalizado.


## <a name="overview"></a>Información general

El control de CollapsiblePanel en el Kit de herramientas de Control de AJAX de ASP.NET extiende un panel y le proporciona la capacidad para contraer su contenido y expanda de nuevo. Estas dos acciones también pueden realizarse desde código de JavaScript personalizado.

## <a name="steps"></a>Pasos

En primer lugar, cree una nueva página ASP.NET e incluya la `ScriptManager` dentro del `<form>` elemento. Esto carga la biblioteca de AJAX de ASP.NET y es necesario para el Kit de herramientas de Control:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

A continuación, cree un panel con algún texto, por lo que puede verse el efecto de expandir y contraer:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

Como puede ver, el panel hace referencia a una clase CSS que se muestra aquí (y básicamente define un color de fondo y el ancho del panel):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

El `CollapsiblePanelExtender` control requiere el `TargetControlID` atributo para que el Kit de herramientas sepa cuál de los paneles para contraer o expandir tras la solicitud:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

Desafortunadamente, el dispositivo extender actualmente no expone una API específica para contraer o expandir el panel, pero algunos métodos sin documentar llevará a cabo. En primer lugar, agregue tres botones HTML a la página que, a continuación, desencadenará el JavaScript del lado cliente para contraer o expandir el contenido del panel:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

En el código de JavaScript del lado cliente (partió `<script type="text/javascript">`), el `$find()` método debe usarse para tener acceso a la `CollapsiblePanelExtender`. `$find("cpe")`Devuelve una referencia a él. Desde aquí, los métodos específicos resolverá la tarea en cuestión.

El método de apertura (expandir) se denomina el panel `_doOpen()`; el código siguiente implementa la `doOpen()` función se llama cuando se hace clic en el primer botón:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

Para cerrar o contraer el panel, el `_doClose()` método debe ejecutarse. Por lo que cuando el usuario hace clic en el segundo botón, se llama al código de JavaScript siguiente:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

El tercer botón alterna el estado del panel: de contraer para expandir y viceversa. El `CollapsiblePanelExtender` expone el `toggle()` método que hace exactamente eso: invierte el estado del panel. Sin embargo, también es otro método (que se usa internamente en el `toggle()` (método)): el `get_Collapsed()` método de la `CollapsiblePanelExtender()` nos indica si el panel está contraído o no. Dependiendo del valor devuelto de esta función, el panel es, a continuación, ya sea expandido (`_doOpen()` método) o contraer (`_doClose()`) método:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


[![El tercer botón cambia el estado del panel: desde contraído a expandido y viceversa](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)

El tercer botón cambia el estado del panel: desde contraído a expandido y viceversa ([haga clic aquí para ver la imagen a tamaño completo](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](collapsing-and-expanding-a-panel-from-javascript-cs.md)
