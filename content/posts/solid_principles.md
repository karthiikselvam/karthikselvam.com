---
title: "Understanding SOLID Principles"
description: "2023"
date: "2023-01-24"
tags:
- fundamentals
---


SOLID is an acronym of the following:
1. S: Single Responsibility Principle
2. O: Open Closed Principle
3. L: Liskov's Substitution Principle
4. I: Interface Segregation Principle
5. D: Dependency Inversion Principle 
---

**1. What is Single Responsibility Principle ?**

S stands for One class should have one, and only one, responsibility. S tells us to write a class for only one goal. As long as we write a class for only one goal, we will sustain high maintainability and visibility control across the application modules. In other words, by sustaining high maintainability, this principle has a significant business impact, and by providing visibility control across the application modules, this principle sustains encapsulation.

For example, the following class computes the area and converts it to inches:
```java
public class RectangleAreaCalculator {
    private static final double INCH_TERM = 0.0254d;
    private final int width;
    private final int height;
    
    public RectangleAreaCalculator(int width, int height) {
        this.width = width;
        this.height = height;
    }
    
    public int area() {
        return width * height;
    }
    
    // this method breaks SRP
    public double metersToInches(int area) {
        return area / INCH_TERM;
    } 
}
```
The situation can be remedied by removing the metersToInches() method from RectangleAreaCalculator, as follows:
```java
public class RectangleAreaCalculator {
    private final int width;
    private final int height;
    
    public RectangleAreaCalculator(int width, int height) {
        this.width = width;
        this.height = height;
    }
 
    public int area() {
        return width * height;
    } 
}
```
Now, RectangleAreaCalculator does only one thing (it computes the rectangle area), thereby observing the SRP. 

Next, metersToInches() can be extracted in a separate class.
```java
public class AreaConverter {
    private static final double INCH_TERM = 0.0254d;
    private static final double FEET_TERM = 0.3048d;
    
    public double metersToInches(int area) {
        return area / INCH_TERM;
    }
    
    public double metersToFeet(int area) {
        return area / FEET_TERM;
    }
}
```
---
**2. What is Open Closed Principle ?**

O stands for Software components should be open for extension, but closed for modification. O sustains the fact that our classes should not contain constraints that will require other developers to modify our classes in order to accomplish their job – other developers should only extend our classes to accomplish their job.

Each shape will implement the Shape interface. Therefore, the code is pretty straightforward:
```java
    public interface Shape { 
    }
    
    public class Rectangle implements Shape {
        private final int width;
        rivate final int height;
        // constructor and getters omitted for brevity
    }
    
    public class Circle implements Shape {
        private final int radius;
        // constructor and getter omitted for brevity
}
```
At this point, we can easily use the constructors of these classes to create rectangles and circles of different sizes. Once we have several shapes, we want to sum their areas. For this, we can define an AreaCalculator class as follows:
```java
    public class AreaCalculator {
        private final List<Shape> shapes;
        public AreaCalculator(List<Shape> shapes) {
            this.shapes = shapes;
    }
 
    // adding more shapes requires us to modify this class
    // this code is not OCP compliant
    public double sum() {
        int sum = 0;
        for (Shape shape : shapes) {
            if (shape.getClass().equals(Circle.class)) {
                sum += Math.PI * Math.pow(((Circle) shape).getRadius(), 2);
            } else 
            if(shape.getClass().equals(Rectangle.class)) {
                sum += ((Rectangle) shape).getHeight() * ((Rectangle) shape).getWidth();
            }
        }
        return sum;
    }
}
```
Since each shape has its own formula for area, we require an if-else (or switch) structure to determine the type of shape. Furthermore, if we want to add a new shape (for example, a triangle), we have to modify the AreaCalculator class to add a new if case. This means that the preceding code breaks the OCP.

The main idea is to extract from AreaCalculator the area formula of each shape in the corresponding Shape class. Hence, the rectangle will compute its area, the circle as well, and so on. To enforce the fact that each shape must calculate its area, we add the area() method to the Shape contract:

```java
public interface Shape { 
    public double area();
}
```
Next, Rectangle and Circle implements Shape as follows:
```java
 public class Rectangle implements Shape {
    private final int width;
    private final int height;

 public Rectangle(int width, int height) {
    this.width = width;
    this.height = height;
 }

  public double area() {
    return width * height;
 }
}

public class Circle implements Shape {
    private final int radius;
    public Circle(int radius) {
        this.radius = radius;
    }
 
    @Override
    public double area() {
        return Math.PI * Math.pow(radius, 2);
    }
}
```
Now, the AreaCalculator can loop the list of shapes and sum the areas by calling the  proper area() method.
```java
public class AreaCalculator {
    private final List<Shape> shapes;
    public AreaCalculator(List<Shape> shapes) {
        this.shapes = shapes;
    }

     public double sum() {
        int sum = 0;
        for (Shape shape : shapes) {
            sum += shape.area();
        }
        return sum;
    }
}
```
---
**3. What is Liskov's Substitution Principle ?**

L stands for Derived types must be completely substitutable for their base types. L sustains the fact that objects of subclasses must behave in the same way as the objects of superclasses, so every subclass (or derived class) should be capable of substituting their superclass without any issues. Most of the time, this is useful for runtime-type identification followed by the cast. For example, consider foo(p), where p is of the type T. Then, foo(q) should work fine if q is of the type S and S is a subtype of T.

