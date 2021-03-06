---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: "Permitir que sólo determinados caracteres en un cuadro de texto (C#) | Documentos de Microsoft"
author: wenz
description: "Controles de validación de ASP.NET pueden asegurarse de que sólo ciertos caracteres se permiten en proporcionados por el usuario. Esto todavía no impide que los usuarios escriban en válido..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: 246c3b5dd55ceb0f47ad1f4982ae5b3bf855e747
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a>Permitir que sólo determinados caracteres en un cuadro de texto (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)

> Controles de validación de ASP.NET pueden asegurarse de que sólo ciertos caracteres se permiten en proporcionados por el usuario. Sin embargo esto todavía no impedir que los usuarios escribir caracteres no válidos e intentar enviar el formulario.


## <a name="overview"></a>Información general

Controles de validación de ASP.NET pueden asegurarse de que sólo ciertos caracteres se permiten en proporcionados por el usuario. Sin embargo esto todavía no impedir que los usuarios escribir caracteres no válidos e intentar enviar el formulario.

## <a name="steps"></a>Pasos

El Kit de herramientas de Control de AJAX de ASP.NET contiene el `FilteredTextBox` control que amplía un cuadro de texto. Una vez activado, sólo un determinado conjunto de caracteres puede especificarse en el campo.

Para que funcione, primero necesitamos como de costumbre ASP.NET AJAX `ScriptManager` que carga las bibliotecas de JavaScript que también son usadas por el Kit de herramientas de Control de AJAX de ASP.NET:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

A continuación, necesitamos un cuadro de texto:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

Por último, el `FilteredTextBoxExtender` control se encarga de restringir que los caracteres que el usuario puede escribir. En primer lugar, establezca el `TargetControlID` atribuir a la `ID` de la `TextBox` control. A continuación, elija uno de los contadores `FilterType` valores:

- `Custom`de forma predeterminada; tendrá que proporcionar una lista de caracteres válidos
- `LowercaseLetters`solo minúsculas
- `Numbers`solo los dígitos
- `UppercaseLetters`sólo las letras mayúsculas

Si el `Custom FilterType` se utiliza, el `ValidChars` propiedad debe configurarse y proporcionar una lista de caracteres que pueden escribirse. Por cierto: si se intenta pegar texto en el cuadro de texto, se quitan todos los caracteres no válidos.

Este es el marcado para la `FilteredTextBoxExtender` control que solo permita dígitos (algo que también habría sido posible con `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

Ejecutar la página y pruebe a escribir una carta si JavaScript está habilitado, no funcionará; Sin embargo, dígitos aparecen en la página. Sin embargo, observe que la protección `FilteredTextBox` proporciona no es infalibles: si JavaScript está habilitado, los datos pueden escribirse en el cuadro de texto, por lo que debe utilizar medios de validación adicional, es decir, ASP. Controles de validación de NET.


[![Pueden especificarse únicamente dígitos](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)

Pueden especificarse únicamente dígitos ([haga clic aquí para ver la imagen a tamaño completo](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))

>[!div class="step-by-step"]
[Siguiente](allowing-only-certain-characters-in-a-text-box-vb.md)
