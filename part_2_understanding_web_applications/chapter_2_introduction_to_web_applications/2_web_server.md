## 2.2 Web Server

Now let's break down what actually happens inside the server. From now on we are going to omit the term server as it is a broad term, and we are going to use more specific terms like: **physical server**, **web server**, **application server** ...

The part that is actually responsible for listening to the incoming HTTP Requests and generating HTTP Responses is the **Web Server**.

A **Web Server** is a computer software that accepts HTTP requests and is able to serve back HTTP responses. So more specifically a **Web Server** is a **Process** that operates on top of an **Operating System**.
The **OS** is installed on a **Physical Server**.

So a **browser** requesting a **web application** is more accurately represented like the following:

```mermaid
sequenceDiagram
participant Browser
box "OS"
participant WebServer as Web Server Process
end
Browser->>WebServer: HTTP Request
WebServer->>Browser: HTTP Response
```
---

**A Web Server's Primary Function**: is to handle HTTP requests and responses. It serves **static** content like HTML, CSS, Javascript and images to the client's browser.

So an even more accurate representation of a **browser** requesting a **Web Server** is the following:

```mermaid
sequenceDiagram
    participant Browser
    box "OS"
        participant WebServer as Web Server Process
        participant ASC as Application Source Code
    end
    Browser->>WebServer: HTTP Request for welcome.html
    WebServer->>ASC: File System Access
    Note over ASC: Contains multiple files
    Note over ASC: including welcome.html
    ASC-->>WebServer: Files Retrieved
    WebServer->>Browser: HTTP Response (welcome.html)
```

---

{% hint style="info"%}

Web Servers allow you to specify the location of the **Application Source Code** in their configuration. 

Example:

in **Apache HTTP Server** you specify the location using the `DocumentRoot` directive inside the `httpd.conf` file. 

Like the following example: `DocumentRoot /var/www/html`

It's important to note that Web Servers like Apache require appropriate file system permissions to access the files it serves.

{% endhint %}


{% hint style="info"%}


### Web Server Modules


Web Server **modules** are extensions that add functionality to the Web Server. They allow you to customize and extend the capabilities of the server without modifying the core code.

{% endhint %}

{% hint style="tip" %}

Apache HTTP Server has a modular architecture, which means that features are separated into individual modules that can be loaded or unloaded as needed. This makes Apache highly flexible and adaptable to various use cases.

Apache modules can be classified as the following: **Core Modules**, **Shared Modules**, **MPMs (Multi-Processing Modules)**, **Handler Modules**, **Filter Modules**, **Security Modules**, **Rewrite and Alias Modules**, **Proxy Modules**, **Logging and Monitoring Modules**, **Authentication and Authorization Modules**.

To enable or disable modules, you can use the `a2enmod` and `a2dismod` commands (on Debian-based systems) or edit the Apache configuration files directly.

{% endhint %}


### MPM Multi-Processing Module

A web application must be able to handle multiple requests simultaneously. Therefore, Apache HTTP Server version 2.0 introduced the **MPM (Multi-Processing Module)**

Prior to Apache 2.0, the Apache server used a single process model called the `mpm_prefork` module.

#### MPMs

There are different MPMs available in Apache, each implementing different strategies for managing concurrency. Some commonly used MPMs include:


* **Prefork MPM**: This MPM follows a non-threaded approach and creates multiple worker processes, each capable of handling a single connection at a time. It provides excellent stability and isolation between processes but can consume more memory due to the overhead of separate processes.


* **Worker MPM**: The Worker MPM is a hybrid model that combines processes and threads. It creates multiple worker processes, each containing multiple threads. Each thread handles an individual connection, allowing for higher concurrency and reduced memory usage compared to the Prefork MPM.


* **Event MPM**: The Event MPM is similar to the Worker MPM but introduces a more efficient event-driven architecture. It utilizes a small number of threads to handle a large number of connections, using event notifications to efficiently manage concurrency and reduce resource usage.

---

The following is a diagram that represents different implementations of the MPM and how they differ.

```mermaid

graph TD;
  A[Apache HTTP Server];
  MPM[Multi-Processing Modules];
  B[mpm_prefork];
  C[mpm_worker];
  D[mpm_event];
  E[Multiple Child Processes];
  F[Multiple Threads per Child];
  G[Keep-Alive Handling];
  A --> MPM;
  MPM --> B;
  MPM --> C;
  MPM --> D;
  B --> E;
  C --> E;
  C --> F;
  D --> E;
  D --> F;
  D --> G;
  
```


