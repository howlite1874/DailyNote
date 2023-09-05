```cpp
struct Foo {
    // Single parameter constructor, can be used as an implicit conversion.
    // Such a constructor is called "converting constructor".
    Foo(int x) {}
};
struct Faz {
    // Also a converting constructor.
    Faz(Foo foo) {}
};

// The parameter is of type Foo, not of type int, so it looks like
// we have to pass a Foo.
void bar(Foo foo);

int main() {
    // However, the converting constructor allows us to pass an int.
    bar(42);
    // Also allowed thanks to the converting constructor.
    Foo foo = 42;
    // Error! This would require two conversions (int -> Foo -> Faz).
    Faz faz = 42;
}
```

Prefixing the `explicit` keyword to the constructor prevents the compiler from using that constructor for implicit conversions. Adding it to the above class will create a compiler error at the function call `bar(42)`. It is now necessary to call for conversion explicitly with `bar(Foo(42))`

The reason you might want to do this is to avoid accidental construction that can hide bugs.  
Contrived example:

You have a `MyString` class with a constructor that constructs a string of the given size. You have a function `print(const MyString&)` (as well as an overload `print (char *string)`), and you call `print(3)` (when you _actually_ intended to call `print("3")`). You expect it to print "3", but it prints an empty string of length 3 instead.