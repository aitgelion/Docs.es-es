---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formateadores de contenido multimedia en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 9103574597df126a22e21a2f51815f608e46f47f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="46dd1-102">Formateadores de contenido multimedia en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="46dd1-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="46dd1-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="46dd1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="46dd1-104">Este tutorial muestra cómo admite formatos multimedia adicionales en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="46dd1-104">This tutorial shows how support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="46dd1-105">Tipos de medios de Internet</span><span class="sxs-lookup"><span data-stu-id="46dd1-105">Internet Media Types</span></span>

<span data-ttu-id="46dd1-106">Un tipo de medio, conocido también como un tipo MIME, identifica el formato de un elemento de datos.</span><span class="sxs-lookup"><span data-stu-id="46dd1-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="46dd1-107">En HTTP, tipos de medios describen el formato del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="46dd1-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="46dd1-108">Un tipo de medio consta de dos cadenas, un tipo y un subtipo.</span><span class="sxs-lookup"><span data-stu-id="46dd1-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="46dd1-109">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="46dd1-109">For example:</span></span>

- <span data-ttu-id="46dd1-110">texto/html</span><span class="sxs-lookup"><span data-stu-id="46dd1-110">text/html</span></span>
- <span data-ttu-id="46dd1-111">image/png</span><span class="sxs-lookup"><span data-stu-id="46dd1-111">image/png</span></span>
- <span data-ttu-id="46dd1-112">application/json</span><span class="sxs-lookup"><span data-stu-id="46dd1-112">application/json</span></span>

<span data-ttu-id="46dd1-113">Cuando un mensaje HTTP contiene un cuerpo de entidad, el encabezado Content-Type especifica el formato del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="46dd1-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="46dd1-114">Esto indica que el receptor a cómo analizar el contenido del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="46dd1-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="46dd1-115">Por ejemplo, si una respuesta HTTP contiene una imagen PNG, la respuesta podría tener los siguientes encabezados.</span><span class="sxs-lookup"><span data-stu-id="46dd1-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="46dd1-116">Cuando el cliente envía un mensaje de solicitud, puede incluir un encabezado de aceptación.</span><span class="sxs-lookup"><span data-stu-id="46dd1-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="46dd1-117">El encabezado Accept indica que el servidor de tipos de los archivos multimedia que el cliente espera el servidor.</span><span class="sxs-lookup"><span data-stu-id="46dd1-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="46dd1-118">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="46dd1-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="46dd1-119">Este encabezado indica al servidor que desea que el cliente HTML, XHTML o XML.</span><span class="sxs-lookup"><span data-stu-id="46dd1-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="46dd1-120">El tipo de medio determina cómo API Web serializa y deserializa el cuerpo del mensaje HTTP.</span><span class="sxs-lookup"><span data-stu-id="46dd1-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="46dd1-121">API Web tiene compatibilidad integrada para XML, JSON, BSON y datos de codificación de formulario, y puede admitir tipos de medios adicionales mediante la escritura de un *formateador del contenido multimedia*.</span><span class="sxs-lookup"><span data-stu-id="46dd1-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="46dd1-122">Para crear a un formateador de medios, que se derivan de una de estas clases:</span><span class="sxs-lookup"><span data-stu-id="46dd1-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="46dd1-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="46dd1-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="46dd1-124">Esta clase usa la lectura asincrónica y los métodos de escritura.</span><span class="sxs-lookup"><span data-stu-id="46dd1-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="46dd1-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="46dd1-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="46dd1-126">Esta clase se deriva de **elemento MediaTypeFormatter** pero utiliza métodos de lectura/escritura de sychronous.</span><span class="sxs-lookup"><span data-stu-id="46dd1-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="46dd1-127">Derivar de **BufferedMediaTypeFormatter** es más sencillo, porque no hay ningún código asincrónico, pero también significa que puede bloquear el subproceso que realiza la llamada durante la E/S.</span><span class="sxs-lookup"><span data-stu-id="46dd1-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="46dd1-128">Ejemplo: Crear a un formateador de medios CSV</span><span class="sxs-lookup"><span data-stu-id="46dd1-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="46dd1-129">En el ejemplo siguiente se muestra un formateador de tipo de medio que puede serializar un objeto del producto en un formato de valores separados por comas (CSV).</span><span class="sxs-lookup"><span data-stu-id="46dd1-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="46dd1-130">Este ejemplo utiliza el tipo de producto definido en el tutorial [crear una API Web que admite las operaciones CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="46dd1-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="46dd1-131">Aquí está la definición del objeto de producto:</span><span class="sxs-lookup"><span data-stu-id="46dd1-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="46dd1-132">Para implementar un formateador CSV, defina una clase que deriva de **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="46dd1-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="46dd1-133">En el constructor, agregue los tipos de medios que admite el formateador.</span><span class="sxs-lookup"><span data-stu-id="46dd1-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="46dd1-134">En este ejemplo, el formateador admite un único tipo de medio, &quot;texto o csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="46dd1-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="46dd1-135">Invalidar el **CanWriteType** método para indicar que escribe el formateador puede serializar:</span><span class="sxs-lookup"><span data-stu-id="46dd1-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="46dd1-136">En este ejemplo, el formateador puede serializar solo `Product` objetos así como las colecciones de `Product` objetos.</span><span class="sxs-lookup"><span data-stu-id="46dd1-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="46dd1-137">De forma similar, invalidar la **CanReadType** método para indicar que escribe el formateador puede deserializar.</span><span class="sxs-lookup"><span data-stu-id="46dd1-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="46dd1-138">En este ejemplo, el formateador no admite la deserialización, por lo que el método simplemente devuelve **false**.</span><span class="sxs-lookup"><span data-stu-id="46dd1-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="46dd1-139">Finalmente, se invalida la **WriteToStream** método.</span><span class="sxs-lookup"><span data-stu-id="46dd1-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="46dd1-140">Este método serializa un tipo mediante la escritura en una secuencia.</span><span class="sxs-lookup"><span data-stu-id="46dd1-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="46dd1-141">Si el formateador admite la deserialización, invalidar la **ReadFromStream** método.</span><span class="sxs-lookup"><span data-stu-id="46dd1-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="46dd1-142">Agregar a un formateador del contenido multimedia a la canalización de API Web</span><span class="sxs-lookup"><span data-stu-id="46dd1-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="46dd1-143">Para agregar un tipo de medio formateador para la canalización Web API, use la **formateadores** propiedad en el **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="46dd1-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="46dd1-144">Codificaciones de caracteres</span><span class="sxs-lookup"><span data-stu-id="46dd1-144">Character Encodings</span></span>

<span data-ttu-id="46dd1-145">Opcionalmente, un formateador de medios puede admitir varias codificaciones de caracteres, como UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="46dd1-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="46dd1-146">En el constructor, agregue uno o varios [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) tipos a la **SupportedEncodings** colección.</span><span class="sxs-lookup"><span data-stu-id="46dd1-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="46dd1-147">Coloque el primero de codificación predeterminado.</span><span class="sxs-lookup"><span data-stu-id="46dd1-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="46dd1-148">En el **WriteToStream** y **ReadFromStream** llamar métodos, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para seleccionar la codificación de caracteres preferida.</span><span class="sxs-lookup"><span data-stu-id="46dd1-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="46dd1-149">Este método devuelve los encabezados de solicitud con la lista de codificaciones admitidas.</span><span class="sxs-lookup"><span data-stu-id="46dd1-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="46dd1-150">Utilice el valor devuelto **codificación** al leer o escribir en la secuencia:</span><span class="sxs-lookup"><span data-stu-id="46dd1-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]