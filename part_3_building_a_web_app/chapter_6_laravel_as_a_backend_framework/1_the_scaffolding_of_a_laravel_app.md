## 6.1 The Scaffolding of a Laravel App

### Let's start from the Web Server

The main role of a Web Server is to serve static files to which it has access.

{% hint type = "info" %}

To tell a web server which files it should serve, you need to configure it. The configuration process varies depending on the web server software you're using, but it generally involves the following steps:

1. **Specify the Document Root**: The document root is the directory where the web server looks for files to serve on your website. For example, if you're using Apache, you can set the document root in your `httpd.conf` file with the `DocumentRoot` directive.

2. **Set Directory Index**: The directory index is a file that the web server will serve if a client requests a directory. By default, this is usually a file like `index.html` or `index.php`. You can set the directory index in your web server's configuration file.

//todo talk about the public directory!!!!!!!!!!!!!

{% endhint %}

//todo: show example of public directory with different html files

Examples of static files include HTML, CSS, JavaScript, and image files.

In the scenario of a user interacting with a Web App. The user will be requesting HTML pages from the browser. The Web Server will try to serve the requested HTML page.

Since the browser only understands **HTML**, **CSS** and **Javascript** then each HTML page will have its own:
 - CSS embedded in the HTML or in a separate section inside that same HTML file in a **style** tag 
 - Javascript defined inside a **script** tag

Since the browser can interpret mainly **HTML**, **CSS**, **JavaScript**, each HTML page will have its own:
- CSS embedded in the HTML or in a separate section inside that same HTML file in a **style** tag
- Javascript defined inside a **script** tag

//todo: show example of html file with js/css tags

{% hint type="info" %}

modern browsers can also handle other types of content like:
- **XML**, **JSON**, **SVG**, **WebP**, 
- and various **media formats** (audio, video). 

They also support 
- **WebAssembly**, a binary instruction format for a stack-based virtual machine.

{% endhint %}

In this case the Web Server will only serve one file per request. The browser will be able to understand the entire served file, and it will not send additional requests.

However, consider the following scenarios:
- when using javascript inside a **script** tag in an HTML page. There is a need to use predefined javascript libraries.
- when using css styling either **embedded** or in a **style** tag. There is a need to use predefined css.

For this reason, an HTML page can link external JS or CSS resources. CSS resources are linked using the **link** tag, and JS resources are linked using the **script** tag. These resources can be local or remote/in the cloud.

#### Browser and linked resources

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

//todo: show diagram of this workflow

{% hint style = "tip" %} 

The caching behavior can be controlled by various HTTP headers, such as `Cache-Control`, `ETag`, and `Last-Modified`. 

{% endhint%}
            
#### including HTML files

Technically each HTML page can use its specific resources css and js resources. And the browser will try to get those resources if the HTML page is requested and the browser doesn't have them.

However, because web apps are mostly structured in a way that takes advantage of reusing HTML components like the **navigation section** and the **footer**. Most web apps use a **main layout** HTML page from which the rest of HTML pages inherit the parent's layout (including the **header tags**).

//todo: show diagram of how a html web app is structured using layouts and anchor links

{% hint style="danger" %}

HTML doesn't support including external HTML files. For that you need to use javascript or PHP to be able to include the HTML programmatically.

Many modern front-end js frameworks and libraries (like React, Angular, Vue.js) build upon the ability to include HTML file using javascript to support component-based architecture.

//todo: talk about the equivalent in php such as blade 

{% endhint %}

//todo: talk how modern js frameworks minify the html skeleton to include just one root elements and then inject the html components as javascript (jsx)

//todo: talk about how the same is done using blade php 


#### Asset Bundling

The HTML pages that inherit from the main layout will automatically also reference/share the css and javascript file(s) that the main layout uses. This allows:

- all the installed js libraries/packages to be bundled into one js file usually called **app.js**
- all the css styling used across the app to be bundled into one css file usually called **app.css** 

{% hint style="tip" %}

A tools that helps bundle the app's:
- javascript packages into one or multiple js files 
- and the app's css into one or multiple css files 

is called an **asset bundler**.

An **asset bundler** is a tool that helps bundle the app's JavaScript packages into one or multiple JS files and the app's CSS into one or multiple CSS files. This process not only combines files but also minifies and optimizes them, improving load time and performance.

Examples of assset bundler are: **vite**, **webpack** and **gulp**.

Laravel actually uses **vite** by default.

{% endhint %}

{% hint type="tip" %}

**Reducing the number of HTTP Requests with bundling**

Each request for a resource (HTML, CSS, JS, images) is an HTTP request, and reducing these requests is one of the reasons bundling and minification are used.

Note that when the number of HTTP requests for resources are reduced, this is beneficial for both the client and server. Those benefits are barely noticeable on the client-side. However, this reduction takes more effect when it is considered in scale on the server-side.

Because, if you have thousands simultaneous users then using bundling will spare the server extra thousands simultaneous requests for resources.

{% endhint %}

{% hint type="danger" %}

Please note that an asset bundler will only bundle JavaScript or CSS libraries that are installed locally in your app's development project. It will not bundle remotely referenced JS or CSS libraries. 

Those remote JS/CSS libraries will still need to be fetched by the browser from their specified remote location(s), which is usually a **CDN** (Content Delivery Network).

{% endhint %}

### PHP for dynamically generated HTML

//todo: talk about how php is necessary and took over to generate HTML, and thus we have index.php entry point

//todo: talk about how the routing is no more resouce based but virtual

//todo: talk about the entry points / vendor / packages / build .. of a classic php app