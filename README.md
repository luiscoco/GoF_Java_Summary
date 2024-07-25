# GoF_Java_Summary

## 1. Module 1

### 1.1 Introduction

**Design patterns** are typical solutions to commonly occurring problems in software design. 

They represent best practices used by experienced object-oriented software developers. 

Understanding these patterns can help you create more flexible, reusable, and maintainable code. 

In this module, we will introduce the concept of design patterns, explore the origins of these patterns, and understand their importance in software development.

### 1.2 Authors and Recommended Reading

The concept of design patterns in software engineering was popularized by the "**Gang of Four" (GoF), consisting of Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides**.

Their book, "**Design Patterns: Elements of Reusable Object-Oriented Software**" is a seminal work in the field. 

For a deeper understanding of design patterns, it is recommended to read the following:

"**Design Patterns**: Elements of Reusable Object-Oriented Software" by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides

"**Head First Design Patterns**" by Eric Freeman and Elisabeth Robson

"**Refactoring**: Improving the Design of Existing Code" by Martin Fowler

"**Clean Code**: A Handbook of Agile Software Craftsmanship" by Robert C. Martin

### 1.3 Object-Oriented Programming (OOP) and UML

**Object-Oriented Programming (OOP)** is a programming paradigm based on the concept of objects, which can contain data and code: data in the form of fields (often known as attributes or properties), 

and code in the form of procedures (often known as methods). 

The key principles of OOP include encapsulation, inheritance, and polymorphism.

**Unified Modeling Language (UML)** is a standardized modeling language that provides a set of graphical notation techniques to create visual models of object-oriented software systems. 

UML is used to specify, visualize, construct, and document the artifacts of software systems. 

Common UML diagrams include:

**Class Diagrams**

**Sequence Diagrams**

**Use Case Diagrams**

**Activity Diagrams**

### 1.4 Principles of Object-Oriented Design

**Object-oriented design principles** help to create a system that is easy to maintain and extend over time

These principles ensure that the software is flexible and can adapt to changes without major rewrites

Some key object-oriented design principles include:

**Encapsulation**: Bundling the data (attributes) and methods (functions) that operate on the data into a single unit or class.

**Abstraction**: Hiding the complex implementation details and showing only the essential features of the object.

**Inheritance**: Mechanism where a new class inherits properties and behavior (methods) from an existing class.

**Polymorphism**: Ability of different objects to respond, each in its own way, to identical messages (or methods).

### 1.5 SOLID Principles

The **SOLID principles** are a set of five design principles intended to make software designs more understandable, flexible, and maintainable.

These principles were introduced by Robert C. Martin and are considered the cornerstone of **object-oriented** design.

The **SOLID principles** include:

**Single Responsibility Principle (SRP)**: A class should have only one reason to change, meaning it should have only one job or responsibility.

**Open/Closed Principle (OCP)**: Software entities should be open for extension but closed for modification.

**Liskov Substitution Principle (LSP)**: Objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program.

**Interface Segregation Principle (ISP)**: Clients should not be forced to depend on interfaces they do not use. Instead of one large interface, multiple smaller interfaces are preferred.

**Dependency Inversion Principle (DIP)**: High-level modules should not depend on low-level modules. 

Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.

By adhering to these principles, developers can create software that is easier to maintain, scale, and refactor.
 
## 2. Module 2:

Module 2: Creational Design Patterns
2.1 Singleton
Text:
The Singleton pattern ensures a class has only one instance and provides a global point of access to it. This pattern is useful when exactly one object is needed to coordinate actions across the system.

Java Code Example:

java
Copy code
public class Singleton {
    // Private static variable of the same class that is the only instance of the class.
    private static Singleton instance;

    // Private constructor to restrict instantiation of the class from other classes.
    private Singleton() {}

    // Public static method that returns the instance of the class, creating it if it doesn't exist.
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

    public void showMessage() {
        System.out.println("Hello World from Singleton!");
    }
}

