---
uid: signalr/overview/performance/scaleout-in-signalr
title: "Introducción a la ampliación de SignalR | Documentos de Microsoft"
author: MikeWasson
description: "Versiones de software emplea en este tema Visual Studio 2013 .NET 4.5 SignalR las versiones anteriores de la versión 2 de este tema para obtener información acerca de las versiones anteriores de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: f1d15250682305f6d0512b72bd2e40cb4a8a18e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-scaleout-in-signalr"></a><span data-ttu-id="ee46a-103">Introducción a la ampliación de SignalR</span><span class="sxs-lookup"><span data-stu-id="ee46a-103">Introduction to Scaleout in SignalR</span></span>
====================
<span data-ttu-id="ee46a-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="ee46a-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="ee46a-105">Versiones de software que se usa en este tema</span><span class="sxs-lookup"><span data-stu-id="ee46a-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="ee46a-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="ee46a-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="ee46a-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="ee46a-107">.NET 4.5</span></span>
> - <span data-ttu-id="ee46a-108">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="ee46a-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="ee46a-109">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="ee46a-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="ee46a-110">Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="ee46a-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="ee46a-111">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="ee46a-111">Questions and comments</span></span>
> 
> <span data-ttu-id="ee46a-112">Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="ee46a-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="ee46a-113">Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="ee46a-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="ee46a-114">En general, hay dos formas de escalar una aplicación web: *escalar verticalmente* y *escalar horizontalmente*.</span><span class="sxs-lookup"><span data-stu-id="ee46a-114">In general, there are two ways to scale a web application: *scale up* and *scale out*.</span></span>

- <span data-ttu-id="ee46a-115">Escalar verticalmente mediante un servidor más grande (o una máquina virtual mayor) con más de RAM, CPU, etcetera de medios.</span><span class="sxs-lookup"><span data-stu-id="ee46a-115">Scale up means using a larger server (or a larger VM) with more RAM, CPUs, etc.</span></span>
- <span data-ttu-id="ee46a-116">Escalar horizontalmente significa agregar más servidores para administrar la carga.</span><span class="sxs-lookup"><span data-stu-id="ee46a-116">Scale out means adding more servers to handle the load.</span></span>

<span data-ttu-id="ee46a-117">El problema con el escalado es que rápidamente se alcanza un límite en el tamaño de la máquina.</span><span class="sxs-lookup"><span data-stu-id="ee46a-117">The problem with scaling up is that you quickly hit a limit on the size of the machine.</span></span> <span data-ttu-id="ee46a-118">Además, deberá escalar horizontalmente. Sin embargo, al escalar horizontalmente, los clientes pueden obtener enrutar a diferentes servidores.</span><span class="sxs-lookup"><span data-stu-id="ee46a-118">Beyond that, you need to scale out. However, when you scale out, clients can get routed to different servers.</span></span> <span data-ttu-id="ee46a-119">Un cliente que está conectado a un servidor no recibirá mensajes enviados desde otro servidor.</span><span class="sxs-lookup"><span data-stu-id="ee46a-119">A client that is connected to one server will not receive messages sent from another server.</span></span>

![](scaleout-in-signalr/_static/image1.png)

<span data-ttu-id="ee46a-120">Una solución consiste en reenviar los mensajes entre servidores, mediante un componente denominado un *backplane*.</span><span class="sxs-lookup"><span data-stu-id="ee46a-120">One solution is to forward messages between servers, using a component called a *backplane*.</span></span> <span data-ttu-id="ee46a-121">Con un backplane habilitada, cada instancia de la aplicación envía mensajes al backplane y backplane reenvía a las demás instancias de aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee46a-121">With a backplane enabled, each application instance sends messages to the backplane, and the backplane forwards them to the other application instances.</span></span> <span data-ttu-id="ee46a-122">(En electrónica, un backplane es un grupo de conectores paralelos.</span><span class="sxs-lookup"><span data-stu-id="ee46a-122">(In electronics, a backplane is a group of parallel connectors.</span></span> <span data-ttu-id="ee46a-123">Por analogía, un backplane de SignalR conecta varios servidores.)</span><span class="sxs-lookup"><span data-stu-id="ee46a-123">By analogy, a SignalR backplane connects multiple servers.)</span></span>

