qt signal:
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