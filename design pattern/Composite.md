**What is the Composite Pattern?**

- **Leaf**: Represents individual objects of the composition.
- **Composite**: Represents nodes in the hierarchy which might have leaves or other composites as children.

**Why use the Composite Pattern?**

1. **Uniformity**: It allows you to treat individual objects (leaf nodes) and compositions of objects (sub-trees) uniformly. This is useful when you want to invoke a particular action on both singular and composite objects without distinguishing between them.
    
2. **Hierarchy Representation**: It provides a structure to represent part-whole hierarchies. This is particularly useful for hierarchies like graphic systems, file systems, and organizational structures.
    
3. **Simplifies Client Code**: Clients can treat complex compositions and individual objects uniformly (i.e., a single object or a group of objects can be treated in the same way).
    
4. **Add or Remove Components Easily**: You can easily add new components or remove existing ones. This makes the pattern highly flexible.
    
5. **Open/Closed Principle**: It's easier to add new kinds of components without changing existing code.
```c++
#pragma once
#include <iostream>
#include <vector>
#include <memory>

struct GraphicObject
{
  virtual void draw() = 0;
};

struct Circle : GraphicObject
{
  void draw() override
  {
    std::cout << "Circle" << std::endl;
  }
};

struct Group : GraphicObject
{
  std::string name;


  explicit Group(const std::string& name)
    : name{name}
  {
  }

  void draw() override
  {
    std::cout << "Group " << name.c_str() << " contains:" << std::endl;
    for (auto&& o : objects)
      o->draw();
  }

  std::vector<GraphicObject*> objects;
};

inline void graphics()
{
  Group root("root");
  Circle c1, c2;
  root.objects.push_back(&c1);

  Group subgroup("sub");
  subgroup.objects.push_back(&c2);

  root.objects.push_back(&subgroup);

  root.draw();
}
```


```c++
#include "Headers.hpp"

class Creature
{
  enum Abilities { str, agl, intl, count };
  array<int, count> abilities;
public:
  int get_strength() const { return abilities[str]; }
  void set_strength(int value) { abilities[str] = value; }

  int get_agility() const { return abilities[agl]; }
  void set_agility(int value) { abilities[agl] = value; }

  int get_intelligence() const { return abilities[intl]; }
  void set_intelligence(int value) { abilities[intl] = value; }

  int sum() const {
    return accumulate(abilities.begin(), abilities.end(), 0);
  }

  double average() const {
    return sum() / (double)count;
  }

  int max() const {
    return *max_element(abilities.begin(), abilities.end());
  }
};

int main(int ac, char* av[])
{
  Creature orc;
  orc.set_strength(16);
  orc.set_agility(11);
  orc.set_intelligence(9);

  cout << "The orc has an average stat of "
       << orc.average()
       << " and a maximum stat of "
       << orc.max()
       << "\n";

  return 0;
}
```