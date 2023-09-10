## 2.2.1: The Scaffolding of a Laravel App

### index.php

The following is the code of the Laravel 10.x **index.php** file.

```PHP

<?php

use Illuminate\Contracts\Http\Kernel;
use Illuminate\Http\Request;

define('LARAVEL_START', microtime(true));

/*
|--------------------------------------------------------------------------
| Check If The Application Is Under Maintenance
|--------------------------------------------------------------------------
|
| If the application is in maintenance / demo mode via the "down" command
| we will load this file so that any pre-rendered content can be shown
| instead of starting the framework, which could cause an exception.
|
*/

if (file_exists($maintenance = __DIR__.'/../storage/framework/maintenance.php')) {
    require $maintenance;
}

/*
|--------------------------------------------------------------------------
| Register The Auto Loader
|--------------------------------------------------------------------------
|
| Composer provides a convenient, automatically generated class loader for
| this application. We just need to utilize it! We'll simply require it
| into the script here so we don't need to manually load our classes.
|
*/

require __DIR__.'/../vendor/autoload.php';

/*
|--------------------------------------------------------------------------
| Run The Application
|--------------------------------------------------------------------------
|
| Once we have the application, we can handle the incoming request using
| the application's HTTP kernel. Then, we will send the response back
| to this client's browser, allowing them to enjoy our application.
|
*/

$app = require_once __DIR__.'/../bootstrap/app.php';

$kernel = $app->make(Kernel::class);

$response = $kernel->handle(
    $request = Request::capture()
)->send();

$kernel->terminate($request, $response);

```

### Analysing the index.php code:

Even if the comments weren't there, the Laravel's index.php file code is straightforward well written.

The banner comments make it even more understandable.

{% hint style = "info" %}

First observation: the length of each comment line in each comment block is **3 characters less** than its preceding comment line.

{% endhint %}

The following is a sequence diagram that describes the interaction between the client, the web server, and Laravel's `index.php`:

