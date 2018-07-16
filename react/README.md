# <React Style Guide />

> **Note**: this style guide is an accumulation of coding standards from different sources as well as my own inputs based on my experiences with React. This style guide can also be used in React Native for most of the part. 

Here's the one for [JavaScript](https://github.com/auxcalibur/javascript).

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Importing](#importing)
  1. [Stateful vs Stateless](#stateful-vs-stateless)

## Basic Rules

  - Only include one React component per file.
    - However, multiple Stateless or Pure Components are allowed per file.
  - Always use JSX syntax.
  - Do not use `React.createElement` unless you're initializing the app from a file that is not JSX.
  - Do not use `createClass`. Prefer `class MyComponent extends Component` or normal functions: `function MyComponent(props) {}` (not arrow functions).
  - Always end each line with `;` if applicable for better code readability.

## Importing

  - Add `{ Component }` when doing `import React, { Component } from 'react';`.

  ```jsx
  import React, { Component } from 'react';
  ```

  - Your imported files should be ordered like:
    - `import React, { Component } from 'react';`
    - External modules (e.g. `import moment from 'moment';`)
    - Your components (e.g. `import MyComponent from '../MyComponent';`)
    - Actions
    - Utilities (e.g. validations, formatter, static dropdown lists)

  ```jsx
  import React, { Component } from 'react';
  import moment from 'moment';
  import styled from 'styled-components';

  import MyComponent from '../MyComponent';
  import OtherComponent from '../../OtherComponent';

  import { login } from '../../../actions/authentication';
  import * from '../../../actions/customAction';

  import { required, email, number } from '../../../utils/validations';
  ```

## Stateful vs Stateless

  - If you have internal state and/or refs, prefer `class Name extends Component` over normal functions (not arrow functions).

  ```jsx
  // bad
  const MyComponent = createClass({
    // ...
    render() {
      return <div>{this.state.hello}</div>;
    }
  });

  // good
  class MyComponent extends Component {
    // ...
    render() {
      return <div>{this.state.hello}</div>;
    }
  }
  ```

  - And if you don't have state or refs, prefer normal functions (not arrow functions) over classes:

  ```jsx
  // bad
  class Listing extends React.Component {
    render() {
      return <div>{this.props.hello}</div>;
    }
  }

  // bad (relying on function name inference is discouraged)
  const Listing = ({ hello }) => (
    <div>{hello}</div>
  );

  // good
  function Listing({ hello }) {
    return <div>{hello}</div>;
  }
  ```


... work-in-progress