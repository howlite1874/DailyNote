1.make a const function can assign value
2.
```c++
std::function<int(int)> a = [=](int x)mutable->int{
	x = 3;
	return x;
}

int x = 2;
a(x);
```
