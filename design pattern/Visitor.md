
```c++
#include <iostream>

class Circle;
class Square;

class ShapeVisitor {
public:
    virtual void visitCircle(Circle* circle) = 0;
    virtual void visitSquare(Square* square) = 0;
};

class Shape {
public:
    virtual void accept(ShapeVisitor* visitor) = 0;
};

class Circle : public Shape {
public:
    double radius = 5.0;

    void accept(ShapeVisitor* visitor) override {
        visitor->visitCircle(this);
    }
};

class Square : public Shape {
public:
    double side = 4.0;

    void accept(ShapeVisitor* visitor) override {
        visitor->visitSquare(this);
    }
};

class AreaVisitor : public ShapeVisitor {
public:
    void visitCircle(Circle* circle) override {
        std::cout << "Circle area: " << 3.1415 * circle->radius * circle->radius << "\n";
    }

    void visitSquare(Square* square) override {
        std::cout << "Square area: " << square->side * square->side << "\n";
    }
};

int main() {
    Circle circle;
    Square square;

    AreaVisitor areaVisitor;

    circle.accept(&areaVisitor);
    square.accept(&areaVisitor);

    return 0;
}

```