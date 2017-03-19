# Installation

```Bash
npm install jest-cli --save-dev
```

```
// package.json
{
    "scripts": {
        "test": "jest"
    },
}
```

jest auto find
- *.spec.js
- *.test.js
- __TEST__/*
- __SPEC__/*

# Demo


```js
// sum.js
module.exports = sum

function sum(a, b) { return a + b }
```

```js
// sum.test.js
const sum = require('./sum')

test('adds 1 + 2 to equals 3', () => {
    expect(sum(1,2)).toBe(3)
})
```

# package.json

```json
{
    ...
    "jest": {
        "testEnvironment": "node"
    }
}
```




