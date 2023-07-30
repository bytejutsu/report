## 6.2 Websocket Client

In a Laravel application providing real-time updates (like live sports scores), you would typically use a mix of HTTP and WebSocket connections.

Here's a general breakdown:


1. When the user first visits your website, their browser makes an HTTP request to your Laravel application. The Laravel application processes this request, generates an HTTP response (containing HTML, CSS, JavaScript, etc.), and sends it back to the client. This is the normal request-response cycle of a web application, and once the response is sent, the Laravel application's work for that request is done.


2. The HTML and JavaScript sent to the client includes code to establish a WebSocket connection back to your server. This connection is made to the WebSocket server, which could be a separate service like Pusher, or your own WebSocket server provided by the Laravel Websockets package. Once this connection is established, it remains open, allowing for real-time communication between the server and the client.


3. When an update happens (like a change in the sports score), the Laravel application broadcasts an event. If you're using the Laravel Websockets package, this might be done via the `broadcast` function in Laravel, which sends the event to the WebSocket server.


4. The WebSocket server then pushes this update to all connected clients in real time.


5. The JavaScript running in the user's browser receives this update from the WebSocket server, and updates the page accordingly.


During this process, the Laravel application does not stay running in between HTTP requests. It simply handles each request (whether that's an initial page load, or a task to update a sports score), sends a response (or broadcasts an event), and then its work for that request is done.

The WebSocket server is responsible for maintaining the long-lived connections with each client, and for pushing updates to those clients as they happen. But the Laravel application itself still works on a per-request basis, processing each request and then terminating. It doesn't stay running in between requests, even though the client maintains a long-lived connection to the WebSocket server.

```mermaid
sequenceDiagram
  participant C as Client
  participant WS as WebSocket Server
  participant L as Laravel Application
  C->>L: HTTP request
  L-->>C: HTTP response (HTML, CSS, JS)
  Note over C: JS received knows how to reach the Websocket Server
  C->>WS: Establish WebSocket connection
  L->>WS: Broadcast event on update
  WS-->>C: Push update to client
  Note over C: JS received knows how to listen to the Websocket Server
  C->>C: Update page
```

### 

#### Connect to the Websocket Server

for that we use **Pusher.js**

#### Listen to the Websocket Server 

for that we use Laravel **Echo.js**