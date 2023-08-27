### [1. 顺序存储结构](#)

二叉树的顺序存储是指用一组地址连续的存储单元依次自上而下、自左至右存储完全二叉树上的结点元素，即将完全二叉树上编号为i的结点元素存储在一维数组下标为i的分量中。

**直接使用一个数组即可， 也可以使用std::array 和 std::vector** 实现。

```cpp
using T = int;
std::vector<std::pair<bool,T>> tree(15);

tree[0].first = true; //头节点存在
tree[0].second = 10;  //值

tree[1].first = true; //头节点存在
tree[1].second = 57;  //值
```

**依据二叉树的性质，完全二叉树和满二叉树采用顺序存储比较合适，树中结点的序号可以唯一地反映结点之间的逻辑关系。**



### [2.  链式存储结构](#)

**二叉树每个结点最多有两个孩子，所以为它设计一个数据域和两个指针域是比较自然的想法，我们称这样的链表叫做二叉链表**。这是最常用的结构了！

```cpp
template<typename T>
struct TreeNode {
    T val;
    TreeNode *left;  //左孩子
    TreeNode *right; //右孩子
    TreeNode() : val(0), left(nullptr), right(nullptr) {};
    explicit TreeNode(const T&  x) : val(x), left(nullptr), right(nullptr) {};
    TreeNode(const T&  x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {};
};
```

**在含有n个结点的二叉链表中，含有n + 1 个空链域**。 因为总共有n个节点，链域有2n个，边数为n-1, 相减即可。 



### [3.  三叉链式存储结构](#)

这样方便啊找到父节点：

```cpp
template<typename T>
struct TreeNode {
    T val;
    TreeNode *left, *right;  //左孩子,右孩子
    TreeNode *parent; //父节点
    TreeNode() : val(0), left(nullptr), right(nullptr),parent(nullptr) {};
    explicit TreeNode(const T&  x) : val(x), left(nullptr), right(nullptr),parent(nullptr) {};
    TreeNode(const T&  x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right),parent(nullptr) {};
    TreeNode(const T&  x, TreeNode *left, TreeNode *right, TreeNode *parent)
    :val(x), left(left), right(right),parent(parent) {};
};
```

