# Redux Saga
It's a Redux middleware that handles async actions. Saga is more sophisticated than redux-thunk. It can manage side-effects (database, API calls etc.). Depends on __yield__ keyword. It consumes and emits action or creates a side-effects. It can also work without Redux. Saga maintains continuously running processes called sagas.


## A saga
+ In general function programing it's a series of reversible transactions 
+ Replaces single, locking transaction
+ Uses a process manager to manage the actions

## Saga in Redux
+ A long-running background process
+ Responsible for application's side effects
+ Used in conjunction with ES6 yield
+ Redux Saga is the process manager – the verious sagas are manipulated by the Saga itself
+ Listens for action, dispatches other actions (using effects)
+ Interact with external APIs or modify system files (using request, fs or other)

## Redux Thunk vs Redux Saga

|Redux Thunk|Redux Saga|
|:----------|:---------|
|Common Redux middleware|Common Redux middleware|
|Created by Redux creator (D. Abramov)|Created by third party developer|
|Runs in any JS context|Only runs in ES6 environments that support yield|
|Has no built-in solution for asynchronous calls|Uses yield and generator functions to simplify async|
|No way to orchestrate side-effects between thunks|Uses effects and plain actions to coordinate sagas|

## Yield
+ Special keyword that can delay the execution of subsequent code
+ Only works inside generator functions
+ Works with promises and codenses code surrounding them

``let data = yield api.cal(myUrl)`` handles the action in a more fashionable way than ES6 promise.

|Advantages|Disadvantages|
|:----------|:---------|
|Significantly fewer lines of code|Only works inside a generator function|
|Significantly less indentation (avoids callback hell)|Requires additional Babel plugins|
|Easiest to read quickly, reason about|Errors must be handled explicitly|
|Easier to debug|Unfamiliar to novice devs|
|Execution stops on unhandled error|Execution stops on unhandled error|

## Generator functions
+ Special JS function denoted by _*_
+ Calling function returns a generator
+ Actual code is executed by calling "next" method
+ Can yield multiple values

### Regular function
```
    function getValue(a, b) {
        const value = a + b;
        return a + b;
    }
    
    let data = getValue(1, 2);
```
With normal function, invocation of function return final value.

### Generator function
```
    function* getValue(a, b) {
        const value = a + b;
        return a + b;
    }
    let gen = getValue(1, 2);
    let data = gen.next().value;
```
With generator function, invocation returns a generator. Next must be called to get the final value.

### Yield and promises
+ Function call that follows yield keyword must return a promise (or object, or other valid structure)
+ Code execution resumes when promise is resolved
+ If promise throws an error, code stops at yield line

## Wrapping generators
+ Yielded promise must still be called manually by some code
+ Redux Saga wraps generators automatically
+ Co.js can wrap generators outside of Redux-Saga app

```
    function* getData() {
    ley data = yield api.call("/cart");
    return data + 5;
    };
    
    let gen = getData();
    let promise = gen.next();
    promise.then(data => {
        let value = gen.next(data);
    });
```
Generator function needs a value returned from API call to proceed. Wrapper code still needs .then somewhere to capture the response from API and pass it to a generator.

If this is wrapped in a redux-saga, we can do it this way:
 ```
     function* mySaga() {
         yield delay(500);
         yield delay(700);
         console.log("Saga complete");
     }
 ```
 
 As they are wrapped in the redux-saga, .then() is never manually called.

 ## Redux Saga effects
 It's a utility method provided by redux-saga. It returns an object containing instruction for redux-saga. The package generates the side effects, not the effect itself. 

 ### Categories of effects

 |Thread management|Action creation|Data seeding|Flow control|
 |:----------------|:--------------|:-----------|:-----------|
 |call|put|select|take|
 |fork|||takeEvery|
 |spawn||takeLatest|
 |apply||||
 |cancel|||

#### Take
One of the simplest redux-saga effects.
+ It pauses between concurrent lines of code
+ Code resumes when specified action is dispatched
+ Pnly one thread – multiple actions do not lead to multiple responses
+ Properties of action are passed as yielded variable

```
    let mySaga = function*() {
        consle.info("Saga begins!");
        const state = yield effects.take("SET_STATE");
        console.info("Got state...", state");
    }

    run(mySaga);
    dispatch({type:"SET_STATE", value: 32});
```

#### Put
+ Immediately dispatches an action to the rest of the app
+ Code execution does not pause
+ Like calling dispatch in react-redux

```
    let mySaga = function*() {
        consle.info("Saga begins!");
        const state = yield effects.take("SET_STATE");
        console.info("Got state...", state");
    }

    run(mySaga);

    let putSaga = function*() {
        yield effects.put({ type: "SET_STATE", value: 32});
    }

    run(putSaga);
```

mySaga completed running. It got value from putSaga. They both can be used to pass the data from one to another.

#### Call
+ Calls the specified methods
+ Equivalent to invoking the method directly
+ Used for testing

```
    let function = () => console.log("Called the function");

    let saga = function*() {
        yield function();
    }

    run(saga) // Called the function
    // Object {@@ redux-saga/TASK: true, id: 1, name: "saga", cont" undefined}

    saga = function*() { 
        yield effects.call(function);
    }

    // Same effect
```

#### Fork
+ Invokes the specified method (like call)
+ Can't access yielded variables
+ Caller continues without pausing execution

If parent process errors or is cancelled, all the forked processes are cancelled as well. Fork can put a finally block of forked method that is invoked during the cancellation. 

```
    function* fn() {
        while(true) {
            console.log("FN");
            yield delay(1000);
        }
    }

    let saga = function*() {
        while (true) {
            yield effects.fork(fn);
            yield delay(500);
        }
    }

    run(saga);
    // FN will be repeated every 1000 and 500 mls.
```

