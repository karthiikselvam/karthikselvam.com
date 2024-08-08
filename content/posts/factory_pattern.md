---
title: "Factory Design Pattern Practice Qestions"
description: "2023"
date: "2023-01-23"
tags:
- fundamentals
---

Exercise 1: Simple Factory
Objective: Implement a simple factory.
Steps:
Create a Shape interface with a method draw().
Implement concrete classes Circle, Square, and Rectangle that implement Shape.
Create a ShapeFactory class with a method getShape(String shapeType) that returns an instance of a specific shape.
Demonstrate using ShapeFactory to create and use different shapes.

Exercise 2: Factory Method
Objective: Implement the Factory Method pattern.
Steps:
Create a Product interface with a method use().
Implement concrete classes ConcreteProductA and ConcreteProductB that implement Product.
Create an abstract class Creator with an abstract method createProduct().
Implement concrete creators ConcreteCreatorA and ConcreteCreatorB that extend Creator and implement the createProduct() method.
Demonstrate using different creators to create and use products.

Exercise 3: Abstract Factory
Objective: Implement the Abstract Factory pattern.
Steps:
Create an interface GUIFactory with methods createButton() and createCheckbox().
Implement concrete factories WindowsFactory and MacFactory that implement GUIFactory.
Create interfaces Button and Checkbox with a method render().
Implement concrete classes WindowsButton, MacButton, WindowsCheckbox, and MacCheckbox that implement Button and Checkbox.
Demonstrate creating different GUIs using the abstract factory.

Exercise 4: Parameterized Factory
Objective: Implement a factory with parameterized creation.
Steps:
Create a Vehicle interface with a method drive().
Implement concrete classes Car, Bike, and Truck that implement Vehicle.
Create a VehicleFactory class with a method getVehicle(String vehicleType, String color, int horsepower) that returns an instance of a specific vehicle with given parameters.
Demonstrate creating and using vehicles with different parameters.

Exercise 5: Factory with Registry
Objective: Implement a factory with a registry for product creation.
Steps:
Create an Animal interface with a method speak().
Implement concrete classes Dog, Cat, and Bird that implement Animal.
Create an AnimalFactory class with a registry to map string types to animal constructors.
Add methods to AnimalFactory to register and create animals based on string types.
Demonstrate registering and creating different animals using the factory.

Exercise 6: Singleton Factory
Objective: Combine Singleton and Factory patterns.
Steps:
Create a Connection interface with a method connect().
Implement concrete classes MySQLConnection and PostgreSQLConnection that implement Connection.
Create a singleton ConnectionFactory with a method getConnection(String type) that returns a specific connection instance.
Demonstrate using the singleton factory to create and use different connections.

Exercise 7: Reflection-Based Factory
Objective: Use reflection to create objects in a factory.
Steps:
Create an Appliance interface with a method operate().
Implement concrete classes Washer and Dryer that implement Appliance.
Create an ApplianceFactory class with a method getAppliance(Class<? extends Appliance> applianceClass) that uses reflection to create instances.
Demonstrate creating and using appliances with the reflection-based factory.