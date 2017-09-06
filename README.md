# Useful code snippets

## Javascript 

### Actual type of value (returns string)
```javascript
// to get an actual type of the value
const type = typeof value == 'object' && value !== null ?
        Object.getPrototypeOf(value).constructor.name :
        typeof value;
```

### Lazy value & getter
```javascript
// /**
//  * @param  {function} lazyFunction
//  * @return {function}
//  */
function createLazyValue(lazyFunction) {

    if (typeof lazyFunction != 'function') {
        throw new Error(`'lazyFunction' parameter should be of type 'funtion' but is ${typeof lazyFunction}!`);
    }

    let value = undefined;
    let called = false;

    return function(...args) {

        if (called) {
            return value;
        } else {
            value = lazyFunction(...args);
            called = true;
            return value;
        }
    };
}

/**
 * @param  {Object} instance
 * @param  {*} key
 * @param  {function} lazyFunction
 * @return {Object}
 */
function createLazyGetter(instance, key, lazyFunction) {

    Object.defineProperty(instance, key, {
        enumerable: true,
        configurable: true,
        get: createLazyValue(lazyFunction)
    });

    return instance;
}
```

### Floating point error treshold & equality
```javascript
// compute the threshold
const THRESHOLD = (0.1 + 0.2 - 0.3) * 2;

function equals(a, b, threshold = THRESHOLD) {

    return (a < b ? b - a : a - b) < threshold;
}

if (true /*run tests*/) {
    const a = 0.1 + 0.2;
    const b = 0.3;
    const b2 = 0.300000000000001;
    const b3 = 0.3000000000000001;

    // item: [ computed_equality_value, expected_result ]
    const cases = [
        [a === b, false],
        [equals(a, b), true],
        [equals(a, b2), false],
        [equals(a, b3), true]
    ];

    cases.forEach((item, index) => {
        if (item[0] != item[1]) {
            throw new Error(`Test case with index ${index} failed! ${item[0]} should equal to ${item[1]}!`);
        }
    });
}
```
