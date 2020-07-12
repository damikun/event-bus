# Event Bus

[![Build Status](https://travis-ci.org/arkency/event-bus.svg?branch=master)](https://travis-ci.org/arkency/event-bus)

Simple event bus for your JavaScript application without any dependencies and with a low size footprint.

## Installation:

### Node.js:

```
npm install --save eventing-bus
```

### Webpack:

```
yarn add eventing-bus
```

or for NPM:

```
npm install --save eventing-bus
```

## Global event bus

By default exports in JavaScript are evaluated only once. This way we can ensure to have one global event bus without doing anything on our side.

There is also a possibility to define an event stream [separately from the global bus](#more-than-one-event-bus).

## Subscribing to events:

````javascript
import EventBus from 'eventing-bus'

var callback = function(name) { console.log("Hello, " + name + "!"); };

EventBus.on("exampleEventName", callback);
````

## Publishing events:

````javascript
import EventBus from 'eventing-bus';

EventBus.publish("exampleEventName", "Watson");
/* After registering the subscription and publishing an event you should see
   "Hello, Watson!" printed in your console. */
````

## More than one event bus:

By default you have only one, singleton event bus instance which holds subscriptions from all parts of your application. But nothing stands on your way to create your own, private instances (for example, for each logically distinct part of your complex app):

````javascript
import EventStream from 'eventing-bus/lib/event_stream';

/* You can use EventStream both as a constructor and as a factory function. */
var privateBus = EventStream();
var newPrivateBus = new EventStream();
````

Those _streams_ created by you won't share any subscriptions, nor events.

## Unregistering a single subscription:

If you need to unregister a subscription (a typical case would be inside the React.js component), it is as easy as calling a return value of the `#on` method as a function:

````javascript
import EventBus from "eventing-bus";

var subscription = EventBus.on('event', function() {
  // ...
});

// This will unregister this (and only this) subscription.
subscription();
````

## Unregistering subscriptions in bulk:

Since by default `EventBus` is a singleton instance of the bus, there may be occasions where you need to unregister all subscriptions (most notably - during testing). It can be done by calling following methods:


#### `#unregisterAllCallbacks`

Removes all event handlers.

````javascript
import EventBus from "eventing-bus";

EventBus.unregisterAllCallbacks();
````

#### `#unregisterCallbacksForEvent`

Remvoes all event handlers for specific event

````javascript
import EventBus from "eventing-bus";

EventBus.on('exampleEvent', () => { console.log("EXAMPLE") });
EventBus.publish("exampleEvent"); // Logs `EXAMPLE`

EventBus.unregisterCallbacksForEvent('exampleEvent');

EventBus.publish("exampleEvent"); // Empty output
````

## Compatibility

If you want to use this library on legacy browsers (IE <= 8 etc.), you need to
provide polyfills for `Array.forEach` and `Array.filter` functions. Check out e.g.
 [es5-shim](https://github.com/es-shims/es5-shim) to read more.

## Contributing

Feel free to report any issue or idea on the GitHub page of this project. If you report an issue, please try to provide reproducing steps or any piece of code that can reproduce the issue.
