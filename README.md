## Atmosphere for Play!

This project brings the [Atmosphere Framework](https://github.com/Atmosphere/atmosphere) to the [Play!](http://www.playframework.com/) Framework. It support ALL Atmosphere's modules like Runtime, Jersey and Socket.IO.

Official release soon with more samples!

## Try it!

Fork the workspace and go under samples/chat or samples/jersey, or download the sample's binaries [Chat]() [Jersey](http://search.maven.org/#search%7Cga%7C1%7Catmosphere-play)

```bash
   % unzip atmosphere-play-<name>-.zip
   % chmod a+x ./start
   % ./start
```

Download [Atmosphere Play!](http://search.maven.org/#search%7Cga%7C1%7Catmosphere-play) or use Maven

```xml
     <dependency>
         <groupId>org.atmosphere</groupId>
         <artifactId>atmosphere-play</artifactId>
         <version>1.0.0.beta1</version>
     </dependency>
```

## WebSockets, Server Side Events, Streaming and Long-Polling transparently supported!

Server side using atmosphere-runtime
```java
@ManagedService(path = "/chat")
public class Chat {
    private final ObjectMapper mapper = new ObjectMapper();

    @Message
    public String onMessage(String message) throws IOException {
        return mapper.writeValueAsString(mapper.readValue(message, Data.class));
    }

}
```

Server side using atmosphere-jersey
```java
 @Path("/chat")
 public class ChatResource {

     /**
      * Suspend the response without writing anything back to the client.
      * @return a white space
      */
     @Suspend(contentType = "application/json")
     @GET
     public String suspend() {
         return "";
     }

     /**
      * Broadcast the received message object to all suspended response. Do not write back the message to the calling connection.
      * @param message a {@link Message}
      * @return a {@link Response}
      */
     @Broadcast(writeEntity = false)
     @POST
     @Produces("application/json")
     public Response broadcast(Message message) {
         return new Response(message.getAuthor(), message.getMessage());
     }
}
```

and on the client side,
```js
            $(function () {
                "use strict";

                var header = $('#header');
                var content = $('#content');
                var input = $('#input');
                var status = $('#status');
                var myName = false;
                var author = null;
                var logged = false;
                var socket = $.atmosphere;
                var subSocket;
                var transport = 'websocket';

                // We are now ready to cut the request
                var request = { url: document.location.toString() + 'chat',
                    contentType : "application/json",
                    logLevel : 'debug',
                    transport : transport ,
                    trackMessageLength : true,
                    fallbackTransport: 'websocket'};


                request.onMessage = function (response) {

                     // do something
                };

                request.onClose = function(response) {
                };

                request.onError = function(response) {
                };

                subSocket = socket.subscribe(request);
```

