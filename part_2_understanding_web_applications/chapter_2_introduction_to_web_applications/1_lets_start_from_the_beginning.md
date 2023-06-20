## 2.1: Let's Start From the Beginning

Let's pretend we are in the year 1996, and we are visiting a website.
We hit the website's URL, and we get a static web page.

```mermaid
sequenceDiagram
    participant Browser
    participant Server

    Browser->>Server: HTTP Request for welcome.html
    Server->>Browser: HTTP Response (welcome.html)
    Browser->>Server: HTTP Request for app.css
    Server->>Browser: HTTP Response (app.css)
    Browser->>Server: HTTP Request for app.js
    Server->>Browser: HTTP Response (app.js)
```
---
Web applications work through a series of interactions between the client's **browser** and the application's **server**. Below is a step-by-step description of how a web application works:

We can abstract a single user interaction as a single **request-response cycle**


### Step 1: User Interaction
The user interacts with the web application through a web browser. This could be by entering a URL, clicking on a link, or submitting a form.

### Step 2: Sending a Request
The browser sends an HTTP request to the web server. 


{% hint style="info" %}
#### 3 common ways of send requests are supported in Laravel

//todo: move this hint to the laravel setion

1. query parameters
2. route parameters (maybe those two are sent in the HTTP header! I have to verify!)
3. forms -> in the HTTP PACKET (not header)

{% endhint %}


### Step 4: Generating a Response
The server generates an HTTP response. This response typically contains the status code, content type, and the actual content (HTML, JSON, etc.).

### Step 5: Sending the Response
The web server sends the HTTP response back to the client's browser.

### Step 6: Rendering the Content
The browser receives the response and renders the content on the screen. If the response contains references to additional resources like images, CSS, or JavaScript files, the browser may send additional requests to fetch them.

### Step 7: User Interaction with the Loaded Page
The user can now interact with the loaded page. Any further interactions that require server processing will generally follow the same steps.

This process is sometimes referred to as the request-response cycle. It's the fundamental mechanism that underlies the functioning of web applications.

---
