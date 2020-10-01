# Component Mounting and Unmounting

## Overview of mounting and unmounting

We'll describe what happens in the mounting and unmounting phases of a React
component's lifecycle.

## Objectives

1. Describe the `static getDerivedStateFromProps` and `componentDidMount` 
   lifecycle methods in the mounting phase
2. Describe the `componentWillUnmount` lifecycle method in the unmounting phase


## Setup and Cleanup

A React component's lifecycle contains distinct phases for creation and
deletion. In coding terms, these are called **mounting** and **unmounting**. You
can also think of them as "setup" and "cleanup".

If you were going to have a picnic, just before you lay down the picnic blanket
you'd make sure the ground was level and clean. Also, after you're done, and
before you clean up your picnic blanket, you'd make sure you've taken all your
belongings off it and cleared up any garbage left on the grass so people after
you can easily use the same spot.

That's very similar to what happens with React components. The browser window is
almost like a great big field that loads the components that can be used. And
when they leave, it's only polite of them to clean up the space they were using —
so that other components can reuse the same space without any annoyances due to
things left behind.

## Pre-Mounting

#### `constructor`

Technically the **`constructor`** is the first function called upon
instantiating **any** class in JS, not just React Components. That being said,
the **`constructor`** has an important role in the life of a component, as it
acts as a perfect place to set the initial state of a component. Within the
constructor, one can initialize state like so:

```javascript
class App extends React.Component {

  constructor() {
    super()
    this.state = {
      key: "value"
    }
  }
  
}
```

In ES7, it is possible to initialize state by simply doing the following inside
of your component. If you see either the syntax above or below, keep in mind
that they accomplish the same task at the same time during the component
lifecycle.

```javascript
class App extends React.Component {
  state = {
    key: "value"
  }
}
```

Note: Bear in mind that we call `super` so that we can execute the `constructor`
function that is inherited from React.Component while adding our own
functionality.

It is possible to use the `constructor` to set an initial state that is
dependent upon props like so:

```javascript
constructor(props) {
  super(props);
  this.state = {
    color: props.initialColor
  };
}
//source: https://reactjs.org/docs/react-component.html#constructor
```

Note that in contrast to the previous example, we take `props` as an argument to
the constructor. This is because we are making use of the props to set an
initial state - if we aren't using props to do this, then we need not include
`props` as an argument to the constructor.

## Mounting

In the mounting (or DOM creation, or "setup") phase, we have access to two
**lifecycle methods**: **`static getDerivedStateFromProps`**, and **`componentDidMount`**.


#### `static getDerivedStateFromProps`

The **`static getDerivedStateFromProps`** is called every time a component is
rendered, including the first time, during mounting. From the [React blog][blog]:

>> "getDerivedStateFromProps exists for only one purpose. It enables a component
to update its internal state as the result of changes in props... We did not
provide many examples, because as a general rule, derived state should be used
sparingly."

In the context of mounting, if your intention is to set an initial state for
your component, it is preferable for you to do this in the `constructor` as
shown above.

If your intention is to set state using data from an async request, it is
preferable that you do this in `componentDidMount`, as we will see below.

In picnic terms, `getDerivedStateFromProp` is the moment when you arrive at the
field with your picnic blanket and you make sure the spot you've chosen is nice
and level. You might find something to clean up before you lay your blanket
down. However, you're already here. Doing anything else now is just delaying you
from spreading out the blanket and enjoying your lunch.

### `componentDidMount`

The **`componentDidMount`** is also only called once, but immediately *after*
the first `render()` method has taken place. That means that the HTML for the
React component has been rendered into the DOM and can be accessed if necessary.
This method is used to perform any DOM manipulation or data-fetching that the
component might need.

If you were at a picnic, this is the moment just after you've laid out your
blanket. You would use this time to set up any things you want to be using
during your stay: lay out all your food and drinks, maybe take out a radio and
put some music on.

In React, this is where you would set up any long-running processes you want to
use in your component, for example fetching data. Suppose we were building a
weather app that fetches data on the current weather and displays it to the
user. We would want this data to update every 15 seconds without the user having
to refresh the page. `componentDidMount` to the rescue!

```javascript
componentDidMount() {
  this.interval = setInterval(this.fetchWeather, 15000);
}
```

## Unmounting

In the unmounting (or deletion, or "cleanup") phase, we have just one lifecycle
method to help us out: `componentWillUnmount`. The `componentWillUnmount` method
is the last function to be called immediately before the component is removed
from the DOM. It is generally used to perform clean-up for any DOM-elements or
timers created in **`componentDidMount`**.

At a picnic, `componentWillUnmount` corresponds to just before you pick up your
picnic blanket. You would need to clean up all the food and drinks you've set on
the blanket first or they'd spill everywhere! You'd also have to shut down your
radio. After that's all done you would be free to pick up your picnic blanket
and put it back in the bag safely. There is no need to make a sandwich right
now, we're already leaving.

For a React component, this is where you would clean up any of those long
running processes that you set up in `componentDidMount`. In the above data
fetching example, all we would have to do is clear the interval so that the
weather API would no longer get called every 15 seconds:

```javascript
componentWillUnmount() {
  clearInterval(this.interval);
}
```

## Summary

The mounting and unmounting steps are important for ensuring that the React
component gets set up and initialized nicely and that when it gets unmounted, it
leaves the space it occupied just as it was before: nice and tidy.

In the mounting step, we can set up any special requirements we may have for
that particular component: fetch some data, start counters etc. It is extremely
important to clean up all the things we set up in the unmounting stage in
`componentWillUnmount`, as not doing so may lead to some pretty nasty
consequences - even as bad as crashing your carefully crafted application!

#### Mounting lifecycle methods

Called once on initial render:

| Method            | current props and state | prevProps | prevState | nextProps |  nextState | Can call `this.setState` | Called when?               | Used for |                                                                                   
|:-------------------------:|:---------:|:---------:|:----------------------:|:-------------------------------------------------------:|:--------------------------------------------------------------------------------:|:---------:|:---------:|:----------------------:|
| `constructor` |     no    |     no    |     no    |     no    |     no    |     no    | once, just before `static getDerivedStateFromProps()` is called for the first time | Setting initial state                                             |
| `static getDerivedStateFromProps()` |     yes    |     no    |     no    |     no    |     no    |     yes    | right before the initial render and **all** re-renders | Not used often |
| `render()`           |     yes   |     no    |     no    |     no    |     no    |     no    | every time React updates and commits to the DOM | Writing JSX for components |
| `componentDidMount`  |     yes   |     no    |     no    |     no    |     no    |     yes   | once, just after mounting  | setting up side effects (e.g. creating new DOM elements or setting up asynchronous functions |


#### Unmounting lifecycle method

Called only once, just before the component is removed from the DOM:

| Method            | current props and state | prevProps | prevState | nextProps |  nextState | Can call `this.setState` | Called when?               | Used for                                                                                    |
|:--------------------:|:---------:|:---------:|:----------------------:|:---------------------------------------------------:|:-------------------------------------------------------:|:---------:|:---------:|:----------------------:|
| `componentWillUnmount` | yes | no | no | no | no | n/a | once, just before component is removed from the DOM | destroying any side effects set up in componentDidMount |

## Resources

- [React: Component Specs and Lifecycle](https://facebook.github.io/react/docs/component-specs.html)

[blog]: https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#when-to-use-derived-state
