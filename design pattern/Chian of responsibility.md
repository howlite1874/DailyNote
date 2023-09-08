```c++
#include <iostream>
#include <string>
using namespace std;

struct Creature
{
  string name;
  int attack, defense;

  Creature(const string& name, const int attack, const int defense)
    : name(name),
      attack(attack),
      defense(defense)
  {
  }


  friend ostream& operator<<(ostream& os, const Creature& obj)
  {
    return os
      << "name: " << obj.name
      << " attack: " << obj.attack
      << " defense: " << obj.defense;
  }
};

class CreatureModifier
{
  CreatureModifier* next{ nullptr }; // unique_ptr
protected:
  Creature& creature; // pointer or shared_ptr
public:
  explicit CreatureModifier(Creature& creature)
    : creature(creature)
  {
  }
  virtual ~CreatureModifier() = default;

  void add(CreatureModifier* cm)
  {
    if (next) next->add(cm);
    else next = cm;
  }

  // two approaches:

  // 1. Always call base handle(). There could be additional logic here.
  // 2. Only call base handle() when you cannot handle things yourself.

  virtual void handle()
  {
    if (next) next->handle();
  }
};

// 1. Double the creature's attack
// 2. Increase defense by 1 unless power > 2
// 3. No bonuses can be applied to this creature

class NoBonusesModifier : public CreatureModifier
{
public:
  explicit NoBonusesModifier(Creature& creature)
    : CreatureModifier(creature)
  {
  }

  void handle() override
  {
    // nothing
  }
};

class DoubleAttackModifier : public CreatureModifier
{
public:
  explicit DoubleAttackModifier(Creature& creature)
    : CreatureModifier(creature)
  {
  }

  void handle() override
  {
    creature.attack *= 2;
    CreatureModifier::handle();
  }
};

class IncreaseDefenseModifier : public CreatureModifier
{
public:
  explicit IncreaseDefenseModifier(Creature& creature)
    : CreatureModifier(creature)
  {
  }


  void handle() override
  {
    if (creature.attack <= 2)
      creature.defense += 1;
    CreatureModifier::handle();
  }
};

int main_()
{
  Creature goblin{ "Goblin", 1, 1 };
  CreatureModifier root{ goblin };
  DoubleAttackModifier r1{ goblin };
  DoubleAttackModifier r1_2{ goblin };
  IncreaseDefenseModifier r2{ goblin };
  //NoBonusesModifier nb{ goblin }; // effectively Command objects

  //root.add(&nb);
  root.add(&r1);
  root.add(&r1_2);
  root.add(&r2);

  root.handle(); // annoying

  cout << goblin << endl;

  //getchar();
  return 0;
}

```

```c++
#include <iostream>
#include <string>
#include <QCoreApplication>
#include <QObject>
#include <functional>

using namespace std;

struct Query
{
  string creature_name;
  enum Argument { attack, defense } argument;
  int result;

  Query(const string& creature_name, const Argument argument, const int result)
    : creature_name(creature_name),
      argument(argument),
      result(result)
  {
  }
};

class Game : public QObject // mediator
{
  Q_OBJECT
public:
  signals:
    void queries(Query&) const;
};

class Creature
{
  Game& game;
  int attack, defense;
public:
  string name;
  Creature(Game& game, const string& name, const int attack, const int defense)
    : game(game),
      attack(attack),
      defense(defense),
      name(name)
  {
  }

  int GetAttack() const
  {
    Query q{ name, Query::Argument::attack, attack };
    emit game.queries(q);
    return q.result;
  }

  friend ostream& operator<<(ostream& os, const Creature& obj)
  {
    return os
      << "name: " << obj.name
      << " attack: " << obj.GetAttack()
      << " defense: " << obj.defense;
  }
};

class CreatureModifier : public QObject
{
  Q_OBJECT
protected:
  Game& game;
  Creature& creature;
public:
  virtual ~CreatureModifier() = default;

  CreatureModifier(Game& game, Creature& creature)
    : QObject(&game), // parented to game for automatic memory management
      game(game),
      creature(creature)
  {
  }
};

class DoubleAttackModifier : public CreatureModifier
{
  Q_OBJECT
public:
  DoubleAttackModifier(Game& game, Creature& creature)
    : CreatureModifier(game, creature)
  {
    connect(&game, &Game::queries, this, [&](Query& q) {
      if (q.creature_name == creature.name && 
        q.argument == Query::Argument::attack)
        q.result *= 2;
    });
  }

  ~DoubleAttackModifier()
  {
    disconnect(&game, &Game::queries, this, nullptr); // Disconnect all signals from game to this object
  }
};

int main(int argc, char* argv[])
{
  QCoreApplication app(argc, argv);

  Game game;
  Creature goblin{ game, "Strong Goblin", 2, 2 };

  cout << goblin << endl;

  {
    DoubleAttackModifier dam{ game, goblin };

    cout << goblin << endl;
  }

  cout << goblin << endl;

  return 0;
}

```