# JavaScript Style Guide;

A collection of JavaScript / React / React Native style guides

Here's the one for [React](https://github.com/auxcalibur/javascript/react).

## Table of Contents

  1. [Introduction](#introduction)
  1. [Source Files](#source-files)
  1. [References](#references)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [String](#string)
  1. [Functions](#functions)

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

## Destructuring

  <a name="destructuring--object"></a>
  - Use object destructuring when accessing and using multiple properties of an object.

    > Why? Destructuring saves you from creating temporary references for those properties.

    ```javascript
    // avoid
    function getData(data) {
      const name = data.name;
      const address = data.address;

      return `${name} lives in ${address}`;
    }

    // prefer
    function getData(data) {
      const { name, address } = data;
      return `${name} lives in ${address}`;
    }

    // prefer more
    function getData({ name, address }) {
      return `${name} lives in ${address}`;
    }
    ```

  <a name="destructuring--array"></a>
  - Use array destructuring

    ```javascript
    const arr = [1, 2, 3, 4];

    // avoid
    const one = arr[0];
    const two = arr[1];

    // prefer
    const [one, two] = arr;
    ```

  <a name="destructuring--object-over-array"></a>
  - Use object destructuring for multiple return values, not array destructuring.

    > Why? You can add new properties over time or change the order of things without breaking call sites.

    ```javascript
    // avoid
    function processInput(input) {
      // ...
      return [one, two, three, four];
    }

    // the caller needs to think about the order of return data
    const [one, __, three] = processInput(input);

    // prefer
    function processInput(input) {
      // ...
      return { one, two, three, four };
    }

    // the caller selects only the data they need
    const { one, three } = processInput(input);
    ```

## Strings

  <a name="strings--quotes"></a>
  - Use single quotes `''` for strings.

    ```javascript
    // avoid
    const name = "Juan Dela Cruz";

    // avoid - template literals should contain interpolation or newlines
    const name = `Juan Dela Cruz`;

    // prefer
    const name = 'Juan Dela Cruz';
    ```

  <a name="strings--line-length"></a>
  - Strings that cause the line to go over 100 characters should not be written across multiple lines using string concatenation.

    > Why? Broken strings are painful to work with and make code less searchable.

    ```javascript
    // avoid
    const longString = 'There is no one who loves or pursues or desires to obtain pain of itself, \
    because it is pain, but because occasionally circumstances occur in which toil and pain \
    can procure him some great pleasure.';

    // avoid
    const longString = 'There is no one who loves or pursues or desires to obtain pain of itself, ' +
      'because it is pain, but because occasionally circumstances occur in which toil and pain ' +
      'can procure him some great pleasure.';

    // prefer
    const longString = 'There is no one who loves or pursues or desires to obtain pain of itself, because it is pain, but because occasionally circumstances occur in which toil and pain can procure him some great pleasure.';
    ```

  <a name="es6-template-literals"></a>
  - When programmatically building up strings, use template strings instead of concatenation.

    > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

    ```javascript
    // avoid
    function doIt(stuff) {
      return 'Just do ' + stuff + '!';
    }

    // avoid
    function doIt(stuff) {
      return ['Just do ', stuff, '!'].join();
    }

    // avoid
    function doIt(stuff) {
      return `Just do ${ stuff }!`;
    }

    // prefer
    function doIt(stuff) {
      return `Just do ${stuff}!`;
    }
    ```

  <a name="strings--eval">
  - Never use `eval()` on a string, it opens too many vulnerabilities.

  <a name="strings--escaping"></a>
  - Do not unnecessarily escape characters in strings.

    > Why? Backslashes harm readability, thus they should only be present when necessary.

    ```javascript
    // avoid
    const string = '\'this\' \i\s \"quoted\"';

    // prefer
    const string = '\'this\' is "quoted"';
    const string = `This is the '${stuff}'`;
    ```

## Functions

  <a name="functions--declarations"></a>
  - Use named function expressions instead of function declarations.

    > Why? Function declarations are hoisted, which means that it’s easy - too easy - to reference the function before it is defined in the file. This harms readability and maintainability. If you find that a function’s definition is large or complex enough that it is interfering with understanding the rest of the file, then perhaps it’s time to extract it to its own module! Don’t forget to explicitly name the expression, regardless of whether or not the name is inferred from the containing variable (which is often the case in modern browsers or when using compilers such as Babel).

    ```javascript
    // avoid
    function foo() {
      // ...
    }

    // avoid
    const bar = function () {
      // ...
    };

    // prefer
    // lexical name distinguished from the variable-referenced invocation(s)
    const short = function longUniqueMoreDescriptiveFunctionName() {
      // ...
    };
    ```

  <a name="functions--note-on-blocks"></a>
  - **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement.

    ```javascript
    // avoid
    if (truth) {
      function test() {
        console.log('Nope.');
      }
    }

    // prefer
    let test;
    if (truth) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  <a name="functions--arguments-shadow"></a>
  - Never name a parameter `arguments`. This will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // avoid
    function foo(name, options, arguments) {
      // ...
    }

    // prefer
    function foo(name, options, args) {
      // ...
    }
    ```

  <a name="es6-rest"></a>
  - Never use `arguments`, opt to use rest syntax `...` instead.

    > Why? `...` is explicit about which arguments you want pulled. Plus, rest arguments are a real Array, and not merely Array-like like `arguments`.

    ```javascript
    // avoid
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // prefer
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a>
  - Use default parameter syntax rather than mutating function arguments.

    ```javascript
    // seriously avoid
    function sampleFunc(opts) {
      opts = opts || {};
      // ...
    }

    // avoid
    function sampleFunc(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // prefer
    function sampleFunc(opts = {}) {
      // ...
    }
    ```

  <a name="functions--default-side-effects"></a>
  - Avoid side effects with default parameters.

    > Why? They are confusing to reason about.

    ```javascript
    var b = 1;
    // avoid
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  <a name="functions--defaults-last"></a>
  - Always put default parameters last.

    ```javascript
    // avoid
    function sampleFunc(opts = {}, name) {
      // ...
    }

    // prefer
    function sampleFunc(name, opts = {}) {
      // ...
    }
    ```

  <a name="functions--constructor"></a>
  - Never use the Function constructor to create a new function.

    > Why? Creating a function in this way evaluates a string similarly to `eval()`, which opens vulnerabilities.

    ```javascript
    // avoid
    var add = new Function('a', 'b', 'return a + b');

    // avoid
    var subtract = Function('a', 'b', 'return a - b');
    ```

  <a name="functions--signature-spacing"></a>
  - Spacing in a function signature.

    > Why? Consistency is good, and you shouldn’t have to add or remove a space when adding or removing a name.

    ```javascript
    // avoid
    const w = function(){};
    const t = function (){};
    const f = function() {};

    // prefer
    const r = function () {};
    const o = function f() {};
    function l() {};
    ```

  <a name="functions--mutate-params"></a>
  - Never mutate parameters.

    > Why? Manipulating objects passed in as parameters can cause unwanted variable side effects in the original caller.

    ```javascript
    // avoid
    function sampleFunc(obj) {
      obj.key = 1;
    }

    // prefer
    function sampleFunc(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
    }
    ```

  <a name="functions--reassign-params"></a>
  - Never reassign parameters.

    > Why? Reassigning parameters can lead to unexpected behavior, especially when accessing the `arguments` object. It can also cause optimization issues, especially in V8.

    ```javascript
    // avoid
    function sampleFunc1(a) {
      a = 1;
      // ...
    }

    function sampleFunc2(a) {
      if (!a) { a = 1; }
      // ...
    }

    // prefer
    function sampleFunc3(a) {
      const b = a || 1;
      // ...
    }

    function sampleFunc4(a = 1) {
      // ...
    }   
    ```

  <a name="functions--spread-vs-apply"></a>
  - Prefer the use of the spread operator `...` to call variadic functions.

    > Why? It’s cleaner, you don’t need to supply a context, and you can not easily compose `new` with `apply`.

    ```javascript
    // avoid
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // prefer
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // avoid
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

    // prefer
    new Date(...[2016, 8, 5]);
    ```

  <a name="functions--signature-invocation-indentation"></a>
  - Functions with multiline signatures, or invocations, should be indented just like every other multiline list in this guide: with each item on a line by itself, with a trailing comma on the last item.

    ```javascript
    // avoid
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // prefer
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // avoid
    console.log(foo,
      bar,
      baz);

    // prefer
    console.log(
      foo,
      bar,
      baz,
    );
    ```

Work-in-progress...