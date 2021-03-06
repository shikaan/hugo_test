---
title: "Suites"
weight: 3
---

Suites are containers for test specifications (see [specs](./specs)).

Along with their grouping function, suites have the fundamental role of
allowing the developer to prepare the testing environment. This happens
through two phases: the **setup phase** which happens _before_ the actual
test suite is run and the **teardown phase** which is supposed to happen
_after_.

In **Titef** you can control those phases via the configuration object
you will pass to the `suite` method.
Also, you can go deeper controlling what happens before and after each
specification.

# Writing suites

Suites can be written using the methods: `suite`, `describe`. 
They accept the following parameters:

| Parameter 	| Type            	| Description                                                                     	|
|-----------	|------------------	|---------------------------------------------------------------------------------	|
| title     	| `String`        	| Title of the test suite. It will be used to generate the report                 	|
| options   	| `Options?`      	| This adds the setup and teardown phase to your test suite                       	|
| callback  	| `Function`  `AsyncFunction` 	| Where you will host your `specs`.  It runs after `setup` and before `teardown`. 	|


Where the `options` object is shaped this way


| Attribute 	| Type       | Description                                 |
|-----------	|----------- |-------------------------------------------- |
| silent     	| `Boolean`  | Exclude the suite from reporters            |
| setup     	| `Function` | What should happen _before_ the suite runs? |
| teardown   	| `Function` | What should happen _after_ the suite runs?  |
| eachSetup     | `Function` | What should happen _before each spec_ runs? |
| eachTeardown  | `Function` | What should happen _after each spec_ runs?  |


# Remarks and caveats

As opposed to `spec` and `xspec`, `suite`s are synchronous.

# Example

```javascript
suite(
  'My first test suite',
  {
    setup() {
      console.log('I am setting up the environment!')
    },
    teardown() {
      console.log('Test suite ended. Should I report anything?')
    }
  },
  () => {
    // Collection of specs
  })
```

```javascript
describe(
  'My second test suite',
  {
    setup() {
      console.log('I am setting up the environment!')
    },
    teardown() {
      console.log('Test suite ended. Should I report anything?')
    }
  },
  () => {
    // Collection of specs
  })
```