// Usage
public class SingletonDemo {
    public static void main(String[] args) {
        // Get the only object available
        Singleton singleton = Singleton.getInstance();
        // Show the message
        singleton.showMessage();
    }
}
2.2 Prototype
Text:
The Prototype pattern is used to create a duplicate object while keeping performance in mind. This pattern involves implementing a prototype interface that tells to create a clone of the current object. The Prototype pattern is used when the cost of creating a new object is expensive or complex.

Java Code Example:

java
Copy code
import java.util.Hashtable;

abstract class Shape implements Cloneable {
    private String id;
    protected String type;

    abstract void draw();

    public String getType() {
        return type;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public Object clone() {
        Object clone = null;
        try {
            clone = super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return clone;
    }
}

class Rectangle extends Shape {
    public Rectangle() {
        type = "Rectangle";
    }

    @Override
    public void draw() {
        System.out.println("Inside Rectangle::draw() method.");
    }
}

class ShapeCache {
    private static Hashtable<String, Shape> shapeMap = new Hashtable<>();

    public static Shape getShape(String shapeId) {
        Shape cachedShape = shapeMap.get(shapeId);
        return (Shape) cachedShape.clone();
    }

    public static void loadCache() {
        Rectangle rectangle = new Rectangle();
        rectangle.setId("1");
        shapeMap.put(rectangle.getId(), rectangle);
    }
}

// Usage
public class PrototypePatternDemo {
    public static void main(String[] args) {
        ShapeCache.loadCache();
        Shape clonedShape = ShapeCache.getShape("1");
        System.out.println("Shape : " + clonedShape.getType());
    }
}
2.3 Factory Method
Text:
The Factory Method pattern provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created. This pattern delegates the responsibility of object instantiation to the subclasses.

Java Code Example:

java
Copy code
abstract class Shape {
    abstract void draw();
}

class Rectangle extends Shape {
    @Override
    void draw() {
        System.out.println("Inside Rectangle::draw() method.");
    }
}

class Circle extends Shape {
    @Override
    void draw() {
        System.out.println("Inside Circle::draw() method.");
    }
}

abstract class ShapeFactory {
    // Factory Method
    abstract Shape createShape();

    public void drawShape() {
        Shape shape = createShape();
        shape.draw();
    }
}

class RectangleFactory extends ShapeFactory {
    @Override
    Shape createShape() {
        return new Rectangle();
    }
}

class CircleFactory extends ShapeFactory {
    @Override
    Shape createShape() {
        return new Circle();
    }
}

// Usage
public class FactoryMethodPatternDemo {
    public static void main(String[] args) {
        ShapeFactory shapeFactory = new RectangleFactory();
        shapeFactory.drawShape();

        shapeFactory = new CircleFactory();
        shapeFactory.drawShape();
    }
}
2.4 Abstract Factory
Text:
The Abstract Factory pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes. This pattern is useful when the system needs to be independent of how its objects are created.

Java Code Example:

java
Copy code
interface Shape {
    void draw();
}

class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Inside Rectangle::draw() method.");
    }
}

class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Inside Circle::draw() method.");
    }
}

interface Color {
    void fill();
}

class Red implements Color {
    @Override
    public void fill() {
        System.out.println("Inside Red::fill() method.");
    }
}

class Blue implements Color {
    @Override
    public void fill() {
        System.out.println("Inside Blue::fill() method.");
    }
}

abstract class AbstractFactory {
    abstract Shape getShape(String shapeType);
    abstract Color getColor(String colorType);
}

class ShapeFactory extends AbstractFactory {
    @Override
    Shape getShape(String shapeType) {
        if (shapeType.equalsIgnoreCase("RECTANGLE")) {
            return new Rectangle();
        } else if (shapeType.equalsIgnoreCase("CIRCLE")) {
            return new Circle();
        }
        return null;
    }

    @Override
    Color getColor(String colorType) {
        return null;
    }
}

class ColorFactory extends AbstractFactory {
    @Override
    Shape getShape(String shapeType) {
        return null;
    }

    @Override
    Color getColor(String colorType) {
        if (colorType.equalsIgnoreCase("RED")) {
            return new Red();
        } else if (colorType.equalsIgnoreCase("BLUE")) {
            return new Blue();
        }
        return null;
    }
}

