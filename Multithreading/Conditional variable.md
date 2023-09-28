```c++
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>

std::mutex mtx;  // 互斥量
std::condition_variable cv;  // 条件变量
bool ready = false;  // 共享的条件标志

void print_id(int id) {
    std::unique_lock<std::mutex> lock(mtx);  // 上锁
    while (!ready) {  // 如果条件未满足，就等待
        cv.wait(lock);  // 等待条件变量
    }
    // 打印线程ID
    std::cout << "Thread " << id << '\n';
}

void go() {
    std::unique_lock<std::mutex> lock(mtx);
    ready = true;  // 设置共享的条件标志
    cv.notify_all();  // 唤醒所有等待的线程
}

int main() {
    std::thread threads[10];
    for (int i = 0; i < 10; ++i) {
        threads[i] = std::thread(print_id, i);  // 启动10个线程
    }

    std::cout << "10 threads ready to race...\n";
    go();  // 唤醒所有线程

    for (auto& th : threads) {
        th.join();
    }

    return 0;
}

```