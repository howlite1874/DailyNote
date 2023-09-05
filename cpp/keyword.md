#### Unsigned & Signed
**Signed variables**, such as signed integers will _allow you to represent numbers both in the positive and negative ranges_.

**Unsigned variables**, such as unsigned integers, will _only allow you to represent numbers in the positive and zero_.

Unsigned and signed variables of the same type (such as `int` and `byte`) both have the same range (range of 65,536 and 256 numbers, respectively), but **unsigned can represent a larger magnitude number than the corresponding signed variable**.

For example, an `unsigned byte` can represent values from `0` to `255`, while `signed byte` can represent `-128` to `127`.

```c++
#include <iostream>

int main()
{
    unsigned short x{ 65535 }; // largest 16-bit unsigned value possible
    std::cout << "x was: " << x << '\n';

    x = 65536; // 65536 is out of our range, so we get modulo wrap-around
    std::cout << "x is now: " << x << '\n';

    x = 65537; // 65537 is out of our range, so we get modulo wrap-around
    std::cout << "x is now: " << x << '\n';

    return 0;
}
```

```
x was: 65535
x is now: 0
x is now: 1
```


```c++
#include <iostream>

int main()
{
    unsigned short x{ 0 }; // smallest 2-byte unsigned value possible
    std::cout << "x was: " << x << '\n';

    x = -1; // -1 is out of our range, so we get modulo wrap-around
    std::cout << "x is now: " << x << '\n';

    x = -2; // -2 is out of our range, so we get modulo wrap-around
    std::cout << "x is now: " << x << '\n';

    return 0;
}
```

```
x was: 0
x is now: 65535
x is now: 65534
```


```c++
#include <iostream>

// int is 4 bytes
int main()
{
    signed int s { -1 };
    unsigned int u { 1 };

    if (s < u) // -1 is implicitly converted to 4294967295, and 4294967295 < 1 is false
        std::cout << "-1 is less than 1\n";
    else
        std::cout << "1 is less than -1\n"; // this statement executes

    return 0;
}
```

```
1 is less than -1
```

### When to use
First, unsigned numbers are preferred when dealing with bit manipulation (covered in chapter O -- that’s a capital ‘o’, not a ‘0’). They are also useful when well-defined wrap-around behavior is required (useful in some algorithms like encryption and random number generation).

Second, use of unsigned numbers is still unavoidable in some cases, mainly those having to do with array indexing. We’ll talk more about this in the lessons on arrays and array indexing.

Also note that if you’re developing for an embedded system (e.g. an Arduino) or some other processor/memory limited context, use of unsigned numbers is more common and accepted (and in some cases, unavoidable) for performance reasons.


#### Char

```c++
char ch2{ 'a' }; 
// initialize with code point for 'a' (stored as integer 97) (preferred)

char ch1{ 97 }; 
// initialize with integer 97 ('a') (not preferred)

char ch3{5}; 
// initialize with integer 5 (stored as integer 5)  

char ch4{'5'}; 
// initialize with code point for '5' (stored as integer 53)

```

```
"The width of wchar_t is compiler-specific and can be as small as 8 bits. Consequently, programs that need to be portable across any C and C++ compiler should not use wchar_t for storing Unicode text. The wchar_t type is intended for storing compiler-defined wide characters, which may be Unicode characters in some compilers."
```

`wchar_t` type is defined as a wide character type. Unfortunately, its size that depends on the compiler.
`char16_t` is a type for UTF-16 character representation, required to be large enough to represent any UTF-16 code unit (16-bit).char32_t is a type for UTF-32 character representation, required to be large enough to represent any UTF-32 code unit (32 bits).


#### integer

|type   |  Byte Size | Meaning  |
|---|---|---|
|short|2|Used for small integers. Range: -32768 to 32767|
|long|at least 4|Used for large integers. Equivalent to long int .|
|unsigned long|4|Used for large positive integers or 0. Equivalent to unsigned long int .|
|long long|8|Used for very large integers. Equivalent to long long int .|


#### Enum & Enum Class
1.  **Scope**:
    
    -   **enum**: The enumeration values are in the scope that contains the enum. This means there's a potential for name clashes.

    ```c++
    enum Color { RED, GREEN, BLUE }; 
    // RED, GREEN, BLUE are all accessible here.
	```
       
        
    -   **enum class**: The enumeration values are scoped within the enum class. This avoids potential naming conflicts.
        

	 ```c++
	   enum class Color { RED, GREEN, BLUE }; 
	   // To access, you'd need Color::RED, Color::GREEN, Color::BLUE.
	```
        
2.  **Implicit Conversion**:
    
    -   **enum**: Can be implicitly converted to an integer.
        
        cppCopy code
    ```c++
        enum Color { RED, GREEN, BLUE }; 
        int myColor = RED;  
        // This is valid.
	```

        
    -   **enum class**: Cannot be implicitly converted to an integer. Explicit casting is required.
        
        cppCopy code
    ```c++
    enum class Color { RED, GREEN, BLUE };
    int myColor = static_cast<int>(Color::RED);
    // Explicit casting needed.
	```
       
        
3.  **Underlying Type**:
    
    -   **enum**: By default, the underlying type might be `int`, but it's not guaranteed. You cannot directly specify the underlying type for it.
    -   **enum class**: You can specify an underlying type for it.
        
    ```c++
    enum class Color : char { RED, GREEN, BLUE };
      // Using char as the underlying type.
	```
        
        
