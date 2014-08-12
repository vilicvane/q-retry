# Q-retry plugin v0.1 for Q

> TypeScript definitions available (DefinitelyTyped).

## Install

```
npm install q-retry
```

**Note**

Q-retry set Q as [peer dependency](http://blog.nodejs.org/2013/02/07/peer-dependencies/),
it will add retry method directly to the same Q you were working with,
but will not add retry to the nested Q module of other modules and its instance.

So make sure the promise you want to use retry on is created or wrapped by non-nested Q module.
(Currently simply Q(promise) will return the original Q promise directly,
I have created a pull request https://github.com/kriskowal/q/pull/566 and hope they will have it merged.)

## Use

```javascript
var Q = require('q-retry');

Q
    .retry(function () {
        if (Math.random() < 0.8) {
            throw new Error('err 1');
        }

        return Q.delay(Math.floor(Math.random() * 1000))
            .then(function () {
                if (Math.random() < 0.8) {
                    throw new Error('err 2');
                }
                return 'expected result.';
            });
    }, function (reason, retries) {
        console.log('failed with ' + reason + ', ' + retries + ' retries left.');
    }, 100)
    .then(function (str) {
        console.log(str);
    })
    .fail(function (reason) {
        console.log(reason);
    });
```

## API

```javascript
/**
 * retry a promise process and return a promise with the fulfilled value.
 * @param process a function like onFulfilled for then method.
 * @param onFail a function takes the reason and the number of left retries as parameters.
 * @param options a number represents for retries limit or an object for more options.
 */
Q.retry(process, [onFail], [options]);
promise.retry(process, [onFail], [options]);
```

options:

**limit** a number represents for retries limit.  
**interval** interval (milliseconds) between each retries.  
**maxInterval** max interval when interval multiplier applied.  
**intervalMultiplier** the multiplier applies to interval after every retry.  