# Difference in the eventloop execution order when using CommonJS modules vs ESM

The order of execution of `nextTick` queue and resolved `Promise` queue gets reversed depending on the module system used.

## Concept
The event loop contains of several phases prominent among which are Timer, IO, Polling, SetImmediate and Close phases.
In between the execution of _each callback_ in all of these phases if there is any callback queued in the *microtask queue* (`nextTick` and `Promise` queue)
then those call backs are processed till the exhaustion of the queues before proceeding with the eventloop.

If CommonJS module system is used then the `nextTick` queue is processed first before mocing on to the `Promise` queue, but in ESM the `Promise` queue is processed first before processing the `nextTick` queue.

## Code
```javascript
// index.js - CommonJS Module

process.nextTick(() => {
    console.log("From next Tick callback");
})

Promise.resolve().then(() => {
    console.log("From Promise callback");
})

// Output
// From next Tick callback
// From Promise callback
```

```javascript
// index.mjs - ESM 

process.nextTick(() => {
    console.log("From next Tick callback");
})

Promise.resolve().then(() => {
    console.log("From Promise callback");
})

// Output
// From Promise callback
// From next Tick callback
```