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

### Simple command line argument getter
```javascript
function getArg(argv, name) {
    // search for name from index 2
    const argIndex = argv.indexOf('-' + name, 2);
    if (argIndex === -1) {
        return null;
    }

    let argValue = argv[argIndex + 1];
    return typeof argValue == 'string' && argValue[0] !== '-' ? argValue : true;
}
```
### Naive and simple delayed call (usefull for testing)
```javascript
/**
 * Promise version of delayed function call. Good for testing, great with async/await to flatten the code
 * @param {Function} fn
 * @param {number} delay
 * @return {Promise}
 */
function delayedCall(fn, delay) {

    return new Promise((resolve, reject) => setTimeout(() => fn() || resolve(), delay));
}
```
### Semioptimal & optimal version of fibonacci function
```javascript
// semioptimal
function fibonacci(n) {

    let result = [0, 1, 1, 2];
    let index = result.length;
    while (n >= index) {
        result.push(result[index - 1] + result[index++ - 2]);
    }

    return result[n];
}

// optimal version
function fibonacciOptimal(n) {

    if (n === 0) return 0;
    if (n === 1 || n === 2) return 1;

    let tmp, n1 = 1, n2 = 1, index = 2;
    while (n > index++) {
        tmp = n2;
        n2 = n2 + n1;
        n1 = tmp;
    }

    return n2;
}
```
