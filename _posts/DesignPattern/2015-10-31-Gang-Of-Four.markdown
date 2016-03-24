---
layout: post
title: Gang of four in brief
---
# Creational design pattern #

# Structural design pattern #
## Adpter Pattern ##

Allow the interface of an existing class to be used from another interface. It is often used to make existing class work with others without modifying their source code.

## Facade Pattern ##

It is often used when a system is very complex or diffcult to understand because the system has a large number of interdependent classes or its source code is unavailable. This pattern hides the complexities of the larger system and provides a simpler interface to the client. It typically involes a single wrapper class which contains a set if members required by client. 

## Birdge Pattern ##

It is meant to "decouple an abstraction from its implementation so that the two can vary independently" The bridege uses encapsulation, aggregation, and can use inheritance to spearate repsosibilities into different classes.

The class itself can be thought of as the implementation and what the class do as the abstraction.

The birdge pattern can also be thought of as two layers of abatraction.

## Proxy Pattern ##

An object representing another object.Allow you to provide an interface to other objects by creating a wrapper class as proxy.The wrapper class can add additional functionality to the object .The proxy could interface to anything: a network connection, a large object ub memory, a file, or some other resource that is expensive or impossible to duplicate.

# Behavioral design pattern #

## Mediator Pattern ##


