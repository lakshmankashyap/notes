# React & Redux

An app needs to remember things in order to work : that's why we need **state management**

**3 principles**

<img src="3_principles.png" width="500px"/>

**Redux's features**

<img src="features_redux.png" width="500px"/>

## State

State management becomes a problem when the app gets more and more complicated.

<img src="react_big_state.png" width="500px"/>

Redux concept : components have just **props**, there is one massive object which is called the **store** and contain the state.

<img src="redux_state.png" width="500px"/>

**Flux pattern**

<img src="flux_pattern.png" width="500px"/>

The big state in React above becomes this state :

<img src="redux_diagram.png" width="500px"/>

## Actions

## Reducers

## Store and provider

## Connect

There are two kind of components : **containers** and **components**

Containers are smart components. They know about the state of the app.

## Middlewares

A middleware listens for actions. It's a tunnel that actions go through and depending on the middleware, it can modify the action or trigger another one before hitting the reducer.