![](scaleout-in-signalr/_static/image2.png)

<span data-ttu-id="ee46a-124">SignalR proporciona actualmente tres paneles posteriores:</span><span class="sxs-lookup"><span data-stu-id="ee46a-124">SignalR currently provides three backplanes:</span></span>

- <span data-ttu-id="ee46a-125">**Bus de servicio de Azure**.</span><span class="sxs-lookup"><span data-stu-id="ee46a-125">**Azure Service Bus**.</span></span> <span data-ttu-id="ee46a-126">Bus de servicio es una infraestructura de mensajería que permite que los componentes enviar mensajes en modo de correspondencia imprecisa.</span><span class="sxs-lookup"><span data-stu-id="ee46a-126">Service Bus is a messaging infrastructure that allows components to send messages in a loosely coupled way.</span></span>
- <span data-ttu-id="ee46a-127">**Redis**.</span><span class="sxs-lookup"><span data-stu-id="ee46a-127">**Redis**.</span></span> <span data-ttu-id="ee46a-128">Redis es un almacén de clave y valor en memoria.</span><span class="sxs-lookup"><span data-stu-id="ee46a-128">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="ee46a-129">Redis admite un patrón de publicación/suscripción ("pub/sub") para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="ee46a-129">Redis supports a publish/subscribe ("pub/sub") pattern for sending messages.</span></span>
- <span data-ttu-id="ee46a-130">**SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="ee46a-130">**SQL Server**.</span></span> <span data-ttu-id="ee46a-131">El backplane de SQL Server escribe mensajes en tablas SQL.</span><span class="sxs-lookup"><span data-stu-id="ee46a-131">The SQL Server backplane writes messages to SQL tables.</span></span> <span data-ttu-id="ee46a-132">El backplane utiliza a Service Broker para la mensajería eficaz.</span><span class="sxs-lookup"><span data-stu-id="ee46a-132">The backplane uses Service Broker for efficient messaging.</span></span> <span data-ttu-id="ee46a-133">Sin embargo, también funciona si Service Broker no está habilitado.</span><span class="sxs-lookup"><span data-stu-id="ee46a-133">However, it also works if Service Broker is not enabled.</span></span>

