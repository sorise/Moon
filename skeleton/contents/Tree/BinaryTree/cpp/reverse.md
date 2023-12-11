###  [For-each 二叉树的递归遍历](#)

**介绍**： 非常简单，必须会！



二叉树数据结构定义：

```cpp
//使用了模板
template<typename T>
struct TreeNode {
    T val;
    TreeNode *left; //左孩子
    TreeNode *right; //右孩子
    TreeNode() : val(0), left(nullptr), right(nullptr) {};
    explicit TreeNode(const T&  x) : val(x), left(nullptr), right(nullptr) {};
    TreeNode(const T&  x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {};
};

//回收一颗树
template<typename T>
void freeTree(TreeNode<T>* tree){
    if (tree != nullptr){
        freeTree(tree->left);
        freeTree(tree->right);
        delete tree;
        tree = nullptr;
    }
}
```

完成递归遍历，然后把结果输出到 `std::vector<T>` 中：



#### [1. 递归先序遍历](#)

主要注意一下空指针提前判断就行！

* 一个前序遍历序列可能对应多个二叉树的形态

```cpp
template<typename T>
void preorder(const TreeNode<T> *node, std::vector<T> &out){
    if (node != nullptr){
        out.push_back(node->val);
        preorder<T>(node->left, out);
        preorder<T>(node->right, out);
    }
}

//先序遍历
template<typename T>
std::vector<T> preorderTraversal(const TreeNode<T> *node){
    std::vector<T> result;
    preorder<T>(node, result);
    return result;
};
```



#### [2. 递归中序遍历](#)

主要注意一下空指针提前判断就行！

* 一个中序遍历序列可能对应多个二叉树的形态

```cpp
template<typename T>
void inorder(const TreeNode<T> *node, std::vector<T> &out){
    if (node != nullptr){
        inorder<T>(node->left, out);
        out.push_back(node->val);
        inorder<T>(node->right, out);
    }
}

template<typename T>
std::vector<T> inorderTraversal(const TreeNode<T> *node){
    std::vector<T> result;
    inorder<T>(node, result);
    return result;
};
```



#### [3. 递归后序遍历](#)

细节决定成败！

* 一个后序遍历序列可能对应多个二叉树的形态

```cpp
template<typename T>
void pastorder(const TreeNode<T> *node, std::vector<T> &out){
    if (node != nullptr){
        pastorder<T>(node->left, out);
        pastorder<T>(node->right, out);
        out.push_back(node->val);
    }
}

template<typename T>
std::vector<T> inorderTraversal(const TreeNode<T> *node){
    std::vector<T> result;
    pastorder<T>(node, result);
    return result;
};
```

