#### Usage

```js
const ExtendableError = require('extendable-error-class');

class MyError extends ExtendableError {
    constructor(m) {
        super(m);
    }
}

```
`extendable-error-class` has no dependencies.


### Why

Because the following doesn't work as expected.
```js
class MyError extends Error {
    constructor(m) {
        super(m);
    }
}
```



### How

The workaround is based on `babel-plugin-transform-builtin-extend` and following code

```js
const ExtendableError = class extends Error {
    constructor(message) {
        super(message);
        this.name = this.constructor.name;
        this.message = message;
        if (typeof Error.captureStackTrace === 'function') {
            Error.captureStackTrace(this, this.constructor);
        } else {
            this.stack = (new Error(message)).stack;
        }
    }
};
```

The code is already compiled with `babel` and `babel-plugin-transform-builtin-extend`. Therefore one is able to use this workaround without adding any dependency.

See http://stackoverflow.com/questions/31089801/extending-error-in-javascript-with-es6-syntax
