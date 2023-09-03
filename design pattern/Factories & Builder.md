1. **Builder Pattern**:
    - **Step-by-Step Construction**: The Builder pattern allows for the step-by-step construction of complex objects. You can incrementally add parts to an object, which is especially useful when creating objects with numerous configuration options or components.
    - **Separation of Construction and Representation**: It separates the construction process of an object from its representation, meaning that the same construction process can create different representations.
    - **Multiple Concrete Builders**: For different types or configurations of objects, there might be multiple concrete builders.
    - **Usually Has a Director**: A director typically uses the builder interface to construct an object. Clients generally use the director to create objects rather than using the builder directly.

```c++
class Car {
    // Components like engine, wheels, doors etc.
};

class CarBuilder {
public:
    virtual ~CarBuilder() {}
    virtual void buildEngine() = 0;
    virtual void buildWheels() = 0;
    virtual void buildDoors() = 0;
    virtual Car* getCar() = 0;
};

class SportsCarBuilder : public CarBuilder {
    Car* car;
public:
    SportsCarBuilder() { car = new Car(); }
    void buildEngine() override {
        // Build a sports car engine
    }
    void buildWheels() override {
        // Build 4 wide wheels
    }
    void buildDoors() override {
        // Build 2 doors
    }
    Car* getCar() override {
        return car;
    }
};

class CarDirector {
    CarBuilder* builder;
public:
    CarDirector(CarBuilder* b) : builder(b) {}

    void construct() {
        builder->buildEngine();
        builder->buildWheels();
        builder->buildDoors();
    }
};

int main() {
    CarBuilder* builder = new SportsCarBuilder();
    CarDirector director(builder);

    director.construct();
    
    Car* myCar = builder->getCar();

    delete builder;
    delete myCar;

    return 0;
}

```

1. **Factory Pattern**:
    - **Object Creation**: The Factory pattern is primarily about object creation. It provides an interface for creating objects but allows subclasses to determine which class to instantiate.
    - **No Incremental Construction**: Factory methods typically create and return an object in one go. There's no notion of incrementally adding parts.
    - **Centralizes Creation Logic**: All logic related to object creation is centralized in the factory method or class.
    - **Multiple Factories Possible**: For different types of objects, there might be multiple factory methods or classes, but each factory typically focuses on the creation of a specific type of object.


```c++
#define _USE_MATH_DEFINES
#include <cmath>
#include <iostream>

enum class PointType
{
  cartesian,
  polar
};

class Point
{
   Point(const float x, const float y)
    : x{x},
      y{y}
  {}

public:
  float x, y;


  friend std::ostream& operator<<(std::ostream& os, const Point& obj)
  {
    return os
      << "x: " << obj.x
      << " y: " << obj.y;
  }

  static Point NewCartesian(float x, float y)
  {
    return{ x,y };
  }
  
  static Point NewPolar(float r, float theta)
  {
    return{ r*cos(theta), r*sin(theta) };
  }
  
};

int main_z()
{
  auto p = Point::NewPolar(5, M_PI_4);
  std::cout << p << std::endl;

  getchar();
  return 0;
}
```

```c++
#include <cmath>

enum class PointType
{
  cartesian,
  polar
};

class Point
{
  // use a factory method
  Point(float x, float y) : x(x), y(y){}
public:
  float x, y;

  friend class PointFactory;
};

class PointFactory
{
  static Point NewCartesian(float x, float y)
  {
    return Point{ x,y };
  }

  static Point NewPolar(float r, float theta)
  {
    return Point{ r*cos(theta), r*sin(theta) };
  }
};

```

```c++
#include <cmath>

// do not need this for factory
enum class PointType
{
  cartesian,
  polar
};

class Point
{
  // use a factory method
  Point(float x, float y) : x(x), y(y) {}

  class PointFactory
  {
    PointFactory() {}
  public:
    static Point NewCartesian(float x, float y)
    {
      return { x,y };
    }
    static Point NewPolar(float r, float theta)
    {
      return{ r*cos(theta), r*sin(theta) };
    }
  };
public:
  float x, y;
  static PointFactory Factory;
};

int main()
{
  auto pp = Point::Factory.NewCartesian(2, 3);

  return 0;
}
```

**Situations to use Factory Pattern (Factory)**:

- **Simple Object Creation**: When you only need to create and immediately return an object.
- **No Multiple Steps Required**: The creation of the object doesn't need multiple steps or configuration options.
- **Runtime Decisions**: When you decide the type of object to create based on some logic or condition at runtime.
- **Emphasis on Subclassing**: The factory pattern allows subclasses to determine which class to instantiate. This provides a mechanism to create objects that are different but share the same interface or base class.
- **Hiding Object Creation Logic**: When you want to encapsulate the specific object creation logic, keeping it separate from client code.
    

**Situations to use Builder Pattern (Builder)**:

- **Complex Object Creation**: When the object to be created consists of multiple parts or has several configuration options.
- **Multi-step Object Creation**: The creation of the object involves multiple steps or needs to be assembled in a specific order.
- **Chaining Calls**: Builder pattern often allows for chained calls, such as `builder.setPartA().setPartB().build()`.
- **Clear Construction Process**: When you want a clear and explicit definition and control of the object construction process.
- **When an Object Requires Different Representations or Configurations**: The same construction process can yield different object representations.
- **Separation of Object Construction and Representation**: This allows the same building process to produce different representations.