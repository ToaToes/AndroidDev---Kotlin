## First. What are Dependencies

In object-oriented programming, dependencies refer to the relationships between classes and objects, indicating that one class or object relies on another to function properly. 

### Dependency for Class

Definition: A class dependency occurs when one class uses or requires another class to perform its functions. This can happen in various ways, such as through composition, aggregation, or association.

Types of Dependencies:

Direct Dependency: A class directly references another class, often through instance variables or method parameters.
</br>Indirect Dependency: A class may depend on another class through an interface or abstract class, allowing for more flexible designs.

```
class Engine {
    fun start() {
        // Engine start logic
    }
}

class Car(private val engine: Engine) { // Car depends on Engine
    fun start() {
        engine.start()
    }
}
```

### Dependency for Object 

Definition: An object dependency is similar to class dependencies but refers specifically to the instances of classes. When one object uses another object to perform its operations, it creates a dependency.

```
val engine = Engine() // Creating an Engine object
val car = Car(engine) // Car object depends on Engine object
```


Link: https://stackoverflow.com/questions/1638919/how-to-explain-dependency-injection-to-a-5-year-old

Link: https://www.freecodecamp.org/chinese/news/a-quick-intro-to-dependency-injection-what-it-is-and-when-to-use-it/

Link: https://stackoverflow.com/questions/131975/what-are-the-benefits-of-dependency-injection-containers#132097

**Link: https://www.jamesshore.com/v2/blog/2006/dependency-injection-demystified**

"Dependency injection means giving an object its instance variables"

Dependency Injection:

DI is a design pattern where an object (or class) receives its dependencies from an external source rather than creating them itself. This promotes loose coupling and enhances testability.
Instance Variables as Dependencies:

In a typical class, instance variables might represent dependencies that the class needs to function. However, with DI, instead of defining and instantiating these dependencies within the class, you can pass them as parameters.

_____

## Dependency, Variable, Instance Variables

Dependency not injection:
```
public class Example {
  private DatabaseThingie myDatabase;

  public Example() {
    myDatabase = new DatabaseThingie();
  }

  public void DoStuff() {
    ...
    myDatabase.GetData();
    ...
  }
}
```

Dependency injection:
```
public class Example {
  private DatabaseThingie myDatabase;

  public Example() {
    myDatabase = new DatabaseThingie();
  }

  public Example(DatabaseThingie useThisDatabaseInstead) {
    myDatabase = useThisDatabaseInstead;
  }

  public void DoStuff() {
    ...
    myDatabase.GetData();
    ...
  }
} 
```


_____

**Dependency Injection:**

DI is a design pattern where an object (or class) receives its dependencies from an external source rather than creating them itself. This promotes loose coupling and enhances testability.
**Instance Variables as Dependencies:**

In a typical class, instance variables might represent dependencies that the class needs to function. However, with DI, instead of defining and instantiating these dependencies within the class, you can pass them as parameters.

_____

**Strengths of a system built using DI patterns:**

DI code is much easier to reuse as the 'depended' functionality is extrapolated into well defined interfaces, allowing separate objects whose configuration is handled by a suitable application platform to be plugged into other objects at will.

DI code is much easier to test. The functionality expressed by the object can be tested in a black box by building 'mock' objects implementing the interfaces expected by your application logic.

DI code is more flexible. It is innately loosely coupled code -- to an extreme. This allows the programmer to pick and choose how objects are connected based exclusively on their required interfaces on one end and their expressed interfaces on the other.

External (Xml) configuration of DI objects means that others can customize your code in unforeseen directions.

External configuration is also a separation of concern pattern in that all problems of object initialization and object interdependency management can be handled by the application server.

Note that external configuration is not required to use the DI pattern, for simple interconnections a small builder object is often adequate. There is a tradeoff in flexibility between the two. A builder object is not as flexible an option as an externally visible configuration file. The developer of the DI system must weigh the advantages of flexibility over convenience, taking care that small scale, fine grain control over object construction as expressed in a configuration file may increase confusion and maintenance costs down the line.
