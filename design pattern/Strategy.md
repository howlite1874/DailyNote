dynamic strategy
```c++
#include <iostream>
#include <functional>

class Context {
public:
    void setStrategy(std::function<void()> strategy) {
        this->strategy = strategy;
    }
    
    void executeStrategy() {
        strategy();
    }
private:
    std::function<void()> strategy;
};

int main() {
    Context context;

    auto strategyA = []() { std::cout << "Strategy A\n"; };
    auto strategyB = []() { std::cout << "Strategy B\n"; };

    context.setStrategy(strategyA);
    context.executeStrategy();

    context.setStrategy(strategyB);
    context.executeStrategy();

    return 0;
}

```


static strategy:
```c++
#include <iostream>

class StrategyA {
public:
    void execute() const {
        std::cout << "Executing strategy A\n";
    }
};

class StrategyB {
public:
    void execute() const {
        std::cout << "Executing strategy B\n";
    }
};

template <typename Strategy>
class Context {
private:
    Strategy strategy;
public:
    void executeStrategy() const {
        strategy.execute();
    }
};

int main() {
    Context<StrategyA> contextA;
    Context<StrategyB> contextB;

    contextA.executeStrategy();
    contextB.executeStrategy();

    return 0;
}

```