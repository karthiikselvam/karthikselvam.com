---
title: "Structural Design Patterns"
description: "2023"
date: "2023-01-23"
tags:
- fundamentals
---

Structural Design Patterns are design patterns that focus on the composition of classes and objects to form larger structures, without changing their individual behaviors. They help to simplify the relationships between objects and classes in a system, and to make the system more flexible and efficient. Following are some of the commonly used structural design patterns.

**1. Adapter pattern** : Adapter pattern is a structural design pattern that allows incompatible classes to work together by converting the interface of one class into another interface that clients expect. This pattern is useful when you have an existing class that cannot be changed, but you need to use it with other classes that have different interfaces.

Let's consider a car example to explain the Adapter pattern in Java. Suppose we have an existing class called "Car" that has a method called "drive()" that takes no arguments and starts the car. We also have another class called "Engine" that has a method called "start()" that starts the engine. However, the interfaces of these two classes are not compatible because the "drive()" method takes no arguments, while the "start()" method of the Engine class takes an argument.

To make these classes work together, we can create an adapter class called "EngineAdapter" that implements the Car interface and uses the Engine class to start the car. The EngineAdapter will have a reference to an Engine object and will use its "start()" method to start the car when the "drive()" method is called.

Here is the example code in Java:
```java
interface Car {
    void drive();
}

class Engine {
    void start(String key) {
        System.out.println("Starting engine with key: " + key);
    }
}

class EngineAdapter implements Car {
    private Engine engine;
    
    public EngineAdapter(Engine engine) {
        this.engine = engine;
    }
    
    @Override
    public void drive() {
        engine.start("ignition key");
    }
}

public class Main {
    public static void main(String[] args) {
        Engine engine = new Engine();
        Car car = new EngineAdapter(engine);
        car.drive();
    }
}
```
In this example, the EngineAdapter class adapts the Engine class to the Car interface by implementing the drive() method and using the start() method of the Engine class to start the car. When we call the drive() method on the Car object, it calls the start() method of the Engine object, which starts the car engine.

In summary, the Adapter pattern allows us to use incompatible classes together by creating an adapter class that implements the required interface and uses the existing class to provide the required functionality.

--- 

**2. Composite pattern** : Adapter pattern is a structural design pattern that allows incompatible classes to work together by converting the interface of one class into another interface that clients expect. This pattern is useful when you have an existing class that cannot be changed, but you need to use it with other classes that have different interfaces.