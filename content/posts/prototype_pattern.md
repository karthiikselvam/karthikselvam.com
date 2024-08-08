---
title: "Prototype Design Pattern Practice Qestions"
description: "2023"
date: "2023-01-23"
tags:
- fundamentals
---

Exercise 1: Basic Prototype Implementation
Objective: Implement a basic prototype.
Steps:
Create an interface Prototype with a method Prototype clone().
Implement a class ConcretePrototype that implements Prototype and includes some fields.
Demonstrate creating and cloning instances of ConcretePrototype.

Exercise 2: Deep vs. Shallow Copy
Objective: Understand deep and shallow copying.
Steps:
Create a class Address with fields street and city.
Modify ConcretePrototype to include an Address field.
Implement both shallow and deep copy methods in ConcretePrototype.
Demonstrate the differences between shallow and deep copying.

Exercise 3: Registry for Prototypes
Objective: Use a prototype registry.
Steps:
Create a prototype registry class that maintains a map of prototype names to prototype instances.
Implement methods to add prototypes to the registry and to create new instances by cloning prototypes.
Demonstrate using the registry to create new instances of different prototypes.

Exercise 4: Prototype with Composition
Objective: Implement the Prototype Pattern with composition.
Steps:
Create a class Document with fields title and a list of Page objects.
Implement Page as a class with a content field.
Implement cloning for both Document and Page classes.
Demonstrate cloning a Document with multiple Page objects.

Exercise 5: Prototype Pattern in a Game
Objective: Apply the Prototype Pattern in a real-world scenario.
Steps:
Create a class Enemy with fields type, health, and attackPower.
Implement the clone method in Enemy.
Create a registry for different types of enemies.
Demonstrate cloning different types of enemies from the registry.

Exercise 6: Prototype with Immutable Objects
Objective: Implement cloning for immutable objects.
Steps:
Create an immutable class Person with fields name and age.
Implement a builder for Person to facilitate creating instances.
Demonstrate creating and cloning immutable Person instances using the builder.

Exercise 7: Prototype with Polymorphism
Objective: Use the Prototype Pattern with polymorphism.
Steps:
Create an abstract class Shape with a method clone().
Implement concrete classes Circle and Square that extend Shape.
Create a registry of Shape objects.
Demonstrate cloning different shapes from the registry.