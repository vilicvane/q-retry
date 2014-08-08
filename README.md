# Q-retry v0.1 (based on Q)

## Install

```
npm install q-retry
```

## Use

```javascript
var Q = require('q-retry');

Q.retry(function () {
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
promise.retry(process, [onFail], [options]);
```

options:

**limit** a number represents for retries limit.  
**interval** interval (milliseconds) between each retry.  
**maxInterval** max interval when interval multiplier applied.  
**intervalMultiplier** the multiplier applies to interval after every retry.  