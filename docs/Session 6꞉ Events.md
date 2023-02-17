---
tags: [Courses]
title: 'Session 6: Events'
created: '2023-02-16T13:54:06.505Z'
modified: '2023-02-16T14:00:19.539Z'
---

# Session 6: Events
Node.js provides an Event Emitter class which allows us to create and handle events in a simple and intuitive way. In this session, we will cover how to use the Event Emitter class to handle events.


## Introduction to the Event Emitter class
The Event Emitter class is a built-in class in Node.js that allows us to create custom events and handle them. We can use it to listen for specific events and execute functions when those events are emitted. Here's an example:
```js
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();

eventEmitter.on('greet', () => {
    console.log('Hello world!');
});

eventEmitter.emit('greet');
```

In this example, we create a new instance of the `EventEmitter` class and attach a listener to the `greet` event using the `on` method. When the greet event is emitted using the `emit` method, the attached listener function will be executed and "Hello world!" will be logged to the console.

## How to inherit events
The Event Emitter class can be extended to create new classes that emit their own events. Here's an example:
```js
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();
myEmitter.on('myEvent', () => {
    console.log('My event was emitted!');
});

myEmitter.emit('myEvent');

```
In this example, we extend the `EventEmitter` class to create a new `MyEmitter` class. We then create an instance of the `MyEmitter` class and attach a listener to the `myEvent` event using the `on` method. When the `myEvent` event is emitted using the `emit` method, the attached listener function will be executed and "My event was emitted!" will be logged to the console.

## How to return event emitters
We can also return instances of the Event Emitter class from functions to allow the caller to attach their own listeners. Here's an example:
```js
const EventEmitter = require('events');

function getEmitter() {
    const emitter = new EventEmitter();

    setTimeout(() => {
        emitter.emit('ready');
    }, 1000);

    return emitter;
}

const myEmitter = getEmitter();
myEmitter.on('ready', () => {
    console.log('Emitter is ready!');
});

```

In this example, the `getEmitter` function creates a new instance of the `EventEmitter` class, sets a timeout, and returns the emitter. We then call the `getEmitter` function and attach a listener to the `ready` event using the on method. When the ready event is emitted by the emitter returned by the `getEmitter` function, the attached listener function will be executed and "Emitter is ready!" will be logged to the console.




