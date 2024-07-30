# 0x03. React component

# Creating and Using React Components: Advanced Guide

## Introduction

This guide will delve deeper into advanced React component concepts, including when to use class or function components, the lifecycle of class components, testing components, utilizing Jest spies, understanding higher-order components (HOCs), and optimizing performance by controlling which components to render.

## Table of Contents

1. [When to Use a Class or a Function to Create a Component](#when-to-use-a-class-or-a-function-to-create-a-component)
2. [The Lifecycle of a Class Component](#the-lifecycle-of-a-class-component)
3. [How to Test a Component](#how-to-test-a-component)
4. [Utilizing a Jest Spy to Verify Function Calls](#utilizing-a-jest-spy-to-verify-function-calls)
5. [What is a Higher-Order Component (HOC) and How to Use It](#what-is-a-higher-order-component-hoc-and-how-to-use-it)
6. [Optimizing Performance and Controlling Component Rendering](#optimizing-performance-and-controlling-component-rendering)

## When to Use a Class or a Function to Create a Component

### Function Components

- **Stateless**: Ideal for components that do not manage their own state.
- **Simplicity**: Easier to write, read, and test.
- **Performance**: Function components are generally faster and consume less memory.

### Class Components

- **Stateful**: Suitable for components that need to manage their own state or lifecycle methods.
- **Lifecycle Methods**: Provide access to lifecycle methods like `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`.

### Example

#### Function Component

```jsx
import React from 'react';

const Greeting = ({ name }) => {
    return <h1>Hello, {name}!</h1>;
};

export default Greeting;
```

#### Class Component

```jsx
import React, { Component } from 'react';

class Greeting extends Component {
    render() {
        return <h1>Hello, {this.props.name}!</h1>;
    }
}

export default Greeting;
```

## The Lifecycle of a Class Component

### Mounting

- **constructor()**: Called before the component is mounted.
- **static getDerivedStateFromProps()**: Syncs state with props.
- **render()**: Renders the component.
- **componentDidMount()**: Invoked after the component is mounted.

### Updating

- **static getDerivedStateFromProps()**: Syncs state with props.
- **shouldComponentUpdate()**: Determines if the component should re-render.
- **render()**: Renders the component.
- **getSnapshotBeforeUpdate()**: Captures current DOM information before updates.
- **componentDidUpdate()**: Invoked after the component updates.

### Unmounting

- **componentWillUnmount()**: Invoked before the component is unmounted and destroyed.

### Example

```jsx
import React, { Component } from 'react';

class LifecycleDemo extends Component {
    constructor(props) {
        super(props);
        this.state = { count: 0 };
    }

    static getDerivedStateFromProps(props, state) {
        return null;
    }

    componentDidMount() {
        console.log('Component did mount');
    }

    shouldComponentUpdate(nextProps, nextState) {
        return true;
    }

    getSnapshotBeforeUpdate(prevProps, prevState) {
        return null;
    }

    componentDidUpdate(prevProps, prevState, snapshot) {
        console.log('Component did update');
    }

    componentWillUnmount() {
        console.log('Component will unmount');
    }

    render() {
        return <div>Count: {this.state.count}</div>;
    }
}

export default LifecycleDemo;
```

## How to Test a Component

### Using Enzyme

Enzyme is a testing utility for React that makes it easier to test components.

### Installing Enzyme

```bash
npm install --save enzyme enzyme-adapter-react-16
```

### Setting Up Enzyme

Create a `setupTests.js` file to configure Enzyme.

```javascript
import { configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

configure({ adapter: new Adapter() });
```

### Example Test

```javascript
import React from 'react';
import { shallow } from 'enzyme';
import Greeting from './Greeting';

describe('Greeting Component', () => {
    it('renders without crashing', () => {
        const wrapper = shallow(<Greeting name="Alice" />);
        expect(wrapper.text()).toBe('Hello, Alice!');
    });
});
```

## Utilizing a Jest Spy to Verify Function Calls

### Using Jest Spy

Jest provides functionality to spy on function calls, which is useful for verifying that functions are being called correctly.

### Example

```javascript
import React from 'react';
import { shallow } from 'enzyme';
import Button from './Button';

describe('Button Component', () => {
    it('calls onClick handler when clicked', () => {
        const onClickSpy = jest.fn();
        const wrapper = shallow(<Button onClick={onClickSpy} />);
        wrapper.find('button').simulate('click');
        expect(onClickSpy).toHaveBeenCalled();
    });
});
```

## What is a Higher-Order Component (HOC) and How to Use It

### What is an HOC?

An HOC is a function that takes a component and returns a new component. It is used to share common functionality between components.

### Example

```jsx
import React from 'react';

const withLogging = (WrappedComponent) => {
    return class extends React.Component {
        componentDidMount() {
            console.log(`${WrappedComponent.name} mounted`);
        }

        render() {
            return <WrappedComponent {...this.props} />;
        }
    };
};

const MyComponent = () => <div>My Component</div>;

export default withLogging(MyComponent);
```

## Optimizing Performance and Controlling Component Rendering

### Using `shouldComponentUpdate`

This lifecycle method allows you to prevent unnecessary re-renders.

### Example

```jsx
import React, { Component } from 'react';

class OptimizedComponent extends Component {
    shouldComponentUpdate(nextProps, nextState) {
        return nextProps.value !== this.props.value;
    }

    render() {
        return <div>{this.props.value}</div>;
    }
}

export default OptimizedComponent;
```

### Using `React.memo`

`React.memo` is a higher-order component that prevents a functional component from re-rendering if its props haven't changed.

### Example

```jsx
import React from 'react';

const MyComponent = React.memo(({ value }) => {
    return <div>{value}</div>;
});

export default MyComponent;
```

### Using `useMemo` and `useCallback`

These hooks help optimize performance by memoizing expensive calculations and functions.

### Example

```jsx
import React, { useMemo, useCallback } from 'react';

const ExpensiveComponent = ({ data }) => {
    const memoizedData = useMemo(() => processData(data), [data]);
    const memoizedCallback = useCallback(() => handleData(data), [data]);

    return <div>{memoizedData}</div>;
};

export default ExpensiveComponent;
```

## Conclusion

This advanced guide covers key concepts for creating and using React components effectively. By understanding when to use class or function components, the lifecycle of class components, testing with Enzyme and Jest, higher-order components, and performance optimization techniques, you can build robust and efficient React applications. Happy coding!