<span data-ttu-id="ee46a-134">Si implementa la aplicación en Azure, considere el uso del backplane de Redis utilizando [Azure Redis Cache](https://azure.microsoft.com/services/cache/).</span><span class="sxs-lookup"><span data-stu-id="ee46a-134">If you deploy your application on Azure, consider using the Redis backplane using [Azure Redis Cache](https://azure.microsoft.com/services/cache/).</span></span> <span data-ttu-id="ee46a-135">Si va a implementar en su propio conjunto de servidores, considere la posibilidad de SQL Server o planos posteriores de Redis.</span><span class="sxs-lookup"><span data-stu-id="ee46a-135">If you are deploying to your own server farm, consider the SQL Server or Redis backplanes.</span></span>

<span data-ttu-id="ee46a-136">Los siguientes temas contienen tutoriales paso a paso para cada backplane:</span><span class="sxs-lookup"><span data-stu-id="ee46a-136">The following topics contain step-by-step tutorials for each backplane:</span></span>

- [<span data-ttu-id="ee46a-137">Escalabilidad horizontal de SignalR con Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="ee46a-137">SignalR Scaleout with Azure Service Bus</span></span>](scaleout-with-windows-azure-service-bus.md)
- [<span data-ttu-id="ee46a-138">Escalabilidad horizontal de SignalR con Redis</span><span class="sxs-lookup"><span data-stu-id="ee46a-138">SignalR Scaleout with Redis</span></span>](scaleout-with-redis.md)
- [<span data-ttu-id="ee46a-139">Escalabilidad horizontal de SignalR con SQL Server</span><span class="sxs-lookup"><span data-stu-id="ee46a-139">SignalR Scaleout with SQL Server</span></span>](scaleout-with-sql-server.md)

## <a name="implementation"></a><span data-ttu-id="ee46a-140">Implementación</span><span class="sxs-lookup"><span data-stu-id="ee46a-140">Implementation</span></span>

<span data-ttu-id="ee46a-141">En SignalR, cada mensaje se envía a través de un bus de mensajes.</span><span class="sxs-lookup"><span data-stu-id="ee46a-141">In SignalR, every message is sent through a message bus.</span></span> <span data-ttu-id="ee46a-142">Implementa un bus de mensajes la [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfaz, que proporciona una abstracción de publicación/suscripción.</span><span class="sxs-lookup"><span data-stu-id="ee46a-142">A message bus implements the [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, which provides a publish/subscribe abstraction.</span></span> <span data-ttu-id="ee46a-143">Los paneles posteriores de trabajo si se reemplaza el valor predeterminado **IMessageBus** con un bus, diseñado para ese backplane.</span><span class="sxs-lookup"><span data-stu-id="ee46a-143">The backplanes work by replacing the default **IMessageBus** with a bus designed for that backplane.</span></span> <span data-ttu-id="ee46a-144">Por ejemplo, el bus de mensajes para Redis es [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), y utiliza Redis [pub/sub](http://redis.io/topics/pubsub) mecanismo para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="ee46a-144">For example, the message bus for Redis is [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), and it uses the Redis [pub/sub](http://redis.io/topics/pubsub) mechanism to send and receive messages.</span></span>

<span data-ttu-id="ee46a-145">Cada instancia del servidor se conecta al backplane a través del bus.</span><span class="sxs-lookup"><span data-stu-id="ee46a-145">Each server instance connects to the backplane through the bus.</span></span> <span data-ttu-id="ee46a-146">Cuando se envía un mensaje, entra en el plano posterior y el backplane lo envía a todos los servidores.</span><span class="sxs-lookup"><span data-stu-id="ee46a-146">When a message is sent, it goes to the backplane, and the backplane sends it to every server.</span></span> <span data-ttu-id="ee46a-147">Cuando un servidor recibe un mensaje del backplane, coloca el mensaje en su memoria caché local.</span><span class="sxs-lookup"><span data-stu-id="ee46a-147">When a server gets a message from the backplane, it puts the message in its local cache.</span></span> <span data-ttu-id="ee46a-148">El servidor, a continuación, envía mensajes a los clientes desde su caché local.</span><span class="sxs-lookup"><span data-stu-id="ee46a-148">The server then delivers messages to clients from its local cache.</span></span>

<span data-ttu-id="ee46a-149">Para cada conexión de cliente, progreso del cliente en la secuencia de mensajes de lectura se realiza un seguimiento mediante un cursor.</span><span class="sxs-lookup"><span data-stu-id="ee46a-149">For each client connection, the client's progress in reading the message stream is tracked using a cursor.</span></span> <span data-ttu-id="ee46a-150">(Un cursor representa una posición en la secuencia de mensajes). Si un cliente se desconecta y, a continuación, se vuelve a conectar, solicita el bus de los mensajes que han llegado después del valor de cursor del cliente.</span><span class="sxs-lookup"><span data-stu-id="ee46a-150">(A cursor represents a position in the message stream.) If a client disconnects and then reconnects, it asks the bus for any messages that arrived after the client's cursor value.</span></span> <span data-ttu-id="ee46a-151">Lo mismo ocurre cuando se utiliza una conexión [sondeo largo](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="ee46a-151">The same thing happens when a connection uses [long polling](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="ee46a-152">Una vez completada una solicitud de sondeo largo, el cliente abre una nueva conexión y pide a los mensajes recibidos después del cursor.</span><span class="sxs-lookup"><span data-stu-id="ee46a-152">After a long poll request completes, the client opens a new connection and asks for messages that arrived after the cursor.</span></span>

<span data-ttu-id="ee46a-153">El funcionamiento de mecanismo de cursor incluso si un cliente se enruta a un servidor diferente en volver a conectar.</span><span class="sxs-lookup"><span data-stu-id="ee46a-153">The cursor mechanism works even if a client is routed to a different server on reconnect.</span></span> <span data-ttu-id="ee46a-154">El backplane es consciente de todos los servidores y no importa qué servidor se conecta un cliente.</span><span class="sxs-lookup"><span data-stu-id="ee46a-154">The backplane is aware of all the servers, and it doesn't matter which server a client connects to.</span></span>

## <a name="limitations"></a><span data-ttu-id="ee46a-155">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="ee46a-155">Limitations</span></span>

<span data-ttu-id="ee46a-156">Con un backplane, el rendimiento máximo del mensaje es menor que lo es cuando los clientes comunicarse directamente con un solo nodo de servidor.</span><span class="sxs-lookup"><span data-stu-id="ee46a-156">Using a backplane, the maximum message throughput is lower than it is when clients talk directly to a single server node.</span></span> <span data-ttu-id="ee46a-157">Eso es porque el backplane reenvía todos los mensajes para cada nodo, por lo que el backplane puede convertirse en un cuello de botella.</span><span class="sxs-lookup"><span data-stu-id="ee46a-157">That's because the backplane forwards every message to every node, so the backplane can become a bottleneck.</span></span> <span data-ttu-id="ee46a-158">Si esta limitación es un problema depende de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee46a-158">Whether this limitation is a problem depends on the application.</span></span> <span data-ttu-id="ee46a-159">Por ejemplo, estos son algunos escenarios típicos de SignalR:</span><span class="sxs-lookup"><span data-stu-id="ee46a-159">For example, here are some typical SignalR scenarios:</span></span>

- <span data-ttu-id="ee46a-160">[Difusión de servidor](../getting-started/tutorial-server-broadcast-with-signalr.md) (p. ej., cotizaciones): planos posteriores funcionan bien para este escenario, debido a que el servidor controla la velocidad a la que se envían mensajes.</span><span class="sxs-lookup"><span data-stu-id="ee46a-160">[Server broadcast](../getting-started/tutorial-server-broadcast-with-signalr.md) (e.g., stock ticker): Backplanes work well for this scenario, because the server controls the rate at which messages are sent.</span></span>
- <span data-ttu-id="ee46a-161">[Cliente a](../getting-started/tutorial-getting-started-with-signalr.md) (p. ej., chat): en este escenario, el plano posterior puede ser un cuello de botella si ajusta el número de mensajes con el número de clientes; es decir, si la tasa de mensajes aumenta proporcionalmente como más los clientes se unen.</span><span class="sxs-lookup"><span data-stu-id="ee46a-161">[Client-to-client](../getting-started/tutorial-getting-started-with-signalr.md) (e.g., chat): In this scenario, the backplane might be a bottleneck if the number of messages scales with the number of clients; that is, if the rate of messages grows proportionally as more clients join.</span></span>
- <span data-ttu-id="ee46a-162">[En tiempo real de alta frecuencia](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (por ejemplo, juegos en tiempo real): no se recomienda un backplane para este escenario.</span><span class="sxs-lookup"><span data-stu-id="ee46a-162">[High-frequency realtime](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (e.g., real-time games): A backplane is not recommended for this scenario.</span></span>

## <a name="enabling-tracing-for-signalr-scaleout"></a><span data-ttu-id="ee46a-163">Habilitar el seguimiento de ampliación de SignalR</span><span class="sxs-lookup"><span data-stu-id="ee46a-163">Enabling Tracing For SignalR Scaleout</span></span>

<span data-ttu-id="ee46a-164">Para habilitar el seguimiento de los planos posteriores, agregue las siguientes secciones en el archivo web.config, bajo la raíz de **configuración** elemento:</span><span class="sxs-lookup"><span data-stu-id="ee46a-164">To enable tracing for the backplanes, add the following sections to the web.config file, under the root **configuration** element:</span></span>

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]