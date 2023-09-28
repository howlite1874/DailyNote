```c++
#ifndef RDX_THREAD_POOL_HPP
#define RDX_THREAD_POOL_HPP
#include <cstddef>
#include <future>
#include <queue>
#include <mutex>
#include <thread>
#include <condition_variable>
namespace rdx {

    /*
        /brief A thread pool that spawns a specified number of workers.

        Worker threads wait for the queue to be filled with tasks.
        Enqueueing tasks return futures so that they can return asynchronous values.
        Destructor will join all workers.
    */
    class thread_pool {
    private:
        std::queue<std::function<void()>> task_queue;
        std::mutex queue_mutex;
        std::condition_variable queue_notification;
        std::vector<std::thread> workers;
        bool should_stop;
        void worker_function();
    public:
        // constructs a thread pool with the given number of workers
        thread_pool(std::size_t num_workers = std::thread::hardware_concurrency());
        ~thread_pool();
        //enqueues a task to be performed and returns a future for that task
        template<class F, class... Args>
        std::future<typename std::result_of<F(Args...)>::type> enqueue(F&& f, Args&&... args);

        thread_pool(thread_pool&&) = delete;
        thread_pool(const thread_pool&) = delete;
        thread_pool& operator=(thread_pool&&) = delete;
        thread_pool& operator=(const thread_pool&) = delete;
    };

    template<class F, class ...Args>
    inline std::future<typename std::result_of<F(Args ...)>::type> thread_pool::enqueue(F&& f, Args && ...args) {
        using return_type = typename std::result_of<F(Args...)>::type;
        auto task = std::make_shared<std::packaged_task<return_type()>>(std::bind(std::forward<F>(f), std::forward<Args>(args)...));
        std::future<return_type> res = task->get_future();
        {
            std::unique_lock<std::mutex> lock(queue_mutex);
            task_queue.emplace([task]() { (*task)(); });
        }
        queue_notification.notify_one();
        return res;
    }

}
#endif // !RDX_THREAD_POOL_H

```

thread_pool.cpp

```cpp
#include "thread_pool.hpp"

namespace rdx {
    void thread_pool::worker_function() {
        while (true) {
            std::function<void()> task;
            {
                std::unique_lock<std::mutex> lock(queue_mutex);
                queue_notification.wait(lock, [this]() {
                    return should_stop || !task_queue.empty();
                });
                if (should_stop && task_queue.empty()) {
                    break;
                }
                if (task_queue.empty()) {
                    continue;
                }
                task = task_queue.front();
                task_queue.pop();
            }
            task();
        }
    }

    thread_pool::thread_pool(std::size_t num_workers) : should_stop(false) {
        for (auto i = 0; i < num_workers; i++) {
            workers.emplace_back(&thread_pool::worker_function,this);
        }
    }

    thread_pool::~thread_pool() {
        {
            std::unique_lock<std::mutex> lock(queue_mutex);
            should_stop = true;
        }
        queue_notification.notify_all();
        for (auto& worker : workers) {
            worker.join();
        }
    }
}
 
```
```