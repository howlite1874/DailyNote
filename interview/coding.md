实现一个基本的链表数据结构，包括添加、删除和查找功能。
```c++
struct Node{
	int data;
	Node* next;
	Node(int value):data(value),next(nullptr) {}
};

class LinkedList{
private:
	Node* head;
public:
	LinkedList() : head(nullptr){};
	void append(int value){
		Node* newNode = new Node(value);
		if(!head){
			head = newNode;
			return;
		}
		
		Node* current = head;
		while(current->next){
			current = current -> next;
		}
		
		current->next = newNode;
	}
	
	void remove(int value){
		if(!head) return;
		if(head->data == value){
			Node* temp = head;
			head = head->next;
			delete temp; return;
		}
		
		Node* current = head;
		while(current->next && current -> next -> data !=value){
			current = current->next;
		}
		
		if(current->next){
			Node* temp = current->next;
			current->next = temp ->next;
			delete temp;
		}
		
		Node* Find(int value){
			Node* current = head;
			while(current){
				if(current->data == value){
					return current;
				}
				current = current ->next;
			}
			 return nullptr;
			}
		}
};
```

给定一个二叉树，写一个函数来查找其最大深度。
```c++

struct TreeNode {
	int val;
	TreeNode* left;
	TreeNode* right; 
	TreeNode(int x) : val(x), left(nullptr), right(nullptr) {} 
};

class Solution {
	public: 
	int maxDepth(TreeNode* root) { // 如果节点为空，则深度为0
		if (root == nullptr) {
			return 0; 
		}
		// 递归计算左子树和右子树的深度
		int leftDepth = maxDepth(root->left);
		int rightDepth = maxDepth(root->right);
		 // 返回当前树的深度，即较大的子树深度加1
		return std::max(leftDepth, rightDepth) + 1;
	}
};

```


quick sort
```cpp
template<typename T>
void quick_sort_recursive(T arr[], int start, int end) {
    if (start >= end) return;
    T mid = arr[end];
    int left = start, right = end - 1;
    while (left < right) {
        while (arr[left] < mid && left < right) left++;
        while (arr[right] >= mid && left < right) right--;
        std::swap(arr[left], arr[right]);
    }
    if (arr[left] >= arr[end])
        std::swap(arr[left], arr[end]);
    else
        left++;
    if (left) {
	    quick_sort_recursive(arr, start, left - 1);
	}
    quick_sort_recursive(arr, left + 1, end);
}

template<typename T>
void quick_sort(T arr[], int len) {
    quick_sort_recursive(arr, 0, len - 1);
}
```



