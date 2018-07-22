# JavaScript Style Guide;

A collection of JavaScript / React / React Native style guides

> **Note**: this style guide is an accumulation of coding standards from different sources (e.g. AirBnB, Google, etc.) as well as my own inputs based on my experiences with JavaScript and React.
> Another note: this style guide is following the [ES6 standard](https://github.com/lukehoban/es6features)

Here's the one for [React](https://github.com/auxcalibur/javascript/react).

## Table of Contents

  1. [References](#references)
  1. [Objects](#objects)

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