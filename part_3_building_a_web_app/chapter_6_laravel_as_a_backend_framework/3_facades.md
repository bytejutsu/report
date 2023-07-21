## 6.3 Facades

{% hint style = "info" %}

Laravel's facades provide a "static" interface to classes that are available in the service container. When you use `App::make('SomeClass')`, you're using the `App` facade to access the service container and resolve the `SomeClass` out of it.

Under the hood, Laravel's facades use the service container to resolve the underlying class and proxy static method calls to it. This means that when you call a static method on a facade, Laravel is actually resolving the underlying class from the service container and calling the method on that instance.

So in essence, when you use a facade, you're using the service container twice: 

1. once to resolve the facade itself, 
2. and once to resolve the class that the facade provides access to.

This allows you to use complex services as if they were simple, static methods, while still benefiting from Laravel's powerful service container and dependency injection features..

{% endhint %}