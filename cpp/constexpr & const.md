```c++
#include <iostream>

int getNumber()
{
    std::cout << "Enter a number: ";
    int y{};
    std::cin >> y;

    return y;
}

int main()
{
    const int x{ 3 };           // x is a compile time constant

    const int y{ getNumber() }; // y is a runtime constant

    const int z{ x + y };       // x + y is a runtime expression
    std::cout << z << '\n';     // this is also a runtime expression

    return 0;
}
```

```c++
int x { 5 };       // not const at all
const int y { x }; // obviously a runtime const (since initializer is non-const)
const int z { 5 }; // obviously a compile-time const (since initializer is a constant expression)
const int w { getValue() }; // not obvious whether this is a runtime or compile-time const
```

```c++
#include <iostream>

int five()
{
    return 5;
}

int main()
{
    constexpr double gravity { 9.8 }; // ok: 9.8 is a constant expression
    constexpr int sum { 4 + 5 };      // ok: 4 + 5 is a constant expression
    constexpr int something { sum };  // ok: sum is a constant expression

    std::cout << "Enter your age: ";
    int age{};
    std::cin >> age;

    constexpr int myAge { age };      // compile error: age is not a constant expression
    constexpr int f { five() };       // compile error: return value of five() is not a constant expression

    return 0;
}
```

Any variable that should not be modifiable after initialization and whose initializer is known at compile-time should be declared as `constexpr`.  
Any variable that should not be modifiable after initialization and whose initializer is not known at compile-time should be declared as `const`.

```cpp
constexpr int x { 3 + 4 }; // 3 + 4 will always evaluate at compile time

int x { 3 + 4 }; // 3 + 4 may evaluate at compile-time or runtime

```




