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
- [Reducer](https://github.com/arjunu/react-redux-testing-cheatsheet#reducer)
- [State](https://github.com/arjunu/react-redux-testing-cheatsheet#state)

# Events

## Simulate click event

```javascript
const wrapper = shallow(<SomeComponent />);

//mock event if event is accessed
const event = { preventDefault: jest.fn() };
wrapper.find('button').simulate('click', event);
expect(event.preventDefault.mock.calls.length).toBe(1);
```
# Simulate on change event

```javascript
const wrapper = shallow(<SomeComponent />);

const event = { target: { value: 'user123' } };
wrapper.find(`input[name='username']`).simulate('change', event);
```

# DOM

## Check for an element/component

```javascript
const wrapper = shallow(<SomeComponent />);
expect(wrapper.find('ChildElementOrComponent').length).toBe(1);

//attribute selector
expect(wrapper.find(`input[type='submit']`).length).toBe(1);
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

# Reducer

## Test reducer (action effect on store)

```javascript
const state = { ... };
const expectedState = { ... };
const payload = { ... };
const action = actionCreator(payload);
expect(reducer(state, action).toEqual(expectedState);
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
