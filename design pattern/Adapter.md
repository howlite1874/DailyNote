**Why use the Adapter Pattern?**

1. **Integration of Legacy Code**: Often in software development, there's a need to integrate old systems with newer systems. The Adapter pattern can help by acting as a middle-man to translate requests from the new system to method calls that the old system understands.
    
2. **Third-party Libraries**: When using a third-party library where you can't change the code, but it doesn't integrate well with your existing system, an Adapter can be written to bridge the gap.
    
3. **Flexibility**: Adapters can make your application more modular and flexible because it isolates the client code (which requires some service) from the implementation of that service. When changes occur in the implementation, it does not affect clients.
    
4. **Decoupling**: If two interfaces are tightly coupled, changes in one interface can lead to cascading changes in the other. An adapter can decouple these interfaces and make sure changes in one do not lead to changes in another.

```c++
class OldRectangle {
public:
    void draw(int x, int y, int w, int h) {
        // Drawing logic...
    }
};

class Shape {
public:
    virtual void draw() = 0;
};

class RectangleAdapter : public Shape {
    OldRectangle oldRectangle;
public:
    RectangleAdapter() {}

    void draw() override {
        oldRectangle.draw(0, 0, 10, 10);  // Just an example to convert the method call
    }
};

```


```c++
#include <gtest/gtest.h>
#include "../../../../../gtest/include/gtest/gtest.h"
#include "../../../../../gtest/include/gtest/internal/gtest-internal.h"
#include "../../../../../gtest/include/gtest/gtest_pred_impl.h"

struct Square
{
  int side{ 0 };


  explicit Square(const int side)
    : side(side)
  {
  }
};

struct Rectangle
{
  virtual int width() const = 0;
  virtual int height() const = 0;

  int area() const
  {
    return width() * height();
  }
};

struct SquareToRectangleAdapter : Rectangle
{
  SquareToRectangleAdapter(const Square& square)
    : square(square) {}
  
  int width() const override
  {
    return square.side;
  }

  int height() const override
  {
    return square.side;
  }

  const Square& square;
};

#include "gtest/gtest.h"

//#include "helpers/iohelper.h"

//#include "exercise.cpp"


namespace
{
  class Evaluate : public testing::Test
  {
  };

  TEST_F(Evaluate, SimpleTest)
  {
    Square sq{ 11 };
    SquareToRectangleAdapter adapter{ sq };
    ASSERT_EQ(121, adapter.area());
  }
}

int main(int ac, char* av[])
{
  testing::InitGoogleTest(&ac, av);
  return RUN_ALL_TESTS();
}
```