---

{% hint style="info" %}

For Apache 2.2 and earlier versions, the default MPM is **mpm_prefork**.

Starting from Apache 2.4, the default MPM is **mpm_event**.

{% endhint %}

---

### mpm_prefork

The following diagram illustrates the flow of HTTP requests and responses in the Apache HTTP Server. It shows that the "httpd Apache Listener" functionality receives HTTP requests from browser clients, forwards them to the "Request Queue", and then the worker processes handle these requests. Once a worker process generates a response, it forwards it back to the "httpd Apache Listener", which then sends the HTTP response back to the browser client.

This diagram visually represents the interaction between browser clients, the Apache HTTP Server, and its components, including the Master Process, Multi-Processing Module (MPM), Request Queue, and Worker Processes. The red links indicate the path of HTTP requests and responses.

```mermaid

graph RL;
  Browser1(Browser Client 1) <--> |HTTP Request/Reponse|HttpdModule;
  Browser2(Browser Client 2) --> |HTTP Request|HttpdModule;
  subgraph OS[OS]
    style OS fill:#ffffff00,stroke:#333;
    subgraph Apache["Apache HTTP Server (Master Process)"]
        HttpdModule["httpd Apache Listener<br>(functionality)"] -->|Forwards Request| Queue;
        MP((("Apache HTTP Server (Master Process)"))) -->|use|MPM;
        MP --> |monitor|Pool;
        MP --> |start|HttpdModule;
        MPM --> |to create|Pool;
        style Apache fill:#f9f9f9,stroke:#333;
        Queue[[Request Queue]];
        MPM{{"`Multi-Processing Module (**mpm_prefork**)`"}}
        subgraph Pool[Pool]
            direction TB;
            style Pool fill:#f9f9f9,stroke:#333;
            Worker1("Worker 1 (Process)");
            Worker2("Worker 2 (Process)");
            Worker3("Worker 3 (Process)");
        end
        Pool -. "watches" .-> Queue;
    end
    Worker1 -->|Access| FileSystem[File System];
    Worker1 -->|Forwards Generated Response|HttpdModule;
    Worker2 -->|Access| FileSystem;
    Worker3 -->|Access| FileSystem;
    FileSystem -->|Retrieve| WelcomePage[public/welcome.html];
    subgraph SourceCode[Application Source Code]
        style SourceCode fill:#f9f9f9,stroke:#333;
        WelcomePage[public/welcome.html];
    end
  end
  style HttpdModule fill:#85C1E9,stroke:#333;
  style Queue fill:#F7DC6F,stroke:#333;
  style Worker1 fill:#82E0CA,stroke:#333;
  style Worker2 fill:#82E0CA,stroke:#333;
  style Worker3 fill:#82E0CA,stroke:#333;
  style FileSystem fill:#E59866,stroke:#333;
  style SourceCode fill:#E59866,stroke:#333;
  style MP fill:#82E0AA,stroke:#333;
  linkStyle 0,2,7,9 stroke: red;


```

### mpm_worker

```mermaid

graph RL;
  Browser1(Browser Client 1) <--> |HTTP Request/Reponse|HttpdModule;
  Browser2(Browser Client 2) --> |HTTP Request|HttpdModule;
  subgraph OS[OS]
    style OS fill:#ffffff00,stroke:#333;
    subgraph Apache["Apache HTTP Server (Master Process)"]
        HttpdModule["httpd Apache Listener<br>(functionality)"] -->|Forwards Request| Queue;
        MP((("Apache HTTP Server (Master Process)"))) -->|use|MPM;
        MP --> |monitor|Pool;
        MP --> |start|HttpdModule;
        MPM --> |to create|Pool;
        style Apache fill:#f9f9f9,stroke:#333;
        Queue[[Request Queue]];
        MPM{{"`Multi-Processing Module (**mpm_worker**)`"}}
        subgraph Pool[Pool]
            direction TB;
            style Pool fill:#f9f9f9,stroke:#333;
            subgraph Worker1["Worker 1 (Process)"]
                Thread1_1["Thread 1"]
                Thread1_2["Thread 2"]
            end
            subgraph Worker2["Worker 2 (Process)"]
                Thread2_1["Thread 1"]
                Thread2_2["Thread 2"]
            end
            subgraph Worker3["Worker 3 (Process)"]
                Thread3_1["Thread 1"]
                Thread3_2["Thread 2"]
            end
        end
        Pool -. "watches" .-> Queue;
    end
    Thread1_1 -->|Access| FileSystem[File System];
    Thread1_1 -->|Forwards Generated Response|HttpdModule;
    Thread2_1 -->|Access| FileSystem;
    Thread3_1 -->|Access| FileSystem;
    FileSystem -->|Retrieve| WelcomePage[public/welcome.html];
    subgraph SourceCode[Application Source Code]
        style SourceCode fill:#f9f9f9,stroke:#333;
        WelcomePage[public/welcome.html];
    end
  end
  style HttpdModule fill:#85C1E9,stroke:#333;
  style Queue fill:#F7DC6F,stroke:#333;
  style Worker1 fill:#82E0CA,stroke:#333;
  style Worker2 fill:#82E0CA,stroke:#333;
  style Worker3 fill:#82E0CA,stroke:#333;
  style FileSystem fill:#E59866,stroke:#333;
  style SourceCode fill:#E59866,stroke:#333;
  style MP fill:#82E0AA,stroke:#333;
  linkStyle 0,2,7,9 stroke: red;

```

