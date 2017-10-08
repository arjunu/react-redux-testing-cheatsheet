# React Redux Testing Cheatsheet (not complete)

This cheatsheet uses the following stack
1. [React JS](https://github.com/facebook/react)
2. [Redux](https://github.com/reactjs/redux)
3. [Immutable JS](https://github.com/facebook/immutable-js)
4. [Jest](https://github.com/facebook/jest)
5. [Enzyme](https://github.com/airbnb/enzyme) 

# Imports

```javascript
import React from 'react';
import { shallow } from 'enzyme';
```

# Table of Contents
- [Events](https://github.com/arjunu/react-redux-testing-cheatsheet#events)
- [DOM](https://github.com/arjunu/react-redux-testing-cheatsheet#dom)
- [Props](https://github.com/arjunu/react-redux-testing-cheatsheet#props)

# Events

## Simulate click event


# DOM

## Check for an element/component

```javascript
const wrapper = shallow(<SomeComponent />);
expect(wrapper.find('ChildElementOrComponent').length).toBe(1);

//attribute selector
expect(wrapper.find(`input[type='submit'`).length).toBe(1);
```

# Props

## Check if prop function was called

```javascript
//mock the function
const propFunction = jest.fn();
const wrapper = shallow(<SomeComponent propFunction={propFunction} />);

//propFunction was called once
expect(propFunction.mock.calls.length).toBe(1);

//propFunction was called with argument 'arg'
expect(propFunction.mock.calls[0][0]).toEqual(arg);
```
## Call component function

```javascript
const wrapper = shallow(<SomeComponent />);
wrapper.instance().componentFunction();
```

# State

## Check state 

```javascript
const wrapper = shallow(<SomeComponent />);
expect(wrapper.state().fruit).toEqual('apple');
```

## Set state

```javascript
const wrapper = shallow(<SomeComponent />);
wrapper.instance().setState({fruit: 'orange'}));
```