class FactoryProducer {
    public static AbstractFactory getFactory(String choice) {
        if (choice.equalsIgnoreCase("SHAPE")) {
            return new ShapeFactory();
        } else if (choice.equalsIgnoreCase("COLOR")) {
            return new ColorFactory();
        }
        return null;
    }
}

// Usage
public class AbstractFactoryPatternDemo {
    public static void main(String[] args) {
        AbstractFactory shapeFactory = FactoryProducer.getFactory("SHAPE");
        Shape shape1 = shapeFactory.getShape("CIRCLE");
        shape1.draw();

        Shape shape2 = shapeFactory.getShape("RECTANGLE");
        shape2.draw();

        AbstractFactory colorFactory = FactoryProducer.getFactory("COLOR");
        Color color1 = colorFactory.getColor("RED");
        color1.fill();

        Color color2 = colorFactory.getColor("BLUE");
        color2.fill();
    }
}
2.5 Builder
Text:
The Builder pattern is used to construct a complex object step by step. It allows you to create different representations of an object using the same construction process. This pattern is useful when the creation process involves several steps or when there are many configurations for an object.

Java Code Example:

java
Copy code
class Meal {
    private String drink;
    private String mainCourse;
    private String side;

    public String getDrink() {
        return drink;
    }

    public void setDrink(String drink) {
        this.drink = drink;
    }

    public String getMainCourse() {
        return mainCourse;
    }

    public void setMainCourse(String mainCourse) {
        this.mainCourse = mainCourse;
    }

    public String getSide() {
        return side;
    }

    public void setSide(String side) {
        this.side = side;
    }

    @Override
    public String toString() {
        return "Meal [drink=" + drink + ", mainCourse=" + mainCourse + ", side=" + side + "]";
    }
}

abstract class MealBuilder {
    protected Meal meal = new Meal();

    public Meal getMeal() {
        return meal;
    }

    public abstract void buildDrink();
    public abstract void buildMainCourse();
    public abstract void buildSide();
}

class ItalianMealBuilder extends MealBuilder {
    @Override
    public void buildDrink() {
        meal.setDrink("Wine");
    }

    @Override
    public void buildMainCourse() {
        meal.setMainCourse("Pizza");
    }

    @Override
    public void buildSide() {
        meal.setSide("Salad");
    }
}

class JapaneseMealBuilder extends MealBuilder {
    @Override
    public void buildDrink() {
        meal.setDrink("Sake");
    }

    @Override
    public void buildMainCourse() {
        meal.setMainCourse("Sushi");
    }

    @Override
    public void buildSide() {
        meal.setSide("Miso Soup");
    }
}

class MealDirector {
    public Meal constructMeal(MealBuilder mealBuilder) {
        mealBuilder.buildDrink();
        mealBuilder.buildMainCourse();
        mealBuilder.buildSide();
        return mealBuilder.getMeal();
    }
}

// Usage
public class BuilderPatternDemo {
    public static void main(String[] args) {
        MealDirector mealDirector = new MealDirector();

        MealBuilder italianMealBuilder = new ItalianMealBuilder();
        Meal italianMeal = mealDirector.constructMeal(italianMealBuilder);
        System.out.println("Italian Meal: " + italianMeal);

        MealBuilder japaneseMealBuilder = new JapaneseMealBuilder();
        Meal japaneseMeal = mealDirector.constructMeal(japaneseMealBuilder);
        System.out.println("Japanese Meal: " + japaneseMeal);
    }
}
These examples demonstrate the core concepts and applications of the Singleton, Prototype, Factory Method
 
## 3. Module 3

### 3.1. Adapter

### 3.2. Bridge

### 3.3. Composite

### 3.4. Decorator

### 3.5. Façade

### 3.6. Flyweight 

### 3.7. Proxy
 
## 4. Module 4

### 4.1. Interpreter

### 4.2. Iterator

### 4.3. Visitor

### 4.4. Observer

### 4.5. Mediator

### 4.6. Memento

### 4.7. Command

### 4.8. Chain of Responsibility
 
### 4.9. Template method

### 4.10. Strategy

### 4.11. State
