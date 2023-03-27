---
title: "Creational Design Patterns"
description: "2023"
date: "2023-01-23"
tags:
- fundamentals
---

Creational Design Patterns 

Creational design patterns solve common problems that arise during the creation of objects.

**1. Builder pattern** : The Builder pattern is a creational design pattern that separates the construction of complex objects from their representation, allowing the same construction process to create different representations. This pattern is especially useful when creating objects that require many steps to initialize, and whose initialization steps are optional or may vary.

```java
public class Car {
    private String make;
    private String model;
    private int year;
    private String color;
    private String engine;
 
    // Private constructor to enforce object creation through builder
    private Car(Builder builder) {
        this.make = builder.make;
        this.model = builder.model;
        this.year = builder.year;
        this.color = builder.color;
        this.engine = builder.engine;
    }
 
    public static class Builder {
        private String make;
        private String model;
        private int year;
        private String color;
        private String engine;
 
        public Builder make(String make) {
            this.make = make;
            return this;
        }
 
        public Builder model(String model) {
            this.model = model;
            return this;
        }
 
        public Builder year(int year) {
            this.year = year;
            return this;
        }
 
        public Builder color(String color) {
            this.color = color;
            return this;
        }
 
        public Builder engine(String engine) {
            this.engine = engine;
            return this;
        }
 
        public Car build() {
            return new Car(this);
        }
    }
}
```

In this example, the Car class has a private constructor to enforce object creation through the Builder. The Builder class has methods to set the different fields of the Car class, and a build() method to create the Car object.

Here's an example of how to use the Builder to create a Car object:

```java
Car car = new Car.Builder()
                .make("Toyota")
                .model("Camry")
                .year(2022)
                .color("Silver")
                .engine("V6")
                .build();
```
--- 

**2. Factory pattern** : The Factory pattern is a creational design pattern that provides a way to create objects without specifying the exact class of object that will be created. Instead, a factory class is used to create objects based on certain conditions or parameters.

```java
public interface Car {
    public void drive();
}

public class Sedan implements Car {
    @Override
    public void drive() {
        System.out.println("Driving a Sedan");
    }
}

public class SUV implements Car {
    @Override
    public void drive() {
        System.out.println("Driving an SUV");
    }
}

public class CarFactory {
    public static Car createCar(String carType) {
        if(carType.equalsIgnoreCase("Sedan")) {
            return new Sedan();
        } else if(carType.equalsIgnoreCase("SUV")) {
            return new SUV();
        } else {
            throw new IllegalArgumentException("Invalid car type: " + carType);
        }
    }
}
```
In this example, we have an interface called Car with a method called drive(), and two classes that implement the Car interface: Sedan and SUV. We also have a CarFactory class with a createCar() method that takes a carType parameter and creates a Car object based on the specified car type.

Here's an example of how to use the CarFactory to create a Car object:

```java
Car sedan = CarFactory.createCar("Sedan");
sedan.drive(); // Output: "Driving a Sedan"

Car suv = CarFactory.createCar("SUV");
suv.drive(); // Output: "Driving an SUV"
```

In this example, we create a Sedan and SUV object using the CarFactory.createCar() method, and then call the drive() method on each object to drive the car. The CarFactory class takes care of creating the correct type of Car object based on the input carType.

Advantages of using static method in factory method:
- It makes the method easily accessible: Since the method is static, it can be accessed directly from the class name, without the need to create an instance of the class.

- It simplifies the code: Since the method is static, it doesn't require an instance of the class to be created, which can simplify the code.

- It is thread-safe: Static methods are thread-safe by default, which means that multiple threads can call the method simultaneously without causing any issues.

---

**3. Abstract Factory pattern** : The Abstract Factory Pattern is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes. It is useful when you need to create a set of related objects that work together and have the same interface, but the concrete implementation of those objects may vary depending on some condition.

Here's an example implementation of the Abstract Factory Pattern in Java:

```java
public interface Car {
    public void drive();
}

public interface CarFactory {
    public Car createCar();
}

public class Sedan implements Car {
    @Override
    public void drive() {
        System.out.println("Driving a Sedan");
    }
}

public class SUV implements Car {
    @Override
    public void drive() {
        System.out.println("Driving an SUV");
    }
}

public class SedanFactory implements CarFactory {
    @Override
    public Car createCar() {
        return new Sedan();
    }
}

public class SUVFactory implements CarFactory {
    @Override
    public Car createCar() {
        return new SUV();
    }
}
```

In this example, we have an interface called Car with a method called drive(), and an interface called CarFactory with a method called createCar(). We also have two classes that implement the Car interface: Sedan and SUV. Additionally, we have two classes that implement the CarFactory interface: SedanFactory and SUVFactory.

The SedanFactory and SUVFactory classes are responsible for creating Sedan and SUV objects, respectively. They implement the createCar() method to return a Car object of the corresponding type.

Here's an example of how to use the Abstract Factory Pattern to create a set of related objects:
```java
CarFactory sedanFactory = new SedanFactory();
Car sedan = sedanFactory.createCar();
sedan.drive(); // Output: "Driving a Sedan"

CarFactory suvFactory = new SUVFactory();
Car suv = suvFactory.createCar();
suv.drive(); // Output: "Driving an SUV"

```

