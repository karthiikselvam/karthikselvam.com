---
title: "Builder Design Pattern Practice Qestions"
description: "2023"
date: "2023-01-23"
tags:
- fundamentals
---

Exercise 1: Basic Builder Implementation
Objective: Implement a basic builder.
Steps:
Create a House class with attributes walls, roof, and windows.
Create a HouseBuilder class with methods to set the attributes of House.
Implement a build() method in HouseBuilder that returns a House instance.
Demonstrate building a House object using HouseBuilder.

Exercise 2: Fluent Builder Interface
Objective: Implement a builder with a fluent interface.
Steps:
Create a Car class with attributes engine, wheels, and color.
Create a CarBuilder class with methods to set the attributes of Car.
Implement the methods in CarBuilder to return the builder itself for method chaining.
Demonstrate building a Car object using the fluent interface.

Exercise 3: Director in Builder Pattern
Objective: Use a director to manage the construction process.
Steps:
Create a Computer class with attributes CPU, RAM, storage, and graphicsCard.
Create a ComputerBuilder class with methods to set the attributes of Computer.
Create a Director class with methods to construct different types of computers (e.g., gaming computer, office computer).
Demonstrate building different types of computers using the Director.

Exercise 4: Immutable Objects with Builder Pattern
Objective: Implement the builder pattern for creating immutable objects.
Steps:
Create an immutable Person class with attributes firstName, lastName, age, and address.
Create a PersonBuilder class to set the attributes of Person.
Ensure the Person class has private final fields and no setters.
Demonstrate creating a Person object using PersonBuilder.

Exercise 5: Nested Builders
Objective: Implement nested builders for complex objects.
Steps:
Create a House class with an inner class Garden and attributes walls, roof, and garden.
Create a HouseBuilder class with methods to set the attributes of House.
Create a nested GardenBuilder class inside HouseBuilder to build the Garden object.
Demonstrate building a House object with a nested Garden using the builders.

Exercise 6: Copy Builder
Objective: Implement a builder that can copy attributes from an existing object.
Steps:
Create a Book class with attributes title, author, pages, and publisher.
Create a BookBuilder class with methods to set the attributes of Book.
Implement a method from(Book book) in BookBuilder that copies the attributes from an existing Book object.
Demonstrate creating a new Book object by copying attributes from an existing Book using BookBuilder.

Exercise 7: Builder Pattern with Inheritance
Objective: Implement the builder pattern with inheritance.
Steps:
Create a base class Employee with attributes name and employeeId.
Create a derived class Manager with an additional attribute department.
Create a builder EmployeeBuilder to build Employee objects.
Create a builder ManagerBuilder that extends EmployeeBuilder to build Manager objects.
Demonstrate building Employee and Manager objects using their respective builders.