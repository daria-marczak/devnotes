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