In this example, we create two instances of CarFactory - SedanFactory and SUVFactory. We then use these factories to create Car objects of the corresponding type - Sedan and SUV. Finally, we call the drive() method on each object to drive the car.

The Abstract Factory Pattern makes it easy to create a set of related objects that work together and have the same interface, without specifying their concrete classes. This provides a lot of flexibility and makes it easier to modify the object creation process without affecting the rest of the code.

---

**4. Difference between factory and abstract factory**
The main difference between the Factory Pattern and the Abstract Factory Pattern is that the Factory Pattern creates individual objects, whereas the Abstract Factory Pattern creates families of related objects.

The Factory Pattern is used to create a single object of a specific type. It provides a single interface that can be used to create an object of a specific class, based on some condition. This pattern is useful when you need to create an object of a specific type and want to decouple the object creation process from the rest of the code.

On the other hand, the Abstract Factory Pattern is used to create a family of related objects. It provides an interface for creating families of related or dependent objects, without specifying their concrete classes. This pattern is useful when you need to create a set of related objects that work together and have the same interface, but the concrete implementation of those objects may vary depending on some condition.

In general, you would use the Factory Pattern when you need to create a single object of a specific type, and the Abstract Factory Pattern when you need to create a family of related objects that work together.

For example, let's say you are building a car dealership application. You might use the Factory Pattern to create instances of individual car models, such as a Sedan or an SUV. However, you would use the Abstract Factory Pattern to create families of related objects, such as a family of Sedans that have the same engine type and features.

In summary, the Factory Pattern and the Abstract Factory Pattern are both useful creational design patterns, but they serve different purposes. The Factory Pattern is used to create individual objects of a specific type, whereas the Abstract Factory Pattern is used to create families of related objects that work together.

---

**5. Singleton pattern** :The Singleton pattern is a creational design pattern that ensures that a class has only one instance and provides a global point of access to it. This pattern is useful when we need to ensure that only one instance of a class is created and shared across the application.

Here's an example implementation of the Singleton pattern in Java using the car concept:
```java
public class CarSingleton {
    // Private static instance of the singleton class
    private static CarSingleton instance;

    // Private constructor to prevent direct instantiation of the class
    private CarSingleton() {
    }

    // Public static method to get the singleton instance
    public static CarSingleton getInstance() {
        if (instance == null) {
            instance = new CarSingleton();
        }
        return instance;
    }

    public void drive() {
        System.out.println("Driving the car...");
    }
}

// Client class
public class CarClient {
    public static void main(String[] args) {
        CarSingleton car1 = CarSingleton.getInstance();
        CarSingleton car2 = CarSingleton.getInstance();

        // Both car1 and car2 will reference the same instance
        System.out.println(car1 == car2);

        car1.drive();
        car2.drive();
    }
}
```

In this example, we have a CarSingleton class that implements the Singleton pattern. The class has a private static instance of itself and a private constructor to prevent direct instantiation of the class. The public static method getInstance() returns the singleton instance, creating it if it doesn't already exist. Finally, we have a drive() method that simulates driving the car.

In the client class, we create two CarSingleton objects using the getInstance() method. Since the getInstance() method returns the same instance each time it is called, both car1 and car2 will reference the same instance of the CarSingleton class. Finally, we call the drive() method on both car1 and car2 to demonstrate that they are referencing the same object.

---

**5. Singleton pattern** : The Prototype pattern is a creational design pattern that allows creating new objects by copying existing ones, without exposing their underlying implementation details. This pattern provides a way to create new objects efficiently by cloning existing instances, rather than creating them from scratch.

Here's an example implementation of the Prototype pattern in Java using the car concept:
```java
// Prototype interface
public interface CarPrototype extends Cloneable {
    public CarPrototype clone();
}

// Concrete Prototype
public class Car implements CarPrototype {
    private String make;
    private String model;
    private int year;

    public Car(String make, String model, int year) {
        this.make = make;
        this.model = model;
        this.year = year;
    }

    public void setMake(String make) {
        this.make = make;
    }

    public void setModel(String model) {
        this.model = model;
    }

    public void setYear(int year) {
        this.year = year;
    }

    @Override
    public CarPrototype clone() {
        return new Car(make, model, year);
    }

    @Override
    public String toString() {
        return "Car{" +
                "make='" + make + '\'' +
                ", model='" + model + '\'' +
                ", year=" + year +
                '}';
    }
}

// Client class
public class CarClient {
    public static void main(String[] args) {
        CarPrototype carPrototype = new Car("Toyota", "Camry", 2022);
        CarPrototype clonedCar1 = carPrototype.clone();
        CarPrototype clonedCar2 = carPrototype.clone();
        
        // Changing properties of cloned car
        ((Car) clonedCar1).setYear(2023);
        ((Car) clonedCar2).setMake("Honda");

        System.out.println(carPrototype.toString());
        System.out.println(clonedCar1.toString());
        System.out.println(clonedCar2.toString());
    }
}
```
In this example, we have a CarPrototype interface that defines the clone() method. The Car class implements the CarPrototype interface and provides an implementation of the clone() method. The CarClient class demonstrates how the prototype pattern can be used to create new Car objects efficiently by cloning an existing Car object. In the client class, we create a carPrototype object with some initial values. We then create two clonedCar objects by calling the clone() method on the carPrototype object. Finally, we modify the properties of the clonedCar objects to demonstrate that they are separate instances.


