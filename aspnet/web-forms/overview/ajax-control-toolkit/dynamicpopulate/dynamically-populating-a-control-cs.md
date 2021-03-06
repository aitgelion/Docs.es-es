---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: "Rellenar dinámicamente un Control (C#) | Documentos de Microsoft"
author: wenz
description: "El control DynamicPopulate en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de la página) y rellena el valor resultante en un control de destino en t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a1868a0e4cec4a95d4175ce255fea2e200692075
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-c"></a>Rellenar dinámicamente un Control (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)

> El control DynamicPopulate en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de la página) y rellena el valor resultante en un control de destino en la página, sin una actualización de página.


## <a name="overview"></a>Información general

El `DynamicPopulate` control en el Kit de herramientas de Control de AJAX de ASP.NET llama a un servicio web (o el método de la página) y rellena el valor resultante en un control de destino en la página, sin una actualización de página. Este tutorial muestra cómo configurar esta opción.

## <a name="steps"></a>Pasos

En primer lugar, necesita un servicio Web de ASP.NET que implementa el método al que llamar `DynamicPopulate`. La clase de servicio web requiere la `ScriptService` atributo que se define en `Microsoft.Web.Script.Services`; en caso contrario, AJAX de ASP.NET no se puede crear el proxy de JavaScript del lado cliente para el servicio web y a su vez es necesario para `DynamicPopulate`.

El método web debe contar con que un argumento de tipo string, denominado `contextKey`, puesto que el `DynamicPopulate` control envía una parte de la información de contexto con cada llamada de servicio web. El siguiente servicio web devuelve la fecha actual en un formato representado por la `contextKey` argumento:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

El servicio web, a continuación, se guarda como `DynamicPopulate.cs.asmx`. Como alternativa, podría implementar el `getDate()` método como un método de página dentro de la página ASP.NET real con el `DynamicPopulate` control.

En el paso siguiente, cree un nuevo archivo ASP.NET. Como siempre, el primer paso es incluir la `ScriptManager` en la página actual para cargar la biblioteca de AJAX de ASP.NET y para realizar el trabajo del Kit de herramientas de Control:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

A continuación, agregue un control de etiqueta (por ejemplo mediante el control HTML del mismo nombre, o la &lt; `asp:Label`  / &gt; control web) que más adelante mostrará el resultado de llamar al servicio web.

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

Un botón HTML (como un control HTML, puesto que no se requiere un postback al servidor), a continuación, se usará para desencadenar la población dinámica:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

Por último, necesitamos la `DynamicPopulateExtender` control para conexión las cosas. Los siguientes atributos se establecerá (excepto los obvios, `ID` y `runat` = `"server"`):

- `TargetControlID`dónde colocar el resultado de llamar al servicio web
- `ServicePath`ruta de acceso al servicio web (omitir si desea utilizar un método de página)
- `ServiceMethod`nombre del método web o método de página
- `ContextKey`información de contexto en que se enviará al servicio web
- `PopulateTriggerControlID`elemento que desencadena la llamada del servicio web
- `ClearContentsDuringUpdate`Si desea vaciar el elemento de destino durante la llamada de servicio web

Como puede ver, el control requiere cierta información pero puesta en marcha de todo lo que es bastante sencilla. Este es el marcado para el `DynamicPopulateExtender` control en el escenario actual:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

Ejecute la página ASP.NET en el explorador y haga clic en el botón; recibirá la fecha actual en formato de año de días del mes.


[![Al hacer clic en el botón recupera la fecha del servidor](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

Al hacer clic en el botón recupera la fecha del servidor ([haga clic aquí para ver la imagen a tamaño completo](dynamically-populating-a-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[Siguiente](dynamically-populating-a-control-using-javascript-code-cs.md)
