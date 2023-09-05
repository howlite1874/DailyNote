In C++, `new` and `delete` are operators used for dynamic memory allocation and deallocation.

1. **`new`**: When you use the `new` operator, it allocates memory for an object in the dynamic storage duration (typically in the heap) and returns the address of that object. In addition to memory allocation, `new` also calls the object's constructor.
    
    For example:
    
    `int* p = new int(5);  // Allocates memory for an int and initializes it to 5`
    
2. **`delete`**: The `delete` operator is used to release the memory previously allocated by `new`. Before freeing the memory, if there's an object at that memory address, `delete` will call its destructor.
    
    Continuing from the above example:
    
    `delete p;  // Releases the memory pointed to by p`
    
3. **Important Notes**:
    - If you allocate memory using `new`, you are responsible for releasing it at an appropriate time with `delete`. Failing to do so can result in a memory leak.
    - Memory that has been released using `delete` should not be accessed again as it might lead to `undefined behavior`.
    - For every memory allocation with `new`, ensure that you use `delete` only once to release it.
    - For arrays, you should use `new[]` for allocation and `delete[]` for deallocation.

```c++
int* arr = new int[10];  // Allocates memory for an array of 10 
ints delete[] arr;  // Releases the memory for the array
```