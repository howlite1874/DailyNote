Opt for piecewise construction so allow people to construct objects piece by piece.
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
