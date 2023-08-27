### [利用栈来实现对树的遍历](#)

```cpp
//Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};
```

#### [1. 前序遍历](#)
先序遍历又是：头节点 -> 左节点 -> 右节点 的顺序， 先输出当前节点，然后将右孩子节点入栈，再让当前节点等于左孩子节点。如果有左子树，继续输出，直到为空，弹出栈节点， 输出右子树。

**头出压右向左走，弹弹弹**

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> result;
    std::stack<TreeNode *> nodes_stack;
    TreeNode *node = root;
    while (node != nullptr || !nodes_stack.empty()){
        //先把头节点压入栈,左链入栈
        if (node != nullptr){
            result.push_back(node->val); //输出
            if (node->right!= nullptr){
                nodes_stack.push(node->right); //右子树入栈
            }
            node = node->left; //输出左子树
        }else if(!nodes_stack.empty()){
            node = nodes_stack.top(); //获得栈顶，输出右子树
            nodes_stack.pop(); 
        }
    }
    return result;
}
```

测试代码:
```cpp
TreeNode root(10);
TreeNode left(11);
TreeNode right(12);
TreeNode left_left(13);
TreeNode left_right(14);

root.left = &left;
root.right = &right;
left.left = &left_left;
left.right = &left_right;

for (auto &&v:preorderTraversal(&root)) {
    std::cout << v << " ";
}
```

#### [2. 中序遍历](#)

中序遍历是：左节点 -> 头节点 -> 右节点的顺序。**所以先压入头结点，然后压入所有层级的左节点**，弹出栈顶节点时判断是否有右子树，有则压入右子树反之表明该节点及其子树已遍历完成。

**头压转左，弹转右**

```cpp
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> result;
    std::stack<TreeNode *> nodes_stack;
    TreeNode *node = root;
    while (node != nullptr || !nodes_stack.empty()){
        //先把头节点压入栈,左链入栈
        if (node != nullptr){
            //根非空 入栈
            nodes_stack.push(node);
            //看看 左节点是否为 empty ,非空 入栈
            node = node->left;
        }else if(!nodes_stack.empty()){
            node = nodes_stack.top(); //获得栈顶
            nodes_stack.pop();//弹出
            result.push_back(node->val); //输出
            node = node->right;
        }
    }
    return result;
}
```

测试代码:
```cpp
TreeNode root(10);
TreeNode left(11);
TreeNode right(12);
TreeNode left_left(13);

root.left = &left;
root.right = &right;
left.left = &left_left;

for (auto &&v:inorderTraversal(&root)) {
    std::cout << v << " ";
}
```




#### [3. 后序遍历 -- 难点](#)
后序遍历二叉树是先访问左子树，再访问右子树，最后访问根结点。

[**算法思想**：](#)

1. 先沿根结点，依次入栈，直到左孩子为空
2. 读取栈顶元素；如果其右孩子不空且未被访问过，将右子树转执行 1；
3. 否则，栈顶元素出栈并访问。

```cpp
vector<int> postorderTraversal(TreeNode* node) {
    vector<int> result;
    std::stack<TreeNode *> nodesStack;
    TreeNode *lastPrint = nullptr;

    while (node != nullptr || !nodesStack.empty()) {
        if (node != nullptr) {
            //走到最左边
            nodesStack.push(node);
            node = node->left;
        } else {
            //弹出栈顶
            TreeNode *topNode = nodesStack.top();
            //若右子树存在，且未被访问过
            if (topNode->right && topNode->right != lastPrint) {
                //转向右
                node = topNode->right;
            }else{
                //否则弹出结点并访问
                result.push_back(topNode->val);
                lastPrint = topNode; //记录最近访问的结点 
                nodesStack.pop();//弹出
                node = nullptr;
            }
        }
    }
    return result;
}
```

