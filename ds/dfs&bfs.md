dfs,graph,stack
```c++
#include <iostream>
#include <vector>
#include <stack>

void DFS(std::vector<std::vector<int>>& graph, int start) {
    std::stack<int> s;
    std::vector<bool> visited(graph.size(), false);

    s.push(start);

    while (!s.empty()) {
        int vertex = s.top();
        s.pop();

        if (!visited[vertex]) {
            std::cout << vertex << " ";
            visited[vertex] = true;
        }

        for (int i : graph[vertex]) {
            if (!visited[i])
                s.push(i);
        }
    }
}

```

tree,dfs
```c++
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

// 前序遍历
void preOrder(TreeNode* root) {
    if (!root) return;
    std::stack<TreeNode*> s;
    s.push(root);

    while (!s.empty()) {
        TreeNode* node = s.top(); s.pop();
        std::cout << node->val << " ";

        if (node->right) s.push(node->right);
        if (node->left) s.push(node->left);
    }
}

// 中序遍历
void inOrder(TreeNode* root) {
    std::stack<TreeNode*> s;
    TreeNode* curr = root;

    while (curr || !s.empty()) {
        while (curr) {
            s.push(curr);
            curr = curr->left;
        }
        curr = s.top(); s.pop();
        std::cout << curr->val << " ";
        curr = curr->right;
    }
}

// 后序遍历
void postOrder(TreeNode* root) {
    if (!root) return;
    std::stack<TreeNode*> s1, s2;
    s1.push(root);

    while (!s1.empty()) {
        TreeNode* node = s1.top(); s1.pop();
        s2.push(node);

        if (node->left) s1.push(node->left);
        if (node->right) s1.push(node->right);
    }

    while (!s2.empty()) {
        TreeNode* node = s2.top(); s2.pop();
        std::cout << node->val << " ";
    }
}

```

bfs,graph,queue
```c++
#include <iostream>
#include <vector>
#include <queue>

void BFS(std::vector<std::vector<int>>& graph, int start) {
    std::queue<int> q;
    std::vector<bool> visited(graph.size(), false);

    visited[start] = true;
    q.push(start);

    while (!q.empty()) {
        int vertex = q.front();
        std::cout << vertex << " ";
        q.pop();

        for (int i : graph[vertex]) {
            if (!visited[i]) {
                visited[i] = true;
                q.push(i);
            }
        }
    }
}

```

BFS,Level Order Traversal
```C++
void levelOrder(TreeNode* root) {
    if (!root) return;
    std::queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        TreeNode* node = q.front(); q.pop();
        std::cout << node->val << " ";

        if (node->left) q.push(node->left);
        if (node->right) q.push(node->right);
    }
}

```