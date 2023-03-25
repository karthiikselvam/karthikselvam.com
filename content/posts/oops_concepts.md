---
title: "Understanding OOP concepts"
description: "2023"
date: "2023-01-25"
tags:
- fundamentals
---

**1. What is an Object?** 

An object is an entity in the real world that possesses state (fields) and behaviors (methods). It represents an instance of a class, occupies space in memory, and can communicate with other objects

**2. What is a Class?** 

A class is a programming construct that serves as a template or blueprint for creating objects. Unlike objects, classes do not consume memory. Instead, they define the properties and behaviors of objects that can be instantiated multiple times.

**3. What is a Abstraction?** 

Abstraction is the concept of exposing to the user only the relevant information while hiding the unnecessary details. This enables the user to focus on what the application does, rather than how it does it.

Let's consider a real-life example: a man driving a car. The man knows what each pedal does and what the steering wheel does, but he doesn't know how these things are done internally by the car. He doesn't know about the inner mechanisms that empower these things. This is what abstraction is. In Java, abstraction can be achieved via abstract classes and interfaces.
```java
public interface Car {
    public void speedUp();
    public void slowDown();
    public void turnRight();
    public void turnLeft();
    public String getCarType();
}
```
Next, each type of car should implement the Car interface and override these methods to provide the implementation of these actions. This implementation is hidden from the user (the man driving the car). For example, the ElectricCar class appears as follows:
```java
public class ElectricCar implements Car {
    private final String carType;

    public ElectricCar(String carType) {
        this.carType = carType;
    } 

    @Override
    public void speedUp() {
        System.out.println("Speed up the electric car");
    }
    @Override
    public void slowDown() {
        System.out.println("Slow down the electric car");
    }

    @Override
    public void turnRight() {
        System.out.println("Turn right the electric car");
    }

    @Override
    public void turnLeft() {
        System.out.println("Turn left the electric car");
    }
    
    @Override
    public String getCarType() {
         return this.carType;
    } 
}
```
The user of this class has access to these public methods without being aware of the implementation:
```
public class Main {
    public static void main(String[] args) {
        Car electricCar = new ElectricCar("BMW");
        System.out.println("Driving the electric car: " + electricCar.getCarType() + "\n");
        electricCar.speedUp();
        electricCar.turnLeft();
        electricCar.slowDown();
    }
}
```
The output is listed as follows:
```
Driving the electric car: BMW
Speed up the electric car
Turn left the electric car
Slow down the electric car
```
**4. What is a Encapsulation?** 

Encapsulation is a technique whereby the state of an object is hidden from the outside world, and a set of public methods are exposed for accessing this state. Encapsulation is achieved when each object keeps its state private inside a class. It is known as a data-hiding mechanism, and has several important advantages associated with it, such as enabling loosely coupled, reusable, secure, and easy-to-test code.

In Java, encapsulation is implemented through the use of access modifiers such as public, private, and protected.
```java
public class Person {
  private String name;
  private int age;

  // Getter method for name
  public String getName() {
    return name;
  }

  // Setter method for name
  public void setName(String name) {
    this.name = name;
  }

  // Getter method for age
  public int getAge() {
    return age;
  }

  // Setter method for age
  public void setAge(int age) {
    this.age = age;
  }
}
```
In this example, we have a Person class that encapsulates the state (name and age) of a person object. The state is hidden from the outside world through the use of private access modifiers on the name and age variables. However, public getter and setter methods (getName(), setName(), getAge(), setAge()) are provided for accessing and modifying the state of the object. This allows us to maintain control over the state of the object, ensuring that it remains valid and consistent at all times, while also providing a well-defined interface for other parts of the program to interact with the object.

**5. What is a Inheritance ?**

Inheritance is a fundamental concept in object-oriented programming, which allows one class (the child or subclass) to inherit properties and methods from another class (the parent or superclass). This helps to promote code reuse, reduce duplication, and make the code more modular and easier to maintain.

In Java, inheritance is achieved through the use of the **extends** keyword. The child class inherits all the visible properties and methods of the parent class, which can be overridden or extended as needed.

Here's an example of inheritance in Java:
```java
public class Animal {
  private String name;
  private int age;

  public Animal(String name, int age) {
    this.name = name;
    this.age = age;
  }

  public void eat() {
    System.out.println(name + " is eating.");
  }

  public void sleep() {
    System.out.println(name + " is sleeping.");
  }
}

public class Cat extends Animal {
  public Cat(String name, int age) {
    super(name, age);
  }

  public void meow() {
    System.out.println("Meow!");
  }

  @Override
  public void sleep() {
    System.out.println(getName() + " is curling up and sleeping.");
  }
}
```
In this example, we have an Animal class that defines common properties and methods for all animals, such as name and age, and eat() and sleep() methods. The Cat class extends the Animal class, inheriting all its properties and methods, and also adds a new meow() method.

We can now create a Cat object and call its methods like this:
```
Cat cat = new Cat("Kitty", 2);
cat.eat(); // Output: Kitty is eating.
cat.sleep(); // Output: Kitty is curling up and sleeping.
cat.meow(); // Output: Meow!
```
In this example, the Cat class has overridden the sleep() method inherited from the Animal class, to provide a more specific implementation for cats. This demonstrates the flexibility and extensibility of inheritance, which allows us to modify the behavior of a class to better fit our needs.

