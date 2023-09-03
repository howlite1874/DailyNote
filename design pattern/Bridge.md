**Key Components:**

1. **Abstraction**: The high-level part of a two-part hierarchy. It maintains a reference to the object of the `Implementor`.
2. **Implementor**: This is the interface for the low-level, or platform, part of the hierarchy.

**Why use the Bridge Pattern?**

1. **Decoupling Interface and Implementation**: If a class changes, it might also require its subclasses to change. By separating the interface from the implementation, you're allowing them to vary independently.
    
2. **Extension**: Both the abstraction and its implementation can be extended independently.
    
3. **Sharing**: An implementation can be shared across multiple objects.
    
4. **Dynamic Binding**: You can switch implementations at run-time.
    
5. **Hide Implementation Details from Client**: The client code doesn't get tangled with the implementation details.
```c++
#include <iostream>
#include <string>
#include <vector>
#include "Person.h"
using namespace std;

// two classes of objects

// Renderers - determine how an object is drawn
// Shapes - determine what to draw

struct Renderer
{
  virtual void render_circle(float x, float y, float radius) = 0;
};

struct VectorRenderer : Renderer
{
  void render_circle(float x, float y, float radius) override
  {
    cout << "Rasterizing circle of radius " << radius << endl;
  }
};

struct RasterRenderer : Renderer
{
  void render_circle(float x, float y, float radius) override
  {
    cout << "Drawing a vector circle of radius " << radius << endl;
  }
};

struct Shape
{
protected:
  Renderer& renderer;
  Shape(Renderer& renderer) : renderer{ renderer } {}
public:
  virtual void draw() = 0; // implementation specific
  virtual void resize(float factor) = 0; // abstraction specific
};

struct Circle : Shape
{
  float x, y, radius;

  void draw() override
  {
    renderer.render_circle(x, y, radius);
  }

  void resize(float factor) override
  {
    radius *= factor;
  }

  Circle(Renderer& renderer, float x, float y, float radius)
    : Shape{renderer},
      x{x},
      y{y},
      radius{radius}
  {
  }
};

void bridge()
{
  RasterRenderer rr;
  Circle raster_circle{ rr, 5,5,5 };
  raster_circle.draw();
  raster_circle.resize(2);
  raster_circle.draw();
}

int main()
{
  // pimpl
  // binary interfaces are fragile; this removes most of the internals to a separate class
  // prevents recompilation of sources reliant on the header

  Person p;
  p.greet();

  getchar();
  return 0;
}

```
**Key Components:**

1. **Abstraction**: The high-level part of a two-part hierarchy. It maintains a reference to the object of the `Implementor`.
2. **Implementor**: This is the interface for the low-level, or platform, part of the hierarchy.

**Why use the Bridge Pattern?**

1. **Decoupling Interface and Implementation**: If a class changes, it might also require its subclasses to change. By separating the interface from the implementation, you're allowing them to vary independently.
    
2. **Extension**: Both the abstraction and its implementation can be extended independently.
    
3. **Sharing**: An implementation can be shared across multiple objects.
    
4. **Dynamic Binding**: You can switch implementations at run-time.
    
5. **Hide Implementation Details from Client**: The client code doesn't get tangled with the implementation details.