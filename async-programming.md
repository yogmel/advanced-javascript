## Asynchronous Programming

[**Go back to Summary**](README.md#summary);

- [Callback](#callback);
- [Promises](#promises);
  - [Promise.all](#promiseall);
- [Async and await](#async-and-await);

---------

Async operations are not executed in order. They are mostly used when performing an API request or downloading some media (videos, audio, etc).

- [Asynchronous Programming - Eloquent Javascript](https://eloquentjavascript.net/11_async.html)

### Callback
Callback is a function that is passed as an argument to another one. Callbacks are not async.

```javascript
function doAsyncTask(cb) {
  setTimeout(() => {
    cb(null, "The correct data");
  }, 0); // force async
}

doAsyncTask((err, data) => { // first parameter is the error
  if(err) {
    throw err
  } else {
    console.log("Data, ", data)
  }
})
```

When a function is async, it can be hard to know in what order it will execute. For that reason, a **callback hell** can happen. That is, functions being nested one after another, defining the calls' order.

- [Callback Hell](http://callbackhell.com/)

### Promises

- [Promise - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

- See also: [Fetch - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

Promises are async by default. It handles the problem of the callback hell, by representing a completion or failure of an async operation. `.then()` can be called to perform an operation after the promise is resolved.

```javascript
function doAsyncTask() {
  const promise = new Promise((resolve, reject) => {
    console.log("Async Work Complete");
    if(false) {
      resolve({ x:1 });
    } else {
      reject( "Error" );
    }
  });
  return promise;
}

doAsyncTask().then(
  (val) => console.log(val),
  (err) => console.log(err)
);
```

Promises also can be chained. The returned value will be sent as a **parameter** to the second promise in the chain.

When an error occurs, it will pass it over the chain until it finds an *error handler*, so there is no need to add error handlers in all steps of it. `throw` also works, but more usually it is used the `.catch()` method.

An optional method `.finally()` can be called at the end of the chain, and the operation inside will execute either it is resolved or rejected.

A promise fork happens when promises are called separately.

```javascript
let promise = Promise.resolve("done");

// promise chain
promise.then(val => {
  console.log(val);
  return "done2"
}).then(val => console.log(val))
  .catch(err => console.log(err)); // error handler
  .finally(_ => console.log("cleaning up"))

// promise fork
promise.then(val => {
  console.log(val);
  return "done2"
})

promise.then(val => {
  console.log(val); // this keeps the first value, so it prints "done"
})
```

#### Promise.all
When multiple promises are called and there is the need to handle its resolution, `Promise.all` is used.

```javascript
const doAsyncTask = delay => {
  return new Promise(res => setTimeout(() => res(delay), delay))
}

let promises = [doAsyncTask(1000), doAsyncTask(2000), doAsyncTask(500)]

Promise.all(promises).then((values) => console.log(values))
// after 2 seconds, a single log is printed with the three values
```

### Async and await
When calling an async function, to prevent the script from running before its resolution, the `await` keyword is used. It only works on `async` functions.

It is important to know that this causes a _blocking_ operation, that is, nothing runs until that function is resolved or rejected.

```javascript
const doAsyncTask = () => Promise.resolve("done");

(async function() {
  let value = await doAsyncTask(); // await causes this to be blocking
  console.log(value); // will be done, rather than undefined
})();
```

`async`/`await` can also be used in conjunction with `.then()`.

```javascript
let asyncFunc = async function() {
  let value = await doAsyncTask();
  console.log(value);
  console.log(2);
  return 3;
}

asyncFunc().then(v => console.log(v)) // prints 3
```

Error handling with promises and `await` is intuitive, as when the promise is rejected, it throws an error that can be caught.

```javascript
const doAsyncTask = () => Project.reject('error'); // throws an error

let asyncFunc = async function() {
  try {
    const value = await doAsyncTask();
  } catch (e) {
    console.log("moo: ", e);
    return;
  }
}

asyncFunc();
```
