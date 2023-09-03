```c++
template<typename T,size_t j>  
void print(const std::array<T,j>& a) {  
    for(int i =0;i<j;i++) {  
        std::cout << a[i]<< std::endl;  
    }  
}  
  
int main() {  
    std::array<int,6> arr = {1,2,3,4,5,6};  
    print(arr);  
  
}
```