The `mpm_worker` module uses multiple worker processes, each of which can handle many threads, with each thread handling one connection at a time. This model allows the server to handle multiple requests concurrently with fewer resources than a process-based model.

### mpm_event

```mermaid

graph RL;
  Browser1(Browser Client 1) <--> |HTTP Request/Reponse|HttpdModule;
  Browser2(Browser Client 2) --> |HTTP Request|HttpdModule;
  subgraph OS[OS]
    style OS fill:#ffffff00,stroke:#333;
    subgraph Apache["Apache HTTP Server (Master Process)"]
        HttpdModule["httpd Apache Listener<br>(functionality)"] -->|Forwards Request| Queue;
        MP((("Apache HTTP Server (Master Process)"))) -->|use|MPM;
        MP --> |monitor|Pool;
        MP --> |start|HttpdModule;
        MPM --> |to create|Pool;
        style Apache fill:#f9f9f9,stroke:#333;
        Queue[[Request Queue]];
        MPM{{"`Multi-Processing Module (**mpm_event**)`"}}
        subgraph Pool[Pool]
            direction TB;
            style Pool fill:#f9f9f9,stroke:#333;
            subgraph Worker1["Worker 1 (Process)"]
                Thread1_1["Worker Thread 1"] --> |Handoff| Thread1_2
                Thread1_2["Supporting Thread 1"]
            end
            subgraph Worker2["Worker 2 (Process)"]
                Thread2_1["Worker Thread 1"] --> |Handoff| Thread2_2
                Thread2_2["Supporting Thread 1"]
            end
            subgraph Worker3["Worker 3 (Process)"]
                Thread3_1["Worker Thread 1"] --> |Handoff| Thread3_2
                Thread3_2["Supporting Thread 1"]
            end
        end
        Pool -. "watches" .-> Queue;
    end
    Thread1_1 -->|Access| FileSystem[File System];
    Thread1_1 -->|Forwards Generated Response|HttpdModule;
    Thread2_1 -->|Access| FileSystem;
    Thread3_1 -->|Access| FileSystem;
    FileSystem -->|Retrieve| WelcomePage[public/welcome.html];
    subgraph SourceCode[Application Source Code]
        style SourceCode fill:#f9f9f9,stroke:#333;
        WelcomePage[public/welcome.html];
    end
  end
  style HttpdModule fill:#85C1E9,stroke:#333;
  style Queue fill:#F7DC6F,stroke:#333;
  style Worker1 fill:#82E0CA,stroke:#333;
  style Worker2 fill:#82E0CA,stroke:#333;
  style Worker3 fill:#82E0CA,stroke:#333;
  style FileSystem fill:#E59866,stroke:#333;
  style SourceCode fill:#E59866,stroke:#333;
  style MP fill:#82E0AA,stroke:#333;
  linkStyle 0,2,7,10,12 stroke: red;

```

In the context of the `mpm_event` module in Apache HTTP Server, "supporting threads" refers to threads that are used to handle certain parts of the request/response process, freeing up the main threads to handle other requests.


In a typical web server setup, a "normal" thread (or worker thread) would handle the entire lifecycle of a client request, from accepting the connection, processing the request, sending the response, and finally closing the connection. This means that while the thread is waiting for network I/O (such as waiting for the full request to arrive or for the response to be fully sent), it can't do anything else.