Suppose we have a class hierarchy for different shapes, with a base class Shape and two derived classes Circle and Rectangle. Each class has a method area() to calculate the area of the shape.
```java
public abstract class Shape {
    public abstract double area();
}

public class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double area() {
        return 3.14 * radius * radius;
    }
}

public class Rectangle extends Shape {
    private double length;
    private double width;

    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    public double area() {
        return length * width;
    }
}
```
Now suppose we have a method calculateTotalArea that takes an array of shapes and returns the total area of all the shapes in the array:
```
public static double calculateTotalArea(Shape[] shapes) {
    double totalArea = 0.0;
    for (Shape shape : shapes) {
        totalArea += shape.area();
    }
    return totalArea;
}
```
According to Liskov's Substitution Principle, we should be able to pass an array of Circle or Rectangle objects to this method without any problems, since both classes inherit from the Shape base class and implement the area() method.
```
Circle circle = new Circle(5);
Rectangle rectangle = new Rectangle(3, 4);
Shape[] shapes = {circle, rectangle};
double totalArea = calculateTotalArea(shapes); // returns 83.5
```
This demonstrates that the Circle and Rectangle classes can be used interchangeably with the Shape class, without causing any errors or unexpected behavior in the program.

---

**4. What is Interface Segregation Principle ?**

I stands for the Interface Segregation Principle (ISP). I stands for Clients should not be forced to implement unnecessary methods that they will not use.

This principle stands for Clients should not be forced to implement unnecessary methods that they will not use. In other words, we should split an interface into two or more interfaces until clients are not forced to implement methods that they will not use. For example, consider the Connection interface, which has three methods: connect(), socket(), and http().

```java
public interface Connection {
    public void socket();
    public void http();
    public void connect();
}
```
WwwPingConnection is a class that pings different websites via HTTP; hence, it requires the http() method, but doesn't need the socket() method. Notice the dummy socket() implementation – since WwwPingConnection implements Connection, it is forced to provide an implementation to the socket() method as well:
```java
public class WwwPingConnection implements Connection {
    private final String www;

    public WwwPingConnection(String www) {
        this.www = www;
    }

    @Override
    public void http() {
        System.out.println("Setup an HTTP connection to " + www);
    }

    @Override
    public void connect() {
    System.out.println("Connect to " + www);
    }

    // this method breaks Interface Segregation Principle
    @Override
    public void socket() {
    }
}
```
Having an empty implementation or throwing a meaningful exception from methods that are not needed, such as socket(), is a really ugly solution. Check the following code:
```
WwwPingConnection www = new WwwPingConnection 'www.yahoo.com');
www.socket(); // we can call this method!
www.connect();
```
What do we expect to obtain from this code? A working code that does nothing, or an  exception caused by the connect() method because there is no HTTP endpoint? Or, we  can throw an exception from socket() of the type: Socket is not supported!. Then, why is  it here?! Hence, it is now time to refactor the code to follow the ISP. In order to comply with the ISP, we need to segregate the Connection interface. Since the connect() method is required by any client, we leave it in this interface.
```java
public interface Connection {
    public void connect();
}
```
The http() and socket() methods are distributed in to separate interfaces that extend the Connection interface as follows:
```java
public interface HttpConnection extends Connection {
    public void http();
    }
public interface SocketConnection extends Connection {
    public void socket();
}
```
This time, the WwwPingConnection class can implement only the HttpConnection interface and use the http() method:
```java
public class WwwPingConnection implements HttpConnection {
    private final String www;
    
    public WwwPingConnection(String www) {
        this.www = www;
    }

    @Override
    public void http() {
        System.out.println("Setup an HTTP connection to " + www);
    }
 
    @Override
    public void connect() {
        System.out.println("Connect to " + www);
    } 
}
```
--- 
**5. What is Dependency Inversion Principle ?**

D stands for the Dependency Inversion Principle. This principle stands for Depend on abstractions, not on concretions. This means that we should rely on abstract layers to bind concrete modules together instead of having concrete modules that depend on other concrete modules. To accomplish this, all concrete modules should expose abstractions only.

A database JDBC URL, PostgreSQLJdbcUrl, can be a low-level module, while a class that connects to the database may represent a high-level module, such as ConnectToDatabase#connect().

```java
public class PostgreSQLJdbcUrl {
    private final String dbName;
    public PostgreSQLJdbcUrl(String dbName) {
        this.dbName = dbName;
    }
    public String get() {
        return "jdbc:// ... " + this.dbName;
    }
}

public class ConnectToDatabase {
    public void connect(PostgreSQLJdbcUrl postgresql) {
        System.out.println("Connecting to " + postgresql.get());
    }
}
```
If we create another type of JDBC URL (for example, MySQLJdbcUrl), then we cannot use the preceding connect(PostgreSQLJdbcUrl postgreSQL) method. So, we have to drop this dependency on concrete and create a dependency on abstraction.

The abstraction can be represented by an interface that should be implemented by each type of JDBC URL
```
public interface JdbcUrl {
    public String get();
}

```
Next, PostgreSQLJdbcUrl implements JdbcUrl to return a JDBC URL specific to PostgreSQL databases:
```java
public class PostgreSQLJdbcUrl implements JdbcUrl {
    private final String dbName;

    public PostgreSQLJdbcUrl(String dbName) {
        this.dbName = dbName;
    }

    @Override
    public String get() {
        return "jdbc:// ... " + this.dbName;
    }
}
```
In precisely the same manner, we can write MySQLJdbcUrl, OracleJdbcUrl, and so on. Finally, the ConnectToDatabase#connect() method is dependent on the JdbcUrl abstraction, so it can connect to any JDBC URL that implements this abstraction.

```java
public class ConnectToDatabase {
    public void connect(JdbcUrl jdbcUrl) {
        System.out.println("Connecting to " + jdbcUrl.get());
 }
}
```
That's it, now you have solid understanding of SOLID principles.