The forks can be called independently. 

#### TakeEvery
+ Works like take, except forks the specified method every time specified action is dispatched
+ Code execution resumes immediately in main thread

```
    let process = function*() {
        while(true) {
            console.log("Process loop");
            yield delay(1000);
        }
    }

    let saga = function8() {
        yield effects.takeEvery("START_PROCESS", process);
        console.log("Saga got to the end");
    }

    run(saga)
    disatch({type: "START_PROCESS});
    // Process loop is now working as we continue forking a new process
```

#### Cancel & Cancelled
##### Cancel
+ Stops a forked process
+ Stopped process will be cut off at most recent yield
+ finally is invoked in forked process

##### Cancelled
+ Method that returns *true* if callee process has been cancelled by caller
+ Used in finally block to determine if cancellation (not error) is cause of termination

```
    let process = function*() {
        try {
            while(true) {
                console.log("Process looped");
                yield delay(500);
            }
        } finally {
            const cancelled = yield effects.cancelled();
            console.info("Cancelled?, cancelled);
        }
    }

    let saga = function*() {
        let forked = yield effects.fork(process);
        yield delay(5000);
        yield effects.cancel(forked);
        console.info("DONE");
    }

    run(saga);
    // Process loops for some time and cancels it.
```

#### TakeLatest
+ Combination of fork, takeEvery and cancel
+ Forks child process each time specified action is dispatched, while keeping exactly one instance of the child process running

##### Sequence
1. Specified action is dispatched
1. Child process is forked in response
1. Child process runs in own thread
1. Specified action is dispatched again
1. Child process is cancelled
1. Go to step 2

```
    let process = function*() {
        let timesLooped = );
        while (true) {
            console.info(`Looped ${timesLooped++} times`);
            yield delay(500);
        }
    }

    let saga = function*() {
        yield effects.takeLatest("START_PROCESS", process);
    }

    run(saga);
    dispatch({type: "START_PROCESS"});
```
The loop count increments. If we dispatch another start process action, we create a new loop as the previous one has been stopped.

#### Select
+ Return a copy of the application's state when yielded to
+ Any passed selectors are invoked

#### Spawn
+ Creates a new process, similar to fork – caller is not interrputed
+ New process is not a child of caller – will not be cancelled if caller errors or is itself cancelled

```
    let process = function*() {
        let timesLooped = 0;
        while (true) {
            console.info(`Looped ${timesLooped++} times`);
            yield delay(500);
        }
    }

    let saga = function*() {
        yield effects.spawn(process);
        yield delay(2000);
        throw new Error();
    }

    run(saga)
```
We will get an uncought saga, but the process will continue to loop. This happens because of the feature of spawn: it's never stopped when an error occurs.

#### All
+ Combines numerous take statements into one
+ Code execution resumes when all actions have been dispatched (in any order)

### Channels
There are three different types of channels:
1. Action channel that buffers actions to be processed one at a time
1. Even channel connects app to outside event sources 
1. Generic channel communicates between two sagas

#### Action channels
+ Records all events with specified type
+ Calling take accesses and removes oldest record
+ Used to handle actions that would otherwise be lost

```
    function* updateSaga() {
        let chan = yield actionChannel("UPDATE");
        while (true) {
            yield effects.take(chan);
            console.info("Update logged");
            yield delay(1000);
        }
    }

    run(updateSaga);
    dispatch({type: "UPDATE"});
```

The process slowly updates as the action comes to the channel.

#### Generic channels
+ Creates a special line of communication between two sagas
+ Action types is not required

```
    function* saga() {
        let chan = yield channel();
        function* handleRequest(chan) {
            while(true) {
                let payload = yield effects.take(chan);
                console.info("Got payload", payload);
                yield delay(1000);
            }
        }
        yield effects.fork(handleRequest, chan);
        yield effects.fork(handleRequest, chan);

        yield effects.put(chan, {payload: 42});
        yield effects.put(chan, {payload: 42});
        yield effects.put(chan, {payload: 42});
        yield effects.put(chan, {payload: 42});
    }
```

Payload is gotten by respective handlers. If we loop this three times, we will have first handler that has 2 payloads and second that has one.

#### Event channel
+ Wraps an outside source of events (i.e. WebSocket)
+ Sagas can take from event channel (the outside events)
+ Event channel converts events into take-able actions and emits them


## Testing redux-saga applications
+ Tests need to avoid making real AJAX calls
+ Effects do not do anything unless run by redux-saga
+ Call effects must be used instead of yielding directly to API methods

```
    function* mySaga(sessionKey) {
        let users = yield api.fetchUsers(sessionKey);
    }
```
This saga is untestable, invoking the generator will actually call API.

``` 
    function* mySaga(sessionKey) {
        let users = yield call([api, api.fetchUsers]),(sessionKey);
    }
```

Testable saga yields a call effect, no outside APIs are called except when run in redux-saga. Passing an array to call allows context to be specified for methods.

### Methods for testing redux-saga applications

#### Official methods
+ Saga is executed as plain generator
+ Tests pass mock values to next()
+ Structure of effects is tested agains expected values
+ Store is never used

#### Alternate methods
+ Mock store and application state are created
+ Entire saga is run from beginning to the end
+ At completion, new state is compared to expected value
+ APIs must be injected as dependencies

|Standard (unit tests)|Alternate (end-to-end tests)|
|:--------------------|:---------------------------|
|Requires that call be used for functions|Call usage recommended but not required|
|Cannot test application state against expected values|Can test application state against expected values|
|Outside APIs can be imported with no special considerations|Any outside APIs must be injected as dependencies|
|Test are brief and simple to set up|Tests are cmplex and require preparation of mock store and APIs|
|Test fails if yielded effects do not match expected values|Test fails if final application state does not match expected values|