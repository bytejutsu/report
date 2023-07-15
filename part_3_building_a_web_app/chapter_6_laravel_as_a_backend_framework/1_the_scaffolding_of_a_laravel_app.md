## 6.1 The Scaffolding of a Laravel App

### Let's start from the Web Server

The main role of a Web Server is to serve static files to which it has access.

Examples of static files include HTML, CSS, JavaScript, and image files.

In the scenario of a user interacting with a Web App. The user will be requesting HTML pages from the browser. The Web Server will try to serve the requested HTML page.

Since the browser only understands **HTML**, **CSS** and **Javascript** then each HTML page will have its own:
 - CSS embedded in the HTML or in a separate section inside that same HTML file in a **style** tag 
 - Javascript defined inside a **script** tag

In this case the Web Server will only serve one file per request. The browser will be able to understand the entire served file, and it will not send additional requests.

However, consider the following scenarios:
- when using javascript inside a **script** tag in an HTML page. There is a need to use predefined javascript libraries.
- when using css styling either **embedded** or in a **style** tag. There is a need to use predefined css.

For this reason an HTML page has the ability to link external js or css resources using the **link** tag. Those resources can be local or remote/in the cloud.

When a user requests an HTML page that references external files using the **link** tag:

1. the browser will notice the referenced files
2. the browser will first check for each referenced file:
   - if it has it already cached internally, 
     - then it will directly use it to build the requested HTML page.
   - if the referenced file is not cached in the browser: 
     - 1. the browser will locate the referenced file:
        - if the referenced file is local:
          - The browser sends an additional request to the same web server asking for the linked resources
        - if the referenced file is remote:
          - The browser sends an additional request to the specified remote location web server   
     - 2. the browser will then cache the new fetched file for later usage.


Technically each HTML page can use its specific resources css and js resources. And the browser will try to get those resources if the HTML page is requested and the browser doesn't have them.

However, because web apps are mostly structured in a way that takes advantage of reusing HTML components like the **navigation section** and the **footer**. Most web apps use a **main layout** HTML page from which the rest of HTML pages inherit the parent's layout (including the **header tags**).

Because the HTML pages that inherit from the main layout will automatically also reference the css and javascript file that the main layout uses, this allows the entire web app to only rely on one shared 

{% hint style="danger" %}

HTML doesn't support including external HTML files. For that you need to use javascript or PHP to be able to include the HTML programmatically.

{% endhint %}

//todo one js, one css, the rest is fetched from the cloud.

//todo easier caching for the browser

{% hint style="tip" %}

asset bundler like vite, webpack, gulp ...

laravel uses vite by default

{% endhint %}