**6. What is a Polymorphism ?**

Polymorphism is a concept in object-oriented programming that enables an object to exhibit different behaviors in certain scenarios. This can be achieved through method overloading, which is a form of compile-time polymorphism, or through method overriding, which is a form of runtime polymorphism and is applicable in the case of an IS-A relationship.

Polymorphism via method overloading(compile time)
```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
}
```
Polymorphism via method overriding(runtime)

```java
public class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog barks");
    }
}

public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal1 = new Dog();
        Animal animal2 = new Cat();
        
        animal1.makeSound(); // Output: Dog barks
        animal2.makeSound(); // Output: Cat meows
    }
}
```
In the example above, we have an Animal class and two subclasses, Dog and Cat, that extend it. The makeSound method is overridden in each subclass to produce a different sound. In the Main class, we create an instance of Dog and Cat but assign them to variables of type Animal. When the makeSound method is called on each of these objects, the appropriate overridden version of the method is executed, producing "Dog barks" and "Cat meows" as output, respectively. This is an example of polymorphism as the same method is called on objects of different types, but the behavior is different depending on the actual type of the object at runtime.

**7. What is a Association ?**

Association is a concept in object-oriented programming that describes the relationship between two classes that are independent of each other. An association does not have an owner, and it can take different forms, including one-to-one, one-to-many, many-to-one, and many-to-many, depending on the cardinality and multiplicity of the relationship between the two classes.

Here is an example of association in Java using a one-to-many relationship:
```java
public class Library {
   private List<Book> books;

   public Library() {
      books = new ArrayList<>();
   }

   public void addBook(Book book) {
      books.add(book);
   }
}

public class Book {
   private String title;
   private String author;

   public Book(String title, String author) {
      this.title = title;
      this.author = author;
   }

   // getters and setters

}
```
In this example, the Library class has an association with the Book class. The Library class has a list of Book objects, and it provides a method addBook() to add a book to the list. This is an example of a one-to-many association because a Library can have many Book objects in its list.

The Book class, on the other hand, has no knowledge of the Library class. It simply defines the properties of a book such as its title and author. This is an example of unidirectional association.

**8. What is a Aggregation ?**

Aggregation is one of the core concepts of OOP. Mainly, aggregation is a special case of unidirectional association. While an association defines the relationship between two classes independent of one another, aggregation represents a HAS-A relationship between these two classes. In other words, two aggregated objects have their own life cycle, but one of the objects is the owner of the HAS-A relationship. Having their own life cycle means that ending one object will not affect the other object. For example, a TennisPlayer has a Racket. This is a unidirectional association since a Racket cannot have a TennisPlayer. Even if the TennisPlayer dies, the Racket is not affected.

For example, a TennisPlayer has a Racket. This is a unidirectional association since a Racket cannot have a TennisPlayer. Even if the TennisPlayer dies, the Racket is not affected.

```java
public class Racket {
    private String type;
    private int size;
    private int weight;

    public Racket(String type, int size, int weight) {
        this.type = type;
        this.size = size;
        this.weight = weight;
    }
 // getters and setters omitted for brevity
}
```
A TennisPlayer HAS-A Racket. Therefore, the TennisPlayer class must be capable of receiving a Racket as follows:
```java
public class TennisPlayer {
    private String name;
    private Racket racket;
    
    public TennisPlayer(String name, Racket racket) {
        this.name = name;
        this.racket = racket
 }
 // getters and setters omitted for brevity
}
```
Next, we create a Racket and a TennisPlayer that uses this Racket:
```java
public static void main(String[] args) {
    Racket racket = new Racket("Babolat Pure Aero", 100, 300);
    TennisPlayer player = new TennisPlayer("Rafael Nadal",  racket);
    System.out.println("Player " + player.getName() + " plays with " + player.getRacket().getType());
}
```
The output is as follows:
```
Player Rafael Nadal plays with Babolat Pure Aero
```

**9. What is a Composition ?**

Composition is one of the core concepts of OOP. Primarily, composition is a more restrictive case of aggregation. While aggregation represents a HAS-A relationship between two objects having their own life cycle, composition represents a HAS-A relationship that contains an object that cannot exist on its own. In order to highlight this coupling, the HAS-A relationship can be named PART-OF as well. For example, a Car has an Engine. In other words, the engine is PART-OF the car. If the car is destroyed, then the engine is destroyed as well. Composition is said to be better than inheritance because it sustains code reuse and the visibility control of objects.
```java
public class Engine {
    private String type;
    private int horsepower;
    public Engine(String type, int horsepower) {
        this.type = type;
        this.horsepower = horsepower;
    }
 // getters and setters omitted for brevity
}
```
Next, we have the Car class. Check out the constructor of this class. Since Engine is part of Car, we create it with the Car.
```java
public class Car {
    private final String name;
    private final Engine engine;
    
    public Car(String name) {
        this.name = name;
        Engine engine = new Engine("petrol", 300);
        this.engine=engine;
    }

    public int getHorsepower() {
        return engine.getHorsepower();
    }

    public String getName() {
        return name;
    } 
}
```
we can test composition from the main() method as follows:
```java
public static void main(String[] args) {
    Car car = new Car("MyCar");
    System.out.println("Horsepower: " + car.getHorsepower());
}
```
output is as follows:
```java
Horsepower: 300
```

That's it for OOPS.