4.  **Strong Typing**:
    -   **enum**: Not strongly typed. You might inadvertently assign a value of one enum type to a variable of another enum type.
    -   **enum class**: Strongly typed. You're not allowed to assign a value of one enum class to a variable of another enum class.


#### Float & Double
|type   |  Minimum Size | Typical Size  |
|---|---|---|
|float|4|4|
|double|8|8|
|long double|4|8, 12, or 16 bytes|

```c++
#include <iostream>

int main()
{
    double zero {0.0};
    double posinf { 5.0 / zero }; // positive infinity
    std::cout << posinf << '\n';

    double neginf { -5.0 / zero }; // negative infinity
    std::cout << neginf << '\n';

    double nan { zero / zero }; // not a number (mathematically invalid)
    std::cout << nan << '\n';

    return 0;
}
```

```
1.#INF
-1.#INF
1.#IND
```


####  auto ＆ decltype
```c++
template <typename T, typename U>
auto sum( T t, U u ) -> decltype(t+u)
```


#### Union
**The purpose of union is _to save memory_ by using the same memory region for storing different objects at different times.**
```c++
#include <cstdint>

 
union S
{
    std::int32_t n;     // occupies 4 bytes
    std::uint16_t s[2]; // occupies 4 bytes
    std::uint8_t c;     // occupies 1 byte
};                      // the whole union occupies 4 bytes
```


```c++
#include <iostream>
 
// S has one non-static data member (tag), three enumerator members (CHAR, INT, DOUBLE), 
// and three variant members (c, i, d)
struct S
{
    enum{CHAR, INT, DOUBLE} tag;
    union
    {
        char c;
        int i;
        double d;
    };
};
 
void print_s(const S& s)
{
    switch(s.tag)
    {
        case S::CHAR: std::cout << s.c << '\n'; break;
        case S::INT: std::cout << s.i << '\n'; break;
        case S::DOUBLE: std::cout << s.d << '\n'; break;
    }
}
 
int main()
{
    S s = {S::CHAR, 'a'};
    print_s(s);
    s.tag = S::INT;
    s.i = 123;
    print_s(s);
}
```

```
a
123
```


#### Cast

- **dynamic_cast**
`dynamic_cast` can be used only with pointers and references to objects. Its purpose is to ensure that the result of the type conversion is a valid complete object of the requested class.  
Therefore, `dynamic_cast` is always successful when we `cast a class to one of its base classes`
```c++
class CBase { };
class CDerived: public CBase { };

CBase b; CBase* pb;
CDerived d; CDerived* pd;

pb = dynamic_cast<CBase*>(&d);     // ok: derived-to-base
pd = dynamic_cast<CDerived*>(&b);  // wrong: base-to-derived 
```

When a class is polymorphic, dynamic_cast performs a special checking during runtime to ensure that the expression yields a valid complete object of the requested class:

```c++
// dynamic_cast
#include <iostream>
#include <exception>
using namespace std;

class CBase { virtual void dummy() {} };
class CDerived: public CBase { int a; };

int main () {
  try {
    CBase * pba = new CDerived;
    CBase * pbb = new CBase;
    CDerived * pd;

    pd = dynamic_cast<CDerived*>(pba);
    if (pd==0) cout << "Null pointer on first type-cast" << endl;

    pd = dynamic_cast<CDerived*>(pbb);
    if (pd==0) cout << "Null pointer on second type-cast" << endl;

  } catch (exception& e) {cout << "Exception: " << e.what();}
  return 0;
}
```

- **static_cast**
`static_cast` can perform conversions between pointers to related classes, not only from the derived class to its base, but also from a base class to its derived. This ensures that at least the classes are compatible if the proper object is converted, but `no safety check` is performed during runtime to check if the object being converted is in fact a full object of the destination type. Therefore, it is` up to the programmer to ensure that the conversion is safe`. On the other side, the overhead of the type-safety checks of dynamic_cast is avoided.

- **reinterpret_cast**
`reinterpret_cast` converts any pointer type to any other pointer type, even of unrelated classes. The operation result is a simple binary copy of the value from one pointer to the other. All pointer conversions are allowed: neither the content pointed nor the pointer type itself is checked.  
  
It can also `cast pointers to or from integer types`. The format in which this integer value represents a pointer is platform-specific. The only guarantee is that a pointer cast to an integer type large enough to fully contain it, is granted to be able to be cast back to a valid pointer.

```c++
class A {};
class B {};
A * a = new A;
B * b = reinterpret_cast<B*>(a);
```

- **const_cast**
This type of casting manipulates the `constness` of an object, either to `be set or to be removed`. For example, in order to pass a const argument to a function that expects a non-constant parameter:
```c++
// const_cast
#include <iostream>
using namespace std;

void print (char * str)
{
  cout << str << endl;
}

int main () {
  const char * c = "sample text";
  print ( const_cast<char *> (c) );
  return 0;
}
```

### typeid & decltype
```c++
#include <iostream>
#include <typeinfo>

class Base {
public:
    virtual ~Base() {}  // 使得该类成为多态
};

class DerivedA : public Base {
};

class DerivedB : public Base {
};

void identify(Base& obj) {
    if (typeid(obj) == typeid(DerivedA)) {
        std::cout << "Object is of type DerivedA\n";
    } else if (typeid(obj) == typeid(DerivedB)) {
        std::cout << "Object is of type DerivedB\n";
    } else {
        std::cout << "Object is of unknown type\n";
    }
}

int main() {
    DerivedA a;
    DerivedB b;
    identify(a);  // 输出: Object is of type DerivedA
    identify(b);  // 输出: Object is of type DerivedB
}

```

