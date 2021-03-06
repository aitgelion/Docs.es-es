---
title: Interfaz web abierta para .NET (OWIN)
author: ardalis
description: Descubra la manera en que ASP.NET Core es compatible con la interfaz web abierta para .NET (OWIN), que permite que las aplicaciones web se desacoplen de servidores web.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/owin
ms.openlocfilehash: 1a6a49715840d66dc37465758d3a896af96e2976
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a>Introducción a la interfaz web abierta para .NET (OWIN)

Por [Steve Smith](https://ardalis.com/) y [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core es compatible con la interfaz web abierta para .NET (OWIN). OWIN permite que las aplicaciones web se desacoplen de los servidores web. Define una manera estándar para usar software intermedio en una canalización a fin de controlar las solicitudes y las respuestas asociadas. El software intermedio y las aplicaciones de ASP.NET Core pueden interoperar con aplicaciones, servidores y software intermedio basados en OWIN.

OWIN proporciona una capa de desacoplamiento que permite que dos marcos de trabajo con modelos de objetos dispares se usen juntos. El paquete `Microsoft.AspNetCore.Owin` proporciona dos implementaciones del adaptador:
- De ASP.NET Core a OWIN 
- De OWIN a ASP.NET Core

Esto permite que ASP.NET Core se hospede sobre un servidor/host compatible con OWIN, o bien que otros componentes compatibles con OWIN se ejecuten sobre ASP.NET Core.

Nota: El uso de estos adaptadores conlleva un costo de rendimiento. Las aplicaciones que solo usan componentes de ASP.NET Core no deben usar el paquete o adaptadores de OWIN.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>Ejecución de software intermedio de OWIN en la canalización de ASP.NET

La compatibilidad con OWIN de ASP.NET Core se implementa como parte del paquete `Microsoft.AspNetCore.Owin`. Puede importar compatibilidad con OWIN en el proyecto mediante la instalación de este paquete.

El software intermedio de OWIN cumple la [especificación de OWIN](http://owin.org/spec/spec/owin-1.0.0.html), que requiere una interfaz `Func<IDictionary<string, object>, Task>` y el establecimiento de determinadas claves (como `owin.ResponseBody`). En el siguiente software intermedio simple de OWIN se muestra "Hello World":

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

La firma de ejemplo devuelve un valor `Task` y acepta un valor `IDictionary<string, object>`, según los requisitos de OWIN.

En el código siguiente se muestra cómo agregar el software intermedio `OwinHello` (mostrado arriba) a la canalización ASP.NET con el método de extensión `UseOwin`.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

Puede configurar la realización de otras acciones en la canalización de OWIN.

> [!NOTE]
> Los encabezados de respuesta solo deben modificarse antes de la primera escritura en la secuencia de respuesta.

> [!NOTE]
> No se recomienda la realización de varias llamadas a `UseOwin` por motivos de rendimiento. Los componentes de OWIN funcionarán mejor si se agrupan.

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>Uso de hospedaje de ASP.NET en un servidor basado en OWIN

Los servidores basados en OWIN pueden hospedar aplicaciones de ASP.NET. Un servidor de este tipo es [Nowin](https://github.com/Bobris/Nowin), un servidor web de OWIN de .NET. En el ejemplo de este artículo, se ha incluido un proyecto que hace referencia a Nowin y lo usa para crear una interfaz `IServer` capaz de autohospedar ASP.NET Core.

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer` es una interfaz que requiere una propiedad `Features` y un método `Start`.

`Start` se encarga de configurar e iniciar el servidor, lo que en este caso se lleva a cabo mediante una serie de llamadas API fluidas que establecen direcciones analizadas desde IServerAddressesFeature. Tenga en cuenta que la configuración fluida de la variable `_builder` especifica que las solicitudes se controlarán mediante el valor `appFunc` definido anteriormente en el método. Se llama a este valor `Func` en cada solicitud para procesar las solicitudes entrantes.

Agregaremos también una extensión `IWebHostBuilder` para que sea fácil de agregar y configurar el servidor Nowin.

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

Después de hacer estas implementaciones, no hace falta nada más para ejecutar una aplicación ASP.NET mediante este servidor personalizado para llamar a la extensión en *Program.cs*:

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

Obtenga más información sobre los [servidores](servers/index.md) de ASP.NET.

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>Ejecución de ASP.NET Core en un servidor basado en OWIN y uso de su compatibilidad con WebSockets

Otro ejemplo de la manera en que ASP.NET Core puede aprovechar las características de los servidores basados en OWIN es el acceso a funciones como WebSockets. El servidor web de OWIN de .NET usado en el ejemplo anterior es compatible con WebSockets integrado, cuyas ventajas puede aprovechar la aplicación ASP.NET Core. En el ejemplo siguiente se muestra una aplicación web simple que admite WebSockets y devuelve todo lo que se envía al servidor a través de WebSockets.

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

Este [ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) se ha configurado con el mismo `NowinServer` que el anterior; la única diferencia es la manera en que la aplicación está configurada en su método `Configure`. La aplicación se muestra mediante una prueba que usa un [cliente WebSocket simple](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en):

![Cliente de prueba de WebSocket](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>Entorno de OWIN

Puede construir un entorno de OWIN mediante `HttpContext`.

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>Claves de OWIN

OWIN depende de un objeto `IDictionary<string,object>` para comunicar información mediante un intercambio de solicitud/respuesta HTTP. ASP.NET Core implementa las claves que se enumeran a continuación. Vea las [extensiones de la especificación principal](http://owin.org/#spec) y las [directrices principales y claves comunes de OWIN](http://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>Datos de solicitud (OWIN v1.0.0)

| Key               | Valor (tipo) | Description |
| ----------------- | ------------ | ----------- |
| owin.RequestScheme | `String` |  |
| owin.RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin.RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owin.RequestProtocol  | `String` | |    
| owin.RequestHeaders | `IDictionary<string,string[]>`  | |
| owin.RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>Datos de solicitud (OWIN v1.1.0)

| Key               | Valor (tipo) | Description |
| ----------------- | ------------ | ----------- |
| owin.RequestId | `String` | Optional |

### <a name="response-data-owin-v100"></a>Datos de respuesta (OWIN v1.0.0)

| Key               | Valor (tipo) | Description |
| ----------------- | ------------ | ----------- |
| owin.ResponseStatusCode | `int` | Optional |
| owin.ResponseReasonPhrase | `String` | Optional |
| owin.ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin.ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>Otros datos (OWIN v1.0.0)

| Key               | Valor (tipo) | Description |
| ----------------- | ------------ | ----------- |
| owin.CallCancelled | `CancellationToken` |  |
| owin.Version  | `String` | |   


### <a name="common-keys"></a>Claves comunes

| Key               | Valor (tipo) | Description |
| ----------------- | ------------ | ----------- |
| ssl.ClientCertificate | `X509Certificate` |  |
| ssl.LoadClientCertAsync  | `Func<Task>` | |    
| server.RemoteIpAddress  | `String` | |    
| server.RemotePort | `String` | |     
| server.LocalIpAddress  | `String` | |    
| server.LocalPort  | `String` | |    
| server.IsLocal  | `bool` | |    
| server.OnSendingHeaders  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| Key               | Valor (tipo) | Description |
| ----------------- | ------------ | ----------- |
| sendfile.SendAsync | Vea [Delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) (Signatura de delegado) | Por solicitud |


### <a name="opaque-v030"></a>Opaque v0.3.0

| Key               | Valor (tipo) | Description |
| ----------------- | ------------ | ----------- |
| opaque.Version | `String` |  |
| opaque.Upgrade | `OpaqueUpgrade` | Vea [Delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) (Signatura de delegado) |
| opaque.Stream | `Stream` |  |
| opaque.CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>WebSocket v0.3.0

| Key               | Valor (tipo) | Description |
| ----------------- | ------------ | ----------- |
| websocket.Version | `String` |  |
| websocket.Accept | `WebSocketAccept` | Vea [Delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) (Signatura de delegado) |
| websocket.AcceptAlt |  | Sin especificaciones |
| websocket.SubProtocol | `String` | Vea la [sección 4.2.2 de RFC6455](https://tools.ietf.org/html/rfc6455#section-4.2.2), paso 5.5 |
| websocket.SendAsync | `WebSocketSendAsync` | Vea [Delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) (Signatura de delegado)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | Vea [Delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) (Signatura de delegado)  |
| websocket.CloseAsync | `WebSocketCloseAsync` | Vea [Delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) (Signatura de delegado)  |
| websocket.CallCancelled | `CancellationToken` |  |
| websocket.ClientCloseStatus | `int` | Optional |
| websocket.ClientCloseDescription | `String` | Optional |

## <a name="additional-resources"></a>Recursos adicionales

* [Middleware](xref:fundamentals/middleware/index)
* [Servidores](xref:fundamentals/servers/index)
