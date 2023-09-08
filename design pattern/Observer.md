Observer defines a one-to-many dependency relationship between objects, so that when the state of an object changes, all objects that depend on it will be notified and automatically updated.
qt signal:
```c++
#include <vector>
#include <iostream>

class Observer {
public:
    virtual void update(float temperature, float humidity, float pressure) = 0;
};

class Subject {
public:
    virtual void registerObserver(Observer* o) = 0;
    virtual void removeObserver(Observer* o) = 0;
    virtual void notifyObservers() = 0;
};

//-------------------------------subject---------------------------------------

class WeatherData : public Subject {
private:
    std::vector<Observer*> observers;
    float temperature;
    float humidity;
    float pressure;

public:
    void registerObserver(Observer* o) override {
        observers.push_back(o);
    }

    void removeObserver(Observer* o) override {
        observers.erase(std::remove(observers.begin(), observers.end(), o), observers.end());
    }

    void notifyObservers() override {
        for (Observer* observer : observers) {
            observer->update(temperature, humidity, pressure);
        }
    }

    void measurementsChanged() {
        notifyObservers();
    }

    void setMeasurements(float temperature, float humidity, float pressure) {
        this->temperature = temperature;
        this->humidity = humidity;
        this->pressure = pressure;
        measurementsChanged();
    }
};

//------------------------------------observer------------------------------------
class CurrentConditionsDisplay : public Observer {
private:
    float temperature;
    float humidity;
    Subject* weatherData;

public:
    CurrentConditionsDisplay(Subject* weatherData) : weatherData(weatherData) {
        this->weatherData->registerObserver(this);
    }

    void update(float temperature, float humidity, float pressure) override {
        this->temperature = temperature;
        this->humidity = humidity;
        display();
    }

    void display() {
        std::cout << "Current conditions: " << temperature << "C degrees and " << humidity << "% humidity" << std::endl;
    }
};

//------------------------------------implement------------------------------------
int main() {
    WeatherData* weatherData = new WeatherData();
    CurrentConditionsDisplay* currentDisplay = new CurrentConditionsDisplay(weatherData);

    weatherData->setMeasurements(28.5, 65, 1013);
    weatherData->setMeasurements(25.5, 70, 1010);
    weatherData->setMeasurements(30.0, 55, 1015);

    delete currentDisplay;
    delete weatherData;

    return 0;
}

```

```c++
#include <QObject>
#include <QString>


// Observable (Subject) class
class Observable : public QObject
{
    Q_OBJECT

private:
    QString data;

public:
    Observable(QObject *parent = nullptr) : QObject(parent) {}

    void setData(const QString &newData)
    {
        if(data != newData)
        {
            data = newData;
            emit dataChanged(data);
        }
    }

signals:
    void dataChanged(const QString &newData);
};

// Observer class
class Observer : public QObject
{
    Q_OBJECT

public:
    Observer(QObject *parent = nullptr) : QObject(parent) {}

public slots:
    void onDataChanged(const QString &newData)
    {
        // Do something when the data changes
        qDebug() << "Data has changed to:" << newData;
    }
};

// Example usage:
// Observable observable;
// Observer observer;
// QObject::connect(&observable, &Observable::dataChanged, &observer, &Observer::onDataChanged);
// observable.setData("New Data");

```