The `mpm_event` module improves upon this by using a different approach. When a client connection is first accepted, it's handled by a worker thread just like in `mpm_worker`. However, once the initial request has been processed and the response is being sent (or if the connection is simply being kept open for possible future requests), the connection is handed off to a separate "supporting" thread. This allows the worker thread to move on and handle other incoming requests.

The supporting threads are part of a separate thread pool and are responsible for handling these "keep-alive" connections, waiting for new requests to come in on these connections, and if a new request does come in, the connection is passed back to a worker thread for processing.

This approach allows `mpm_event` to handle a larger number of concurrent connections, as the worker threads are freed up to handle new requests instead of being tied up with long-lived connections. It's particularly beneficial for workloads where many connections are in the keep-alive state, waiting for new requests.

---

<section style="background-color: aliceblue; padding: 20px; border-radius: 10px">

<h3 style="color: grey; font-weight: 700;">Configuration</h3>

</section>

The number of worker processes and threads created by Apache HTTP Server can be configured in the Apache configuration file, typically named `httpd.conf` or `apache2.conf`, depending on your system.

The specific directives you need to modify depend on the Multi-Processing Module (MPM) you are using.

For the `mpm_worker` and `mpm_event` modules, you can use the following directives:

- `ServerLimit`: This directive sets the maximum configured value for `MaxRequestWorkers` for the lifetime of the Apache process.

- `MaxRequestWorkers`: This directive sets the limit on the total number of simultaneous requests to be served. For `mpm_worker` and `mpm_event`, this is the total number of worker threads (i.e., the product of `ServerLimit` and `ThreadsPerChild`).

- `ThreadsPerChild`: This directive sets the number of threads created by each worker process.


Example of an Apache configuration file:


```apache
<IfModule mpm_worker_module>
    ServerLimit          16
    StartServers         4
    MinSpareThreads      25
    MaxSpareThreads      75 
    ThreadsPerChild      25
    MaxRequestWorkers    400
    MaxConnectionsPerChild 10000
</IfModule>
```

In this example: 
  * Apache would start with 4 worker processes (due to `StartServers`), 
  * each with 25 threads (`ThreadsPerChild`). 
  * It would allow up to 16 worker processes (`ServerLimit`).
  * and a maximum of 400 threads (`MaxRequestWorkers`)
  * which means that if needed, Apache could scale up to 16 processes each with 25 threads to handle heavy load.

<section style="background-color: aliceblue; padding: 20px; border-radius: 10px">

These configuration should be adjusted according to the server's hardware capabilities and the nature of your workload. 

</section>

{% hint style="danger"%}

#### Terminating workers (process/threads)

worker processes/threads will be terminated in case if they crash. But they will also be terminated in the following scenarios: 

* Idle Timeout: For the `mpm_worker` and `mpm_event modules`, the `MaxSpareThreads` directive sets an upper limit on the number of idle worker threads. If there are more idle threads than this limit, some of the idle threads will be terminated.

* System Conditions: Worker processes or threads may also be terminated due to system conditions, such as out-of-memory situations, or system shutdown or reboot.

{% endhint %}

{% hint style="info" %}

#### creating workers (process/threads)

**MaxConnectionsPerChild** Limit Reached: For the `mpm_prefork`, `mpm_worker`, and `mpm_event` modules, the `MaxConnectionsPerChild` configuration directive sets a limit on the number of requests that a worker process should handle before it is terminated and a new process is created. This can be useful for mitigating memory leaks in third-party modules or in the Apache server itself. When a worker process reaches this limit, it will not accept new connections and will terminate after finishing its current request.

{% endhint %}

{% hint style="info" %}

The maximum number of requests that a worker process or thread can handle in Apache HTTP Server is determined by several factors and configurations:


* **Web Server Configuration:**
  * MaxConnectionsPerChild
  * ThreadsPerChild
  * MaxRequestWorkers
  * KeepAlive and MaxKeepAliveRequests
  * LimitRequestLine and LimitRequestFieldSize


* **OS Configuration:**
  * File Descriptors: limit on the total number of file descriptors
  * TCP/IP Stack Settings
  * Network Interface Settings
  * Process Limits
  * Memory Management
  * Security Limits


* **Hardware Resources:** 
  * CPU
  * Memory(RAM)
  * Disk I/O: SSD - HDD
  * Network Bandwidth
  * Hardware Concurrency: number of CPUs and number of CPU cores.


{% endhint %}







### Dynamic Content

