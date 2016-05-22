---
layout: post
title: think in react
---

## main point
* V of MVC
* virtual DOM
* one-way data flow, from owner to owned components
* composeable components
* declarative

## props and state of component
* react componets are just like functions, take in `props` and `state` and render html.
* props are pssed form parent and are "owned" by the parent, props are immutable.
* state is private to the component and can be changed by calling `this.setState()` to rerender component.
* Try to keep as many of components as possible stateless, and have a stateful component above them in the hierarchy that passes its state to its children via props.

## what and where of state
* components are just state machines.
* what is 
	1. state should contain data that a component's event handlers may change to trigger a UI update.
	2. identify the minimal but complete repesentation of state, *Dont't repeat Yourself*.
* what isn't
	1. computed data
	2. react components
	3. duplicated data from props
* where
	1. state live in common owner of components that need state


## data flow
* from owner to owned
* inverse by event handler