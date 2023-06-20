## 2.2 Request-Response Cycle


The following is a sequence diagram that visually represents the interactions between the user, browser, web server, and application server during the request-response cycle of a web application.

```mermaid
sequenceDiagram
    participant User as User
    participant Browser as Browser
    participant WebServer as Web Server
    participant AppServer as Application Server
    User->>Browser: Interact with web app
    Browser->>WebServer: Send HTTP request
    WebServer->>AppServer: Forward request to app server
    AppServer->>WebServer: Generate HTTP response
    WebServer->>Browser: Send HTTP response
    Browser->>User: Render content on screen
    User->>Browser: Interact with loaded page
```

---

