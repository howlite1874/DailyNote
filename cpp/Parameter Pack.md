**Unary folds:**

```c++
template<typename... Args>
auto sum(Args... args) {
    return (... + args); // Unary right fold
}

sum(1, 2, 3, 4); // Output: 10
```

**Binary folds:**

```c++
template<typename... Args1, typename... Args2>
auto addPairs(std::tuple<Args1...> t1, std::tuple<Args2...> t2) {
    return std::tuple( (std::get<Args1>(t1) + std::get<Args2>(t2))... );
}
```

**Folds with Initializers:**

```c++
template<typename... Args>
auto concatWithSpaces(const Args&... args) {
    return (std::string() + ... + args); // Produces concatenated strings without spaces
}

concatWithSpaces("Hello", "world!"); // Output: "Helloworld!"
```

std::string() ----> ""

**perfect forward**
```c++
template<typename Func, typename... Args>
auto wrapper(Func&& func, Args&&... args) -> decltype(auto) {
    return func(std::forward<Args>(args)...);
}
```