| [![](https://mermaid.ink/img/pako:eNp9Ul1rwjAU_SuXPLv9gD4IRYUJCtJ2G4zCuGtubVh7kyWpbIj_fanVslm1T7035yOcnL0otCQRCUdfLXFBc4Vbi03OED6D1qtCGWQPs1oR-_H-lT5SsjuygK4boJ_GQMWSvh9NZfqjXu9hOh0EInjKsg0k3U3cyWk4DMBBIIINOkfuCh4Lr3bo6dJtGP_rzKlUTLCKk_hlsXpPszjJ7hJmFRWfsCwhqwhiY2pVoFeaYengOcAsrFGxJ8aQ5V2lhLbK-UA4KrVew0qjPAd3i9TypfMYPw7UGc3u_nUyso3iLrmr-pJuJfv3hfo3vbAVE9EEcVQy1GzfkXLhK2ooF1H4lVRiW_tc5HwIUAxJpD9ciMjbliaiNTKYnlopohJrN2wXUnlthyUdx3Xf52OtJyI0703rM_HwC1_lARs?type=png)](https://mermaid.live/edit#pako:eNp9Ul1rwjAU_SuXPLv9gD4IRYUJCtJ2G4zCuGtubVh7kyWpbIj_fanVslm1T7035yOcnL0otCQRCUdfLXFBc4Vbi03OED6D1qtCGWQPs1oR-_H-lT5SsjuygK4boJ_GQMWSvh9NZfqjXu9hOh0EInjKsg0k3U3cyWk4DMBBIIINOkfuCh4Lr3bo6dJtGP_rzKlUTLCKk_hlsXpPszjJ7hJmFRWfsCwhqwhiY2pVoFeaYengOcAsrFGxJ8aQ5V2lhLbK-UA4KrVew0qjPAd3i9TypfMYPw7UGc3u_nUyso3iLrmr-pJuJfv3hfo3vbAVE9EEcVQy1GzfkXLhK2ooF1H4lVRiW_tc5HwIUAxJpD9ciMjbliaiNTKYnlopohJrN2wXUnlthyUdx3Xf52OtJyI0703rM_HwC1_lARs) |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Figure 2.2.1.1: Client + Server + index.php                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

In this diagram:

1. The client sends an HTTP request to the web server.
2. The web server passes the HTTP request to `index.php`.
3. `index.php` performs the following steps:
    - Define LARAVEL_START
    - Check If The Application Is Under Maintenance
    - Register The Auto Loader
    - Run The Application
    - Sends an HTTP response back to the web server
    - Terminate The Application
4. The web server sends the HTTP response back to the client.

Let's break down the steps that `index.php` performs and see which section of the `index.php` code is responsible for which step:

1 Define LARAVEL_START

```PHP
define('LARAVEL_START', microtime(true));
```

2 Check If The Application Is Under Maintenance

```PHP
if (file_exists($maintenance = __DIR__.'/../storage/framework/maintenance.php')) {
    require $maintenance;
}
```
 
3 Register The Auto Loader

```PHP
require __DIR__.'/../vendor/autoload.php';
```
 
4 Run The Application

```PHP
$app = require_once __DIR__.'/../bootstrap/app.php';

$kernel = $app->make(Kernel::class);
```
 
5 Sends an HTTP response back to the web server

```PHP
$response = $kernel->handle(
    $request = Request::capture()
)->send();
```
 
6 Terminate The Application

```PHP
$kernel->terminate($request, $response);
```

### Explaining the index.php code sections

1. **Define LARAVEL_START**: This line sets a constant that holds the timestamp of when the script started. This can be used for profiling and debugging.
2. **Check If The Application Is Under Maintenance**: Laravel provides a maintenance mode that can be activated with the `php artisan down` command. When in maintenance mode, Laravel will display a custom view to all requests into your application. This can be used when you are updating your server or installing a new feature and you don't want users to see errors or experience downtime.
3. **Register The Auto Loader**: This part of the script loads Composer's autoloader. Composer is a tool for dependency management in PHP, and its autoloader automatically loads PHP classes when they're needed, so you don't have to manually include them.
4. **Run The Application**: This section does the actual work of handling the incoming HTTP request and sending a response.


- It starts by **bootstrapping the Laravel application**, which involves setting up error handling, configuring logging, loading configuration files, and more. 

```PHP
$app = require_once __DIR__.'/../bootstrap/app.php';
```

- Then it creates an instance of Laravel's **HTTP kernel**, which is responsible for handling the request. The kernel handles the request and returns a response, which is then sent back to the client.

```PHP
$kernel = $app->make(Kernel::class);
```

5 **Terminate The Application**: After the response has been sent to the client, the script calls the kernel's `terminate` method. This method is used to perform any final tasks after the response has been sent, such as committing database transactions or writing to log files.


### The HTTP Kernel

Ok, now let's try to reason backwards and try to understand the following lines of code:

```PHP
$response = $kernel->handle(
    $request = Request::capture()
)->send();

$kernel->terminate($request, $response);
```

the Laravel's HTTP kernel is primarily responsible for handling incoming HTTP requests to the application. It is the central component that manages the request lifecycle in a Laravel application.

To understand how the Laravel HTTP kernel works, we need to firstly start by analysing the code that `index.php` is **using**, namely, `Illuminate\Contracts\Http\Kernel`.

### Analysing the `Illuminate\Contracts\Http\Kernel`:

The following is the code of the `Kernel.php` file which is namespaced as `Illuminate\Contracts\Http\Kernel`:

```PHP
<?php

namespace Illuminate\Contracts\Http;

interface Kernel
{
    /**
     * Bootstrap the application for HTTP requests.
     *
     * @return void
     */
    public function bootstrap();

    /**
     * Handle an incoming HTTP request.
     *
     * @param  \Symfony\Component\HttpFoundation\Request  $request
     * @return \Symfony\Component\HttpFoundation\Response
     */
    public function handle($request);

    /**
     * Perform any final actions for the request lifecycle.
     *
     * @param  \Symfony\Component\HttpFoundation\Request  $request
     * @param  \Symfony\Component\HttpFoundation\Response  $response
     * @return void
     */
    public function terminate($request, $response);

    /**
     * Get the Laravel application instance.
     *
     * @return \Illuminate\Contracts\Foundation\Application
     */
    public function getApplication();
}
```


This code is the interface for the HTTP Kernel in Laravel. 

An interface in PHP is a contract or a blueprint for a class. 

It defines a set of methods that the class must implement. In this case, any class that implements the `Illuminate\Contracts\Http\Kernel` interface must define the methods `bootstrap()`, `handle()`, `terminate()`, and `getApplication()`.

Here's what each method is intended to do:

1. **`bootstrap()`**: This method is responsible for bootstrapping the application for HTTP requests. It doesn't take any parameters and doesn't return anything (`void`).
2. **`handle($request)`**: This method is responsible for handling an incoming HTTP request. It takes a `Symfony\Component\HttpFoundation\Request` object as a parameter and returns a `Symfony\Component\HttpFoundation\Response` object.
3. **`terminate($request, $response)`**: This method is responsible for performing any final actions for the request lifecycle. It takes a `Symfony\Component\HttpFoundation\Request` object and a `Symfony\Component\HttpFoundation\Response` object as parameters and doesn't return anything (`void`).
4. **`getApplication()`**: This method is responsible for getting the Laravel application instance. It doesn't take any parameters and returns an `Illuminate\Contracts\Foundation\Application` object.


### The <span style="color: red;">question</span> that begs itself:

How does the following line of code works?:

```PHP
$kernel = $app->make(Kernel::class);
```

How is the `$app->make();` method able to instantiate the `Kernel` Interface using just its namespace as a string `Kernel::class`? 

In PHP, you can't instantiate an interface. In order to instantiate an Interface you need to create a non-abstract class that implements that interface.


### Short answer:

<div style="display:flex; align-items:center; justify-content:center; border:5px solid black;">
    <h1 style="color:red; margin-top: 0px;">Laravel Service Container</h1>
</div>

**Definition:** The **Laravel Service Container**, also known as the **IoC (Inversion of Control)** container, is a powerful **tool** that has a variety of **key features** that help managing a Laravel application.

The service container is essentially a <span style="color: red;">**box of objects (services)**</span> that the application needs to function. These services can be anything from database connections to mailer classes or custom-written services for your application. The container allows you to <span style="color: red; font-weight: bold">bind</span> these services into it, and then <span style="color: red; font-weight: bold">resolve</span> them out when you need them.


In Laravel, the `$app->make()` method is used to **resolve** a class out of the service container. 

When you call `$app->make(Kernel::class)`, Laravel is not trying to instantiate the `Kernel` interface. Instead, it's looking for a concrete implementation of that interface that has been bound into the service container.

Earlier in the `bootstrap/app.php` file, Laravel **binds** the `Kernel` interface to a concrete implementation:

```php
$app->singleton(
    Illuminate\Contracts\Http\Kernel::class,
    App\Http\Kernel::class
);
```

This code tells Laravel: "Whenever someone asks for the `Kernel` interface, give them an instance of `App\Http\Kernel`."

So, when you call `$app->make(Kernel::class)`, Laravel gives you an instance of `App\Http\Kernel`, which is a concrete class that implements the `Kernel` interface.

This is a fundamental part of Laravel's service container and is key to how Laravel handles dependency injection. It allows you to depend on abstractions (like interfaces) in your code, while Laravel takes care of providing the correct implementation.

{% hint style = "tip" %}

In Laravel, the `$app` variable is not globally accessible in all PHP files by default. However, Laravel provides a variety of ways to access the application instance (and thus the service container) when you need it.

1 **Dependency Injection**: Laravel's service container is primarily intended to be used with dependency injection. This means that instead of trying to access the `$app` variable directly, you type-hint the dependencies your class needs in its constructor, and Laravel will automatically inject them for you. This is the recommended way to access services in Laravel.

{% hint type = "tip" %}

Most of the time, when we talk about **Dependency Injection** in the context of Laravel, we're typically referring to the <span style="color: red;">**Automatic**</span> **Dependency Injection** feature provided by Laravel's Service Container.

**Dependency Injection** by **definition** is simply: a design pattern where an object's dependencies are provided to it, rather than the object having to create or find those dependencies itself. This can be done <span style="color: blue;">**Manually**</span>, by simply passing the dependencies to the object when it's **constructed**.

The following is a classic example of manually injecting the `$database` **dependency** to the `$userRepository` object instance of the `UserRepository` class through its constructor `new UserRepository($database);`:

```php
$database = new Database();
$userRepository = new UserRepository($database);

$user = $userRepository->findUserById(1);
```

{% endhint %}

The following is an example of using Laravel's service container dependency injection through type hinting:

```PHP

namespace App\Http\Controllers;

use App\Services\UserService;

class UserController extends Controller {
    protected $userService;

    public function __construct(UserService $userService) {
        $this->userService = $userService;
    }

    public function show($id) {
        $user = $this->userService->findUserById($id);

        // Return a view or JSON response with the user data
    }
}

```

In this example, the `UserController` depends on the `UserService`. By type-hinting the `UserService` dependency in the `UserController`'s constructor, Laravel's service container will automatically resolve and inject it.

---

2 **Facades**: Laravel's facades provide a "static" interface to classes that are available in the service container. Under the hood, facades use the service container to resolve the underlying class and proxy calls to it. For example, you can use the `App` facade to access the application instance anywhere in your code like this: `App::make('SomeClass')`.

---

3 **Helpers**: Laravel provides a number of global helper functions that can be used to access various services. For example, the `app()` function can be used to access the service container. If you call `app('SomeClass')`, it will resolve 'SomeClass' out of the container.

{% endhint %}

