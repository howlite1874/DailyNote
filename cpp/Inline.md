- > When should I write the keyword 'inline' for a function/method in C++?
    
    Only when you want the function to be defined in a header. More exactly only when the function's definition can show up in multiple translation units. It's a good idea to define small (as in one liner) functions in the header file as it gives the compiler more information to work with while optimizing your code. It also increases compilation time.
    
- > When should I not write the keyword 'inline' for a function/method in C++?
    
    Don't add inline just because you think your code will run faster if the compiler inlines it.