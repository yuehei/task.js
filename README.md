task.js
=======
A beautiful way to control your task flow, simple and powerful. take a look at [Demo](http://michalliu.github.com/task.js/) and [Automatic test](http://michalliu.github.com/task.js/test/index.html)

Usage
-----
Think of the following code

    setTimeout(function () {
        // do something
        setTimeout(function () {
            // do something
            setTimeout(function () {
                // do something
                setTimeout(function () {
                    // do something
                    setTimeout(function () {
                      // do something
                    },1000)
                },1000)
            },1000)
        },1000)
    },1000);

By this tiny script `task.js`, you just need this

    task()
    .sleep(1000)
    .run(function () {})
    .sleep(1000)
    .run(function () {})
    .sleep(1000)
    .run(function () {})
    .sleep(1000)
    .run(function () {})
    .sleep(1000)
    .run(function () {})
    .start();
    
Instance method
----
1. **run(fn)**
    
    run function

    **params:**
    
    `fn` - the function to be executed, the value fn returned will be the parammeter of the next fn. so it is easy to break a complex operation to mutiple simple tasks using `task.js`
    
    **returns:**
    
    the task object itself
    
2. **sleep(time)**

    sleep some time
    
    **params:**
    
    `time` - the waiting time to perform the next operation. time can be a function or number, if time is function, it use the returned value as the waiting time,and it will try to convert return value to number, it's parameter is the return value from last "run" function. so, it is easily to dynamiclly generate the timeout value according to the result of last opteration.

    **returns:**
    
    the task object itself
    
3. **start()**
    
    start execute task

    **params:**
    
    this function takes no params
    
    **returns:**
    
    this function has no return value

3. **cancel()**
    
    stop execute task, the interrupt handler will be executed, see interrupt method

    **params:**
    
    this function takes no params
    
    **returns:**
    
    this function has no return value

4. **progress(fn)**

    set the callback function to execute after each small task is executed
    
    `fn` - the callback function. this function will be passed three parameters. `step` - the index of "run" function, `totalStep` - the total step of the task, `currentRepeat` - the index of repeat, `totalRepeat` - total repeat times.
    
    **returns:**
    
    the task object itself
    
5. **done(fn)**

    set callback function when task is done, if the task is protected, that means there's no assertion errors. if task isn't protected, that means the task has finished.
    
    **params:**
    
    `fn` - the callback function when task is done
    
    **returns:**
    
    the task object itself

6. **interrupt(fn)**

    set callback function when task is interrupted. when the task is protected and there is assertion fail, the task will be interrupted.
    
    **params:**
    
    `fn` - the callback function when task is done. `detail` - fn will be fulfilled with error details.
    
    **returns:**
    
    the task object itself
    
7. **repeat(times)**

    set the times to repeat to do the task
    
    **params:**
    
    `times` - the times to repeat to do the task
    
    **returns:**
    
    the task object itself
    
8. **assertEquals([fn,fn,...])**
    
    assert parammeters or it's return value is equal

    **params:**
    
    `fn` - the function to be executed. it's parameter is the return value from last "run" function.
    
    **returns:**
    
    the task object itself
    
9. **assertNotEquals([fn,fn,...])**

    assert parammeters or it's return value isn't equal

    **params:**
    
    `fn` - the function to be executed. it's parameter is the return value from last "run" function.
    
    **returns:**
    
    the task object itself
    
10. **assertFalse(fn)**

    assert parammeters or it's return value is false

    **params:**
    
    `fn` - the function to be executed. it's parameter is the return value from last "run" function.
    
    **returns:**
    
    the task object itself
    
11. **assertTrue(fn)**

    assert parammeters or it's return value is true

    **params:**
    
    `fn` - the function to be executed. it's parameter is the return value from last "run" function.
    
    **returns:**
    
    the task object itself

12. **protect()**

    protect the queue, once any assertion failed, the task is finished. this is the default behaviour.
    
    **params:**
    
    this function takes no params
    
    **returns:**
    
    this function returns nothing

13. **unprotect()**

    unprotect the queue, ignore the assertion failed event, continue to execute the rest task.
    
    **params:**
    
    this function takes no params
    
    **returns:**
    
    this function returns nothing

Static method
----

**onAssertionFail**

Hook the assertion fail event
    
    task.onAssertionFail = function (err) {
        // err object contains error info
        // or you can just throw it
        throw err;
    }

FAQ
===

What will happen if assertion failed?
----

it follows the following routine

1. if `onAssertionFail` is defined, just let `onAssertionFail` handle it
2. if `window` has `console` object, and console has assert method, let `window.console` handle it
3. if no handlers, just throw an `AssertionFail` error
4. stop the rest tasks

Show me some examples
----

```javascript
var i=1;
var result = 0;

task().run(function () {
    result = result + i;
    i = i + 1;
}).repeat(100).done(function () {
    console.log(result); // output 5050
}).start();
```

```javascript
var i=1;
var result = 0;

task().run(function () {
    result = result + i;
    i = i + 1;
}).sleep(500).run(function () {
    console.log(result); // output result every 500ms
}).repeat(100).done(function () {
    console.log(result); // output 5050
});
}).start();
```
