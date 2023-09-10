## 2.1.1: Websocket Server

WebSocket servers provide a powerful tool for building real-time, interactive web applications. While they operate alongside traditional web servers, they serve a distinctly different role and enable new kinds of user experiences.

They do so while maintaining the fundamental principles of server-side web development, like the request-response cycle of HTTP and the short-lived nature of individual application instances.


### The Need of WebSocket Servers and Their Role:


WebSockets provide a protocol for real-time, bidirectional communication between a client (typically a web browser) and a server. This is in contrast to the HTTP protocol, which is unidirectional - the client sends a request and the server sends back a response. WebSockets, on the other hand, keep the connection open, allowing data to be sent back and forth as needed. This is crucial for applications that require real-time updates, such as chat apps, live sports updates, and collaborative editing tools.

<figure style="text-align: center">
  <img src="img_18.png" alt="websocket server in a laravel app">
  <figcaption style="text-align: center;">Figure 2.1.1.1: Websocket server in a laravel app</figcaption>
</figure>

### WebSocket Server Process:
 

A WebSocket server can either be a standalone process running on the same operating system as the web server (like the Laravel Websockets package) or it could be a separate service running elsewhere (like Pusher). The WebSocket server's role is to manage these long-lived WebSocket connections, handling incoming messages and broadcasting messages to clients.

The following is a diagram of the WebSocket Server running as process in the same OS as the Web Server:

<figure style="text-align: center">
  <img src="img_17.png" alt="websocket server process vs web server process">
  <figcaption style="text-align: center;">Figure 2.1.1.2: Websocket process vs web server process</figcaption>
</figure>

The following is a diagram of the WebSocket Server and the Web Server each running on a different OS:

<figure style="text-align: center">
  <img src="img_16.png" alt="websocket server in a separate OS">
  <figcaption style="text-align: center;">Figure 2.1.1.3: Websocket server in a separate OS</figcaption>
</figure>

### Laravel Applications and WebSocket Connections:

   
While a WebSocket connection between a client and the WebSocket server is long-lived, a Laravel application still adheres to its typical lifecycle: it receives an HTTP request, sends back an HTTP response, and then terminates. The presence of a WebSocket server doesn't change this fundamental aspect of the Laravel application.

<figure style="text-align: center">
  <img src="img_15.png" alt="App terminates after sending HTTP Response">
  <figcaption style="text-align: center;">Figure 2.1.1.4: App terminates after sending HTTP Response</figcaption>
</figure>

### Laravel Application Communication with WebSocket Server:

   
In a typical scenario, the Laravel application doesn't communicate directly with the WebSocket server, but rather communicates through events. When something happens in a Laravel application that should be broadcast to clients (for example, a new message is added in a chat app), the application dispatches an event. This event is heard by the WebSocket server, which then broadcasts the relevant data to all connected clients. All of this is done while maintaining the short-lived nature of the Laravel application instance.

<figure style="text-align: center">
  <img src="img_14.png" alt="Laravel app communicating with websocket server via events">
  <figcaption style="text-align: center;">Figure 2.1.1.5: Laravel app communicating with websocket server via events</figcaption>
</figure>

### WebSocket Servers and Clients:


In most scenarios, it's the WebSocket server that is actively pushing messages to the clients. The clients typically interact with the server via standard HTTP requests to access and modify resources. For example, in a chat application, a new message is sent to the server via an HTTP request. The server then broadcasts this message to other connected clients using the WebSocket connection.

<figure style="text-align: center">
  <img src="img_13.png" alt="Client communicating both with websocket and HTTP web servers">
  <figcaption style="text-align: center;">Figure 2.1.1.6: Client communicating both with websocket and HTTP web servers</figcaption>
</figure>

<div style="padding-top: 32px;"></div>

In a typical messaging app scenario the following occurs:

   1. When a user sends a message, the client makes an HTTP POST request to the web server. The payload of the request contains the message content and any other necessary information.
   2. The web server routes the request to the Laravel application, which processes the request. This includes storing the message in the database and maybe other business logic.
   3. After storing the message, the Laravel application dispatches an event to signify that a new message has been created.
   4. The WebSocket server, which could be Laravel Websockets or Pusher or something similar, is listening for this event. When it hears the event, it broadcasts a message to all connected clients with the details of the new message.
   5. The clients receive this message over their WebSocket connections. They handle the message by updating the user interface to display the new message.
   6. The original HTTP request from step 1 is responded to, indicating success or failure to the client that sent the message.

<figure style="text-align: center">
  <img src="img_12.png" alt="Propagation of a typical event in a websocket application">
  <figcaption style="text-align: center;">Figure 2.1.1.7: Propagation of a typical event in a websocket application</figcaption>
</figure>

### Clients Sending Messages to WebSocket Server:

There are scenarios where the client might also send messages to the WebSocket server. A notable example is Google Docs. 

As users type and make changes to a document, those changes are sent to the WebSocket server over the WebSocket connection. The WebSocket server then broadcasts these changes to all other clients connected to the same document. Each client application would have the logic to handle these updates and apply them to the document in real-time. This allows all users to see changes as they happen.

There's still server-side application logic involved, but it's primarily for handling more permanent changes like saving the final version of the document, handling authentication, managing users, and similar tasks. These operations would typically be performed via traditional HTTP requests to the server, and are separate from the real-time, collaborative aspect of the application facilitated by the WebSocket connection. 

<figure style="text-align: center">
  <img src="img_11.png" alt="Multiple clients interacting with a websocket server">
  <figcaption style="text-align: center;">Figure 2.1.1.8: Multiple clients interacting with a websocket server</figcaption>
</figure>