More than often we need our web application to provide content based on the specific user's provided data.

It is impossible to store all possible combinations of all the possible users that may interact with our application as static content since this would result in an infinite amount of static files.

To solve this problem we need to **generate** the file (for example a html page) based on data that the user provided. If this data changes the generated content will change accordingly and hence the name **dynamic content** as opposed to **static content** that never changes.

To perform the **generation** of a **dynamic content** a **script** needs to be executed. This **script** specifies how this content is going to be generated.

As said before the **primary function** of a **Web Server** is to serve **static** assets. But **Web Servers**' functionality can be extended through the use of **modules**.


{% hint style="info" %}

**Handler Modules**: These modules handle specific types of content. For example, **mod_php** handles **PHP scripts**, `mod_cgi` handles CGI scripts, and `mod_dav` handles `WebDAV` publishing.

{% endhint %}

{% hint style="info"%}

**mod_php**: is a module for the Apache HTTP server that enables the server to process PHP scripts. It is one of the ways to run PHP scripts on a web server.

{% endhint %}


{% hint style="info"%}

Other than **mod_php** there is also:

**mod_python**: is an Apache HTTP Server module that allows the execution of Python code within the Apache web server process. The **mod_python** is deprecated and now replaced with **mod_wsgi**

**mod_jserv**: also known as Apache JServ Protocol (AJP), is an Apache module designed to enable communication between the Apache HTTP Server and Java applications running in a separate Java Virtual Machine (JVM). However, **mod_jserv** is an older technology and has been largely replaced by the Apache Tomcat Connector (mod_jk) and the Apache Tomcat Native Connector (mod_proxy_ajp) for integrating Apache with Java applications

{% endhint %}

----

The following diagram shows how a Web Server uses a **script execution module** to generate a **dynamic** profile web page.

```mermaid

sequenceDiagram
    participant Browser
    box "Web Server Process"
        participant WebServer as Web Server Core Module and Enabled Modules
        participant SEM as Script Execution Module
    end
    participant ASC as Application Source Code
    participant Database as Database Server Process
    Browser->>WebServer: HTTP request for profile.html
    WebServer->>ASC: Locate profile.html
    Note over ASC: Contains multiple files
    Note over ASC: including profile.html
    WebServer->>SEM: Pass request if a script is inside profile.html
    SEM->>Database: Query data (if needed)
    SEM->>WebServer: Return generated HTML
    WebServer->>Browser: HTTP response with HTML content
    Browser->>Browser: Render the page
    
```

---

The following diagram describes the same interaction with Apache HTTP Server as a Web Server, PHP as the script language and mod_php as the Script Execution Module.

```mermaid
sequenceDiagram
participant Browser
box "Apache HTTP Server Process"
participant Apache as Apache Core Module and Enabled Modules
participant mod_php
end
participant ASC as Application Source Code
participant Database as Database Server Process
Browser->>Apache: HTTP request for profile.html
Apache->>ASC: Locate profile.html
Note over ASC: Contains multiple files
Note over ASC: including profile.html
Apache->>mod_php: Pass request if a PHP script is inside profile.html
mod_php->>Database: Query data (if needed)
mod_php->>Apache: Return generated HTML
Apache->>Browser: HTTP response with HTML content
Browser->>Browser: Render the page
```

---


### Aftermath


The approach of using a **Script Execution Module** has its **Pros** and **Cons**. Let us analyse them in the case of **Apache HTTP Server** and **mod_php**.

#### Pros:

* **Ease Of Integration**: mod_php integrates tightly with Apache. Once enabled, PHP scripts can be executed without additional setup, making it convenient for developers.


* **Performance**: mod_php is highly optimized for performance because it runs PHP code directly within the Apache server process. This eliminates the need for launching separate PHP processes for each request, resulting in faster execution compared to CGI or FastCGI approaches.

#### Cons:

* **Limited scalability**: mod_php is not ideal for high-traffic websites or applications with a heavy load. As each Apache worker process handles both serving static files and executing PHP scripts, scaling PHP applications may require scaling the entire Apache server, which can be resource-intensive.


* **Lack of process isolation**: Since mod_php runs PHP code within the Apache process, there is no strict process isolation. If a PHP script crashes or consumes excessive resources, it can affect the stability and performance of the entire server.


* **Security risks**: Running PHP within the Apache process raises potential security concerns. If a PHP script has a vulnerability, it could potentially affect the entire server and compromise its security. Careful consideration and security measures are necessary to mitigate these risks.
