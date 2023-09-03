## weak_ptr
1.**Transient Observers**
```c++
class NewsPublisher;

class NewsReader {
public:
    NewsReader(std::shared_ptr<NewsPublisher> publisher) : publisher_(publisher) {}

    void checkNews() {
        if (auto sharedPublisher = publisher_.lock()) { 
            //transfer weak_ptr to shared_ptr
        } else {
            
        }
    }

private:
    std::weak_ptr<NewsPublisher> publisher_;
};

```

2.**Cache Patterns**
```c++
class Resource {};

class ResourceCache {
public:
    std::shared_ptr<Resource> getResource(int id) {
        auto resource = cache_[id].lock();
        if (!resource) {
            resource = std::make_shared<Resource>();
            cache_[id] = resource;
        }
        return resource;
    }

private:
    std::unordered_map<int, std::weak_ptr<Resource>> cache_;
};

```

3.**Delayed Initialization**
```c++
class LazyResourceFactory {
public:
    std::shared_ptr<Resource> getResource() {
        if (auto resource = resource_.lock()) {
            return resource;
        } else {
            auto newResource = std::make_shared<Resource>();
            resource_ = newResource;
            return newResource;
        }
    }

private:
    std::weak_ptr<Resource> resource_;
};

```

4.**Breaking Cycles in Reference Counting**
```c++
class Child;

class Parent {
public:
    std::shared_ptr<Child> child;
};

class Child {
public:
    std::weak_ptr<Parent> parent;  // Use weak_ptr to break the cycle
};

```

## shared_ptr & unique_ptr

 **`std::unique_ptr`**:
    - **Overhead**: `std::unique_ptr` has almost no overhead. It doesn't need to maintain a reference count because it ensures that only one `std::unique_ptr` points to a specific object at any time.
    - **Ownership**: The ownership is explicit. The object pointed to will be destroyed when the `std::unique_ptr` goes out of scope or is reset.
    - **Transferability**: You can transfer its ownership but can't copy it.
    - **Use Case**: `std::unique_ptr` should be your first choice when you know an object will be managed by a single owner and there's no need to share ownership.

```c++
#include <iostream>
#include <memory>

int main() {
    std::unique_ptr<int> ptr1 = std::make_unique<int>(5);

    std::unique_ptr<int> ptr2 = std::move(ptr1);

    if (!ptr1) {
        std::cout << "ptr1 is empty" << std::endl;
    }

    std::cout << "*ptr2 = " << *ptr2 << std::endl;

    return 0;
}

```

 **`std::shared_ptr`**:
    - **Overhead**: `std::shared_ptr` comes with extra overhead because it maintains a reference count. Every time a new `std::shared_ptr` points to an object or an existing one is destroyed, this count changes.
    - **Ownership**: Multiple `std::shared_ptr`s can share ownership of the same object. The object will be destroyed only when the last `std::shared_ptr` pointing to it is destroyed.
    - **Use Case**: You should use `std::shared_ptr` when an object needs to be co-owned by multiple entities or when you need to utilize `std::weak_ptr` to avoid circular references.

