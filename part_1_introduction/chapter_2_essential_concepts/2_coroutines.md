## 2.2 Coroutines

**Definition**: a coroutine is an instance of suspendable computation. It is conceptually similar to a thread, in the sense that it takes a block of code to run that works concurrently with the rest of the code. However, a coroutine is not bound to any particular thread.

{% hint type="tip" %}

1. Coroutines are the smallest program unit **That is not bound to a CPU** that can run concurrently.  

2. With the use of coroutines you can achieve concurrency with a single thread.

{% endhint %}

### The original and general use of Coroutines

//todo

### The use of Coroutines in Asynchonous Programming

{% hint style="info" %}

In asynchronous programming coroutines are just functions in the main thread that are tagged to be assigned to any other thread in the pool to be executed. It is a way to abstract threads and reduce the complexity of handling them since you don't interact with a thread directly instead you use coroutines from the main thread.

**coroutines are mainly(but not only) used in asynchronous programming**

{% endhint %}

#### a coroutine in javascript (in a javascript runtime)

```
let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("Promise resolved!"), 1000);
});

promise.then((value) => console.log(value));

async function coroutine() {
    let response = await fetch('https://api.github.com/users/octocat');
    let data = await response.json();
    console.log(data);
}

coroutine();
```

#### a coroutine in php using the swoole extension

```
<?php
use Swoole\Coroutine;

Coroutine\run(function () {
    $client = new Swoole\Coroutine\Http\Client('api.github.com', 443, true);
    $client->set(['timeout' => 1]);
    $client->setHeaders([
        'Host' => "api.github.com",
        "User-Agent" => 'Chrome/49.0.2587.3',
        'Accept' => 'application/json',
        'Accept-Encoding' => 'gzip',
    ]);
    $client->get('/users/octocat');

    echo $client->body;
});
?>
```