## 6.3 Real-Time Communication Techniques

The following are common techniques used to facilitate real-time communication between a client (such as a web browser) and a server. 

### 1. Polling
- **How it Works**: The client repeatedly sends HTTP requests to the server at regular intervals to check for new data.
- **Pros**: Simple to implement.
- **Cons**: Inefficient, as it can lead to unnecessary requests when there's no new data, and latency in receiving updates if the polling interval is too long.

```mermaid
sequenceDiagram
  participant Client
  participant Server
  Client->>Server: HTTP request (check for updates)
  Server-->>Client: HTTP response (no updates)
  Note over Client: Wait for polling interval
  Client->>Server: HTTP request (check for updates)
  Server-->>Client: HTTP response (updates available)
```

### 2. Server-Sent Events (SSE)
- **How it Works**: The client opens a single HTTP connection to the server, and the server sends updates to the client over that connection as they become available.
- **Pros**: More efficient than polling, as updates are sent as soon as they're available.
- **Cons**: Unidirectional (server to client only), and not supported by all browsers.

```mermaid
sequenceDiagram
  participant Client
  participant Server
  Client->>Server: HTTP request (open connection for updates)
  Server-->>Client: HTTP response (connection established)
  Note over Server: When updates are available
  Server-->>Client: Send updates
```

### 3. WebSockets
- **How it Works**: The client and server establish a full-duplex, persistent connection over which they can send messages to each other.
- **Pros**: Bidirectional, allowing both the client and server to send messages at any time; more efficient than polling or SSE.
- **Cons**: More complex to implement; requires a specific protocol (though many libraries are available to simplify this).

```mermaid
sequenceDiagram
  participant Client
  participant Server
  Client->>Server: HTTP request (open WebSocket connection)
  Server-->>Client: HTTP response (WebSocket connection established)
  Note over Client,Server: Bidirectional communication
  Client->>Server: Send message
  Server-->>Client: Send message
```

### 4. Push Notifications
- **How it Works**: The server sends notifications to the client via a third-party service (such as Apple's Push Notification Service for iOS or Google's Firebase Cloud Messaging for Android).
- **Pros**: Allows the server to send notifications to the client even when the client's app is not running or the web page is not open.
- **Cons**: Requires integration with platform-specific services; may have privacy implications.

```mermaid
sequenceDiagram
  participant Client
  participant Server
  participant PushService as Push Notification Service
  Client->>Server: HTTP request (subscribe for push notifications)
  Server->>PushService: Register Client for push notifications
  Note over Server: When updates are available
  Server->>PushService: Send push notification
  PushService-->>Client: Deliver push notification
```