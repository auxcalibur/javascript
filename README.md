# JavaScript Style Guide;

A collection of JavaScript / React / React Native style guides

Here's the one for [React](https://github.com/auxcalibur/javascript/react).

## Table of Contents

  1. [Introduction](#introduction)
  1. [Source Files](#source-files)
  1. [References](#references)
  1. [Objects](#objects)

## Introduction

  <a name="introduction"></a>
  - This style guide is an accumulation of coding standards from different sources (e.g. AirBnB, Google, etc.) as well as my own inputs based on my experiences with JavaScript and React.
  - Optional formatting choices made in examples must not be enforced as rules.

  > **NOTE:** this style guide is following the [ES6 standard](https://github.com/lukehoban/es6features)

## Source Files

  <a name="source-files"></a>
  - **Files**
    - File names must be all lowercase and may include `_` underscores or `-` dashes, but no additional punctuation. Follow the convention that your project uses. File name's extension must be `.js`.
  - **Special Characters**
    - *Whitespace* - Tab characters are **not** used for indentation.

## References

  <a name="references--prefer-const"></a>
  - Use `const` for all of your references; avoid using `var`.

    > Why? This ensures that you can’t reassign your references, which can lead to bugs and difficult to comprehend code.

    ```javascript
    // avoid
    var a = 1;
    var b = 2;

    // prefer
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a>
  - If you must reassign references, use `let` instead of `var`.

    > Why? `let` is block-scoped rather than function-scoped like `var`.

    ```javascript
    // avoid
    var name = 'John Doe';

    if (true) {
      name = 'Juan Dela Cruz';
    }

    // prefer
    let name = 'John Doe';

    if (true) {
      name = 'Juan Dela Cruz';
    }
    ```

## Objects

  <a name="objects--no-new"></a>
  - Use the literal syntax for object creation.

    ```javascript
    // avoid
    const obj = new Object();

    // prefer
    const obj = {};
    ```

  <a name="es6-computed-properties"></a>
  - Use computed property names when creating objects with dynamic property names.

    > Why? They allow you to define all the properties of an object in one place.

    ```javascript
    function getKey(key) {
      return `${key}`;
    }

    // avoid
    const obj = {
      id: 1,
      name: 'Chocolate',
    };

    obj[getKey('enabled')] = true;

    // prefer
    const obj = {
      id: 1,
      name: 'Chocolate',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a>
  - Use object method shorthand.

    ```javascript
    // avoid
    const obj = {
      ctr: 1,

      addValue: function (value) {
        return obj.ctr + value;
      },
    };

    // prefer
    const obj = {
      ctr: 1,

      addValue(value) {
        return obj.ctr + value;
      },
    };
    ```

  <a name="es6-object-concise"></a>
  - Use property value shorthand.

    > Why? It is shorter to write and descriptive.

    ```javascript
    const name = 'Juan Dela Cruz';

    // avoid
    const obj = {
      name: name,
    };

    // prefer
    const obj = {
      name,
    };
    ```

  <a name="objects--grouped-shorthand"></a>
  - Group your shorthand properties at the beginning of your object declaration.

    > Why? It’s easier to tell which properties are using the shorthand.

    ```javascript
    const name = 'Juan Dela Cruz';
    const address = 'Metro Manila';

    // avoid
    const obj = {
      age: 24,
      gender: 'Male',
      name,
      yearsOfExperience: 4,
      address
    };

    // prefer
    const obj = {
      name,
      address
      age: 24,
      gender: 'Male',
      yearsOfExperience: 4,
    };
    ```

  <a name="objects--quoted-props"></a>
  - Only quote properties that are invalid identifiers.

    > Why? In general we consider it subjectively easier to read. It improves syntax highlighting, and is also more easily optimized by many JS engines.

    ```javascript
    // avoid
    const obj = {
      'foo': 1,
      'bar': 2,
      'invalid-id': 3
    };

    // prefer
    const obj = {
      foo: 1,
      bar: 2,
      'invalid-id': 3
    };
    ```

  <a name="objects--prototype-builtins"></a>
  - Do not call `Object.prototype` methods directly, such as `hasOwnProperty`, `propertyIsEnumerable`, and `isPrototypeOf`.

    > Why? These methods may be shadowed by properties on the object in question - consider `{ hasOwnProperty: false }` - or, the object may be a null object (`Object.create(null)`).

    ```javascript
    // avoid
    console.log(object.hasOwnProperty(key));

    // prefer
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // or
    const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
    console.log(has.call(object, key));
    ```

  <a name="objects--rest-spread"></a>
  - Prefer the object spread operator over [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) to shallow-copy objects. Use the object rest operator to get a new object with certain properties omitted.

    ```javascript
    // never do this
    const orig = { a: 1, b: 2 };
    const copy = Object.assign(orig, { c: 3 }); // this mutates the `orig` object
    delete copy.a; // so does this

    // avoid
    const orig = { a: 1, b: 2 };
    const copy = Object.assign({}, orig, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // prefer
    const orig = { a: 1, b: 2 };
    const copy = { ...orig, c: 3 }; // copy => { a: 1, b: 2, c: 3 }
    ```

## Arrays

  <a name="arrays--literals"></a>
  - Use the literal syntax for array creation.

    ```javascript
    // avoid
    const things = new Array();

    // prefer
    const things = [];
    ```

  <a name="arrays--push"></a>
  - Use [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) instead of direct assignment to add items to an array.

    ```javascript
    const things = [];

    // avoid
    things[things.length] = 'Chocolate';

    // prefer
    things.push('Chocolate');
    ```

  <a name="es6-array-spreads"></a>
  - Use array spreads `...` to copy arrays.

    ```javascript
    // avoid
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
    }

    // prefer
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a>
  - To convert an iterable object to an array, use spreads `...` instead of [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

    ```javascript
    const bar = document.querySelectorAll('.foo');

    // this is good but...
    const things = Array.from(bar);

    // prefer this
    const things = [...bar];
    ```

  <a name="arrays--from-array-like"></a>
  - Use [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) for converting an array-like object to an array.

    ```javascript
    const arrayLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

    // avoid
    const things = Array.prototype.slice.call(arrayLike);

    // prefer
    const things = Array.from(arrayLike);
    ```

  <a name="arrays--mapping"></a>
  - Use [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) instead of spread `...` for mapping over iterables, because it avoids creating an intermediate array.

    ```javascript
    // avoid
    const things = [...foo].map(bar);

    // prefer
    const things = Array.from(foo, bar);
    ```

  <a name="arrays--callback-return"></a>
  - Use return statements in array method callbacks. It’s ok to omit the return if the function body consists of a single statement returning an expression without side effects, following [Arrow Implicit Return](#arrows--implicit-return).

    ```javascript
    // prefer
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // prefer
    [1, 2, 3].map(x => x + 1);

    // avoid - no returned value means `acc` becomes undefined after the first iteration
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      acc[index] = flatten;
    });

    // prefer
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      acc[index] = flatten;
      return flatten;
    });

    // avoid
    inbox.filter((msg) => {
      const { subject, author } = msg;
      
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // prefer
    inbox.filter((msg) => {
      const { subject, author } = msg;

      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

  <a name="arrays--bracket-newline"></a>
  - Use line breaks after open and before close array brackets if an array has multiple lines

    ```javascript
    // bad
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    // bad
    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }];

    // bad
    const arr = [
      1, 2,
    ];

    // good
    const arr = [[0, 1], [2, 3], [4, 5]];

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    const numberInArray = [
      1,
      2,
    ];
    ```



Work-in-progress...