
```c++
#include <iostream>
#include <string>
#include <vector>

// Forward declaration
class User;

class ChatRoomMediator {
public:
    virtual void showMessage(User* user, const std::string& message) = 0;
};

class User {
protected:
    ChatRoomMediator* mediator;
    std::string name;

public:
    User(ChatRoomMediator* med, const std::string& name) : mediator(med), name(name) {}

    const std::string& getName() const {
        return name;
    }

    void send(const std::string& message) {
        mediator->showMessage(this, message);
    }
};

//-----------------------------------------------------------------------------
class ChatRoom : public ChatRoomMediator {
private:
    std::vector<User*> users;

public:
    void addUser(User* user) {
        users.push_back(user);
    }

    void showMessage(User* user, const std::string& message) override {
        std::cout << user->getName() << ": " << message << std::endl;
        // You can implement more logic here, for example broadcasting to all users
    }
};

//--------------------------------------------------------------------------------
int main() {
    ChatRoom* chatroom = new ChatRoom();

    User* john = new User(chatroom, "John");
    User* jane = new User(chatroom, "Jane");

    chatroom->addUser(john);
    chatroom->addUser(jane);

    john->send("Hi Jane!");
    jane->send("Hello John!");

    delete jane;
    delete john;
    delete chatroom;

    return 0;
}

```


```c++
#include <iostream>
#include <QObject>
#include <string>

using namespace std;

class EventData
{
public:
    virtual ~EventData() = default;
    virtual void print() const = 0;
};

class PlayerScoredData : public EventData
{
public:
    string player_name;
    int goals_scored_so_far;

    PlayerScoredData(const string& player_name, const int goals_scored_so_far)
        : player_name(player_name), goals_scored_so_far(goals_scored_so_far) {}

    void print() const override
    {
        cout << player_name << " has scored! (their "
             << goals_scored_so_far << " goal)" << "\n";
    }
};

class Game : public QObject
{
    Q_OBJECT

public:
    void emitEvent(EventData* e)
    {
        emit eventOccurred(e);
    }

signals:
    void eventOccurred(EventData* e);
};

class Player : public QObject
{
    Q_OBJECT

public:
    string name;
    int goals_scored = 0;
    Game& game;

    Player(const string& name, Game& game)
        : QObject(&game), name(name), game(game) {}

    void score()
    {
        goals_scored++;
        PlayerScoredData ps{name, goals_scored};
        game.emitEvent(&ps);
    }
};

class Coach : public QObject
{
    Q_OBJECT

public:
    explicit Coach(Game& game) : QObject(&game)
    {
        connect(&game, &Game::eventOccurred, this, &Coach::onPlayerScored);
    }

public slots:
    void onPlayerScored(EventData* e)
    {
        PlayerScoredData* ps = dynamic_cast<PlayerScoredData*>(e);
        if (ps && ps->goals_scored_so_far < 3)
        {
            cout << "coach says: well done, " << ps->player_name << "\n";
        }
    }
};

int main(int argc, char** argv)
{
    Game game;
    Player player{ "Sam", game };
    Coach coach{ game };

    player.score();
    player.score();
    player.score(); // ignored by coach

    return 0;
}

```