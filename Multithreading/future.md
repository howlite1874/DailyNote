```c++
#include <iostream>
#include <future>
#include <thread>

int compute_value() {
    std::this_thread::sleep_for(std::chrono::seconds(2)); // 模拟长时间运算
    return 42;
}

int main() {
    // 启动一个异步任务
    std::future<int> result_future = std::async(std::launch::async, compute_value);

    // 在这里，我们可以继续执行其他任务，而 compute_value 在后台运行

    int value = result_future.get();  // 这里会阻塞，直到任务完成并返回结果

    std::cout << "Computed value: " << value << std::endl;
    return 0;
}

```