- Bring a specific member from the namespace into the current scope.
 ```c++
 #include <iostream>

int main() {

  using std::string;
  using std::cout;
  string s = "Hello World";
  cout << s;
  return 0;

}
```

- Bring all members from the namespace into​ the current scope.
```c++
#include <iostream>

using namespace std;

int main() {
  cout << "Hello World";
  return 0;

}
```

- Bring a base class method ​or variable into the current class’s scope.
```c++
#include <iostream>

using namespace std;
class Base {
   public:
      void greet() {
         cout << "Hello from Base" << endl;;
      }

};

class Derived : Base {
   public:
      using Base::greet;
      void greet(string s) {
         cout << "Hello from " << s << endl;
         // Instead of recursing, the greet() method
         // of the base class is called.
         greet();
      }

};

int main() {
   Derived D;
   D.greet("Derived");
   return 0;
}
```
