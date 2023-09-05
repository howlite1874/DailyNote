
```c++
#include <iostream>
#include <string>
#include <cstdint>
using namespace std;

#include <boost/bimap.hpp>
#include <boost/flyweight.hpp>
#include <boost/flyweight/key_value.hpp>
using namespace boost;
using namespace flyweights;

// coloring in the console by-letter vs using ranges

// boost.flyweight  

// naive
typedef uint32_t key;

// mmorpg
struct User
{
  User(const string& first_name, const string& last_name)
    : first_name{add(first_name)}, last_name{add(last_name)}
  {
  }

  const string& get_first_name() const
  {
    return names.left.find(last_name)->second;
  }

  const string& get_last_name() const
  {
    return names.left.find(last_name)->second;
  }

  static void info()
  {
    for (auto entry : names.left)
    {
      cout << "Key: " << entry.first << ", Value: " << entry.second << endl;
    }
  }

  friend ostream& operator<<(ostream& os, const User& obj)
  {
    return os
      << "first_name: " << obj.first_name << " " << obj.get_first_name()
      << " last_name: " << obj.last_name << " " << obj.get_last_name();
  }

protected:
  static bimap<key, string> names;
  static int seed;

  static key add(const string& s)
  {
    auto it = names.right.find(s);
    if (it == names.right.end())
    {
      // add it
      key id = ++seed;
      names.insert(bimap<key, string>::value_type(seed, s));
      return id;
    }
    return it->second;
  }
  key first_name, last_name;
};

int User::seed = 0;
bimap<key, string> User::names{};

void naive_flyweight()
{
  User john_doe{ "John", "Doe" };
  User jane_doe{ "Jane", "Doe" };

  cout << "John " << john_doe << endl;
  cout << "Jane " << jane_doe << endl;

  User::info();
}

struct User2
{
  // users share names! e.g., John Smith
  flyweight<string> first_name, last_name;
  //string first_name, last_name;
  // ...

  User2(const string& first_name, const string& last_name)
  {
  }
};

void boost_flyweight()
{
  User2 john_doe{ "John", "Doe" };
  User2 jane_doe{ "Jane", "Doe" };

  
  cout << boolalpha <<  (&jane_doe.last_name.get() == &john_doe.last_name.get());
  //cout << (&jane_doe.last_name == &john_doe.last_name);
}

int main_()
{
  naive_flyweight();
  boost_flyweight();

  getchar();
  return 0;
}

```


```c++
#include <iostream>
#include <string>
#include <ostream>
#include <vector>
using namespace std;

class FormattedText
{
  string plain_text;
  bool *caps;
public:
  explicit FormattedText(const string& plainText)
    : plain_text{plainText}
  {
    caps = new bool[plainText.length()];
  }
  ~FormattedText()
  {
    delete[] caps;
  }
  void capitalize(int start, int end)
  {
    for (int i = start; i <= end; ++i)
      caps[i] = true;
  }
  
  friend std::ostream& operator<<(std::ostream& os, const FormattedText& obj)
  {
    string s;
    for (int i = 0; i < obj.plain_text.length(); ++i)
    {
      char c = obj.plain_text[i];
      s += (obj.caps[i] ? toupper(c) : c);
    }
    return os << s;
  }
};

class BetterFormattedText
{
public:
  struct TextRange
  {
    int start, end;
    bool capitalize, bold, italic;

    bool covers(int position) const
    {
      return position >= start && position <= end;
    }
  };

  TextRange& get_range(int start, int end)
  {
    formatting.emplace_back(TextRange{ start, end });
    return *formatting.rbegin();
  }

  explicit BetterFormattedText(const string& plainText)
    : plain_text{plainText}
  {
  }

  friend std::ostream& operator<<(std::ostream& os, const BetterFormattedText& obj)
  {
    string s;
    for (size_t i = 0; i < obj.plain_text.length(); i++)
    {
      auto c = obj.plain_text[i];
      for (const auto& rng : obj.formatting)
      {
        if (rng.covers(i) && rng.capitalize)
          c = toupper(c);
        s += c;
      }
    }
    return os << s;
  }

private:
  string plain_text;
  vector<TextRange> formatting;
};

int main(int argc, char* argv[])
{
  FormattedText ft("This is a brave new world");
  ft.capitalize(10, 15);
  cout << ft << endl;

  BetterFormattedText bft("This is a brave new world");
  bft.get_range(10, 15).capitalize = true;
  cout << bft << endl;
}

```
