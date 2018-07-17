# React Redux Testing Cheatsheet

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
import renderer from 'react-test-renderer'; // for jest snapshot
import toJson from 'enzyme-to-json'; // for enzyme snapshot
```

# Table of Contents
- [Events](https://github.com/arjunu/react-redux-testing-cheatsheet#events)
- [DOM](https://github.com/arjunu/react-redux-testing-cheatsheet#dom)
- [Mocks](https://github.com/arjunu/react-redux-testing-cheatsheet#mocks)
- [Props](https://github.com/arjunu/react-redux-testing-cheatsheet#props)
- [Reducer](https://github.com/arjunu/react-redux-testing-cheatsheet#reducer)
- [Snapshot](https://github.com/arjunu/react-redux-testing-cheatsheet#snapshot)
- [State](https://github.com/arjunu/react-redux-testing-cheatsheet#state)
- [Timers](https://github.com/arjunu/react-redux-testing-cheatsheet#timers)

# Events

### Simulate click event

```javascript
const wrapper = shallow(<SomeComponent />);

//mock event if event is accessed
const event = { preventDefault: jest.fn() };
wrapper.find('button').simulate('click', event);
expect(event.preventDefault.mock.calls.length).toBe(1);
```
### Simulate on change event

```javascript
const wrapper = shallow(<SomeComponent />);

const event = { target: { value: 'user123' } };
wrapper.find(`input[name='username']`).simulate('change', event);
```

# DOM

### Check for an element/component

```javascript
const wrapper = shallow(<SomeComponent />);
expect(wrapper.find('ChildElementOrComponent').length).toBe(1);

//attribute selector
expect(wrapper.find(`input[type='submit']`).length).toBe(1);
```

### Check for focus

```javascript
const wrapper = mount(<SomeComponent />);
const element = wrapper.instance().componentInstanceVariableHoldingTheRef;
spyOn(element, 'focus');
// call code that triggers focus
// ...
expect(element.focus).toHaveBeenCalledTimes(1);
```
# Mocks

### Mock named import

```javascript
// foosModule.js
export const foo = () => {...}

// fooConsumerModule.js
import { foo } from './foosModule.js';
export const fooConsumer = () => foo();

// test.js 
import fooConsumer from './fooConsumerModule.js';
// import and mock the implementation
const module = require('./foosModule.js');
module.foo = () => 42;
expect(fooConsumer()).toEqual(42);
```

### Mock module

```javascript
jest.mock('./foo');
const foo = require('./foo');
foo.mockImplementation(() => 42);
```

# Props

### Check if prop function was called

```javascript
//mock the function
const propFunction = jest.fn();
const wrapper = shallow(<SomeComponent propFunction={propFunction} />);

//propFunction was called once
expect(propFunction.mock.calls.length).toBe(1);

//propFunction was called with argument 'arg'
expect(propFunction.mock.calls[0][0]).toEqual(arg);
```
### Update prop

```javascript
wrapper.setProps({ name: 'bar' });
```

http://airbnb.io/enzyme/docs/api/ShallowWrapper/setProps.html

### Call component function

```javascript
const wrapper = shallow(<SomeComponent />);
wrapper.instance().componentFunction();
```

# Reducer

### Test reducer (action effect on store)

```javascript
const state = { ... };
const expectedState = { ... };
const payload = { ... };
const action = actionCreator(payload);
expect(reducer(state, action).toEqual(expectedState);
```
# Snapshot

### Jest snapshot (shallow)

```javascript
const wrapper = shallow(<Component {...props}></Component>);
expect(toJson(wrapper)).toMatchSnapshot();
```

### Deep Snapshot 

```javascript
const tree = renderer.create(<Component {...props}/>).toJSON();
expect(tree).toMatchSnapshot();
```

# State

### Check state 

```javascript
const wrapper = shallow(<SomeComponent />);
expect(wrapper.state().fruit).toEqual('apple');
```

### Check asynchronously changed state 

```javascript
test('', done => {
  const wrapper = shallow(<SomeComponent />);
  // do something that changes state in an async method
  process.nextTick(() => {
    wrapper.update();
    expect(wrapper.state().fruit).toEqual('apple');
    done();
  });
});
```

### Set state

```javascript
const wrapper = shallow(<SomeComponent />);
wrapper.instance().setState({fruit: 'orange'}));
```

# Timers

### Post timeout execution 

```javascript
setInterval(() => this.setState({ flag: true }), 100);

jest.useFakeTimers();
test('', () => {
  jest.runTimersToTime(100);
  wrapper.update();
  expect(wrapper.state().flag).toEqual(true);
});
```
