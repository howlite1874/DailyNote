The Facade Pattern is a structural design pattern that provides a simplified interface to a larger body of code, such as a library, a framework, or a complex system of classes. It hides the complexities of the larger system and provides a simpler, more user-friendly interface.
```c++
class Light {
public:
    void on() { cout << "Light is ON" << endl; }
    void off() { cout << "Light is OFF" << endl; }
};

class AirCondition {
public:
    void on() { cout << "AirCondition is ON" << endl; }
    void off() { cout << "AirCondition is OFF" << endl; }
};

class AudioSystem {
public:
    void on() { cout << "AudioSystem is ON" << endl; }
    void off() { cout << "AudioSystem is OFF" << endl; }
};

class HomeFacade {
private:
    Light light;
    AirCondition ac;
    AudioSystem audio;

public:
    void leaveHome() {
        light.off();
        ac.off();
        audio.off();
    }

    void arriveHome() {
        light.on();
        ac.on();
        audio.on();
    }
};

int main() {
    HomeFacade home;
    home.arriveHome();  // Turns on everything
    home.leaveHome();   // Turns off everything
}

```
