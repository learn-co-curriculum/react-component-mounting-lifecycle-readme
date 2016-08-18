# React: Component Mounting and Unmounting

## Objectives

1. Describe what happens in the mounting phase of a React component's lifecycle
2. Describe what happens in the unmounting phase of a React component's
   lifecycle

## Overview

This lesson is a deep dive into the mounting and unmounting phases; we'll deal
with rendering separately, since it works somewhat differently (in that it
happens over and over again).

It's a good idea to go over when is good to do what (e.g., make AJAX calls for
initial data in `componentDidMount()`, clear timers in `componentWillUnmount()`,
etc.)

## Suggested metaphors and narratives
A cooking narrative would be great here: the stuff in the `componentDidMount()` is the mise-en-place/prep stuff, while the `componentWillUnmount()` would serve as a metaphor for cleaning up when you're done (e.g. if you had an egg timer running, make sure it's off before we're done with the kitchen, or it'll go off when we're relaxing and not in the kitchen anymore?)

## Resources

- [React: Component Specs and Lifecycle](https://facebook.github.io/react/docs/component-specs.html)
