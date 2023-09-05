```c++
// C++ Program to Demonstrate the Concept of Dynamic binding
// with the help of virtual function
#include <iostream>
using namespace std;

class GFG {
public:
	void call_Function() // function that call print
	{
		print();
	}
	void print() // the display function
	{
		cout << "Printing the Base class Content" << endl;
	}
};
class GFG2 : public GFG // GFG2 inherit a publicly
{
public:
	void print() // GFG2's display
	{
		cout << "Printing the Derived class Content"
			<< endl;
	}
};
int main()
{
	GFG geeksforgeeks; // Creating GFG's pbject
	geeksforgeeks.call_Function(); // Calling call_Function
	GFG2 geeksforgeeks2; // creating GFG2 object
	geeksforgeeks2.call_Function(); // calling call_Function
									// for GFG2 object
	return 0;
}

```

```
Printing the Base class Content
Printing the Base class Content
```
As we can see, the print() function of the parent class is called even from the derived class object. To resolve this we use virtual functions.

### Derive
1. **Deconstructor process**: first call the constructor of the derived class, then call the constructor of the base class. 2.
2. **Constructor**: first call the constructor of the base class, then call the constructor of the derived class.