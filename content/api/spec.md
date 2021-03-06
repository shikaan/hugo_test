---
title: "Specifications"
weight: 2
---

Specifications are basically functions: as long as no `AssertionError` is
thrown, the specification is assumed to be passed.

Given that, every `spec` should always contain one assertion in order to
be useful. Also, every `spec` is expected to live in a `suite` or in a 
`describe`.

# Writing specifications

Specifications can be written using the methods: `it`, `spec`. 
They accept the following parameters:

| Parameter 	| Type          	        | Description                                              |
|-----------	|------------------------	|----------------------------------------------------------|
| title     	| `String`        	        | Title of the spec. It will be used to generate the report|
| callback  	| `Function`  `AsyncFunction` 	| Where you will host your assertions.                     |

# Excluding specifications 

Sometimes you maybe want to ignore certain test specifications. To do so
just add an `x` (as _exclude_) in front of you spec: `xspec`, `xit`.

Same parameters as above:

| Parameter 	| Type          	        | Description                                              |
|-----------	|------------------------	|----------------------------------------------------------|
| title     	| `String`        	        | Title of the spec. It will be used to generate the report|
| callback  	| `Function`  `AsyncFunction` 	| Where you will host your assertions.                     |

# Remarks and caveats
In **Titef** any `spec` can be either synchronous or asynchronous. Be
careful though: once you use the `async` keyword make sure your specs
are independent before going for an `async` flow, because they will
run concurrently and can potentially lead to unexpected results.

This namely means that `eachSetup` and `eachTeardown` callbacks are still
executed before and after each `spec`, but there is no guarantee that - 
when running more than one `spec` - the order of said callbacks matches 
the order of the `spec`s.

For example, the following snippet
```javascript
suite('suite', {
  eachSetup() {
    console.log('setup')  
  },
  eachTeardown() {
    console.log('teardown')
  }
}, async () => {
  spec('spec1', async () => {
    console.log('spec1');
  });
  spec('spec2', async () => {
    console.log('spec2');
  })
})
```
might yield the following outputs
```
setup
spec1
teardown
setup
spec2
teardown
```
```
setup
spec1
setup
spec2
teardown
teardown
```

As you can see the order setup/spec/teardown is preserved for each spec, 
but it's not preserved across different specs. 

If you're not interested in the order of execution of the specs and 
you just care for the order of reporting, you can sleep tight: 
**Titef** will always report result in the same order as the specs 
are written.

# Examples
```javascript
const assert = require('assert');
const myClass = require('./my-class');

suite('myClass:myMethod', () => {
  spec('exists', () => {
    assert.ok(!!myClass.myMethod);
  })
  xspec('i am excluded', () => {
    assert.false(true);
  })
})
```
```javascript
const assert = require('assert');
const myClass = require('./my-class');

describe('myClass:myMethod', () => {
  it('exists', () => {
    assert.ok(!!myClass.myMethod);
  })
  xit('i am excluded', () => {
    assert.false(true);
  })
})
```