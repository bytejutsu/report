## 5.2 Events

In this section, we will provide an overview of what events are in the context of Laravel and why they are useful. 


1. Events provide a simple observer implementation, allowing you to subscribe and listen for various events in your application.


2. They serve as a great way to decouple various aspects of your application, as the event object can be used to share data between the event listeners.

 
3. They can be used for tasks like sending notifications, firing off jobs to queues, writing to logs and more.


### Understanding Laravel Events
 

1. Events in Laravel can be classified into two main categories: persistent events and request-specific events.
 
    - Persistent events are those that persist between subsequent HTTP requests. A good example of these are job events, which may be processed in the background and not within the lifecycle of a single HTTP request.

    - Request-specific events are those that occur within the lifespan of a single HTTP request. They get fired and handled during a single HTTP request and do not persist across multiple requests.

 
2. Event classes are typically stored in the `app/Events` directory.

 
3. Events can be broadcast over websockets to provide real-time updates to your application's users.

 
4. Laravel's event broadcasting allows you to broadcast your server-side Laravel events to your client-side JavaScript application.


### Creating Events


1. Creating an event using `php artisan make:event EventName`.

 
2. Explain the structure of the generated event class.

 
3. Show an example of defining event and setting data on it.


### Event Listeners

An Event Listeners listens to a particular event and performs an action when that event is fired.


1. Creating a listener using `php artisan make:listener ListenerName --event=EventName`.

 
2. Explain the structure of the generated listener class.

 
3. Show an example of how to set up a listener to respond to an event.


### Event Subscribers


Event Subscribers are classes that may subscribe to multiple events from within the class itself.


This can provide a way to keep related listener logic grouped together.


To use an Event Subscriber we need to:


1. Firstly create an event subscriber.
 

2. Secondly register the event subscriber.


Todo: Give examples of when using an event subscriber might be preferable to individual listeners.


### Broadcasting Events


To broadcast events using Laravel, we need to: 


1. Firstly to configure Laravel to use a broadcasting service like Pusher or Laravel Websockets in the `.env` file and the `config/broadcasting.php` file.


2. Secondly to mark an event as "broadcastable" by implementing the `ShouldBroadcast` or `ShouldBroadcastNow` interfaces.


This takes us to the next chapter where we talk about websockets and the websocket server.


