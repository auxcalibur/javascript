# JavaScript Style Guide;

A collection of JavaScript / React / React Native style guides

> **Note**: this style guide is an accumulation of coding standards from different sources (e.g. AirBnB, Google, etc.) as well as my own inputs based on my experiences with JavaScript and React.

Here's the one for [React](https://github.com/auxcalibur/javascript/react).

## Table of Contents

  1. [References](#references)
  1. [Objects](#objects)

## References

  <a name="references--prefer-const"></a><a name="1.1"></a>
  - [1.1](#references--prefer-const) Use `const` for all of your references; avoid using `var`.

    > Why? This ensures that you can’t reassign your references, which can lead to bugs and difficult to comprehend code.

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="1.2"></a>
  - [1.2](#references--disallow-var) If you must reassign references, use `let` instead of `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html)

    > Why? `let` is block-scoped rather than function-scoped like `var`.

    ```javascript
    // bad
    var count = 1;
    if (true) {
      count += 1;
    }

    // good
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

... work-in-progress