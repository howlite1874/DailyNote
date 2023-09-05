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