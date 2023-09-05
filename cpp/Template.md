**Template Specialization**
```c++
// template specialization
#include <iostream>
using namespace std;

// class template:
template <class T>
class mycontainer {
    T element;
  public:
    mycontainer (T arg) {element=arg;}
    T increase () {return ++element;}
};

// class template specialization:
template <>
class mycontainer <char> {
    char element;
  public:
    mycontainer (char arg) {element=arg;}
    char uppercase ()
    {
      if ((element>='a')&&(element<='z'))
      element+='A'-'a';
      return element;
    }
};
```

It is also possible to set default values or types for class template parameters. For example, if the previous class template definition had been:  
  

|   |   |
|---|---|
|```<br>1<br>```|```<br>template <class T=char, int N=10> class mysequence {..};<br>```|
 