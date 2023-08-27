### [层次遍历](#)

**介绍：** 其实很简单，搞清楚关键点！

* 需要一个队列
* 首先把头节点加入队列。
* while循环中判断队列中元素个数 n。 进行n次出队列。
* 出队列元素的子节点加入队列中。
* 下一个循环

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {

    vector<vector<int>> levelResult;
    std::queue<TreeNode*> nodeQueue;

    if (root == nullptr) return levelResult;
    nodeQueue.push(root);

    while (!nodeQueue.empty()){
        vector<int> result;
        auto nodeSize = nodeQueue.size();
        result.reserve(nodeSize);
        for (int i = 0; i < nodeSize; ++i) {
            auto front = nodeQueue.front();
            result.push_back(front->val) ;
            if (front->left != nullptr) nodeQueue.push(front->left);
            if (front->right != nullptr) nodeQueue.push(front->right);
            nodeQueue.pop();
        }
        levelResult.push_back(result);
    }
    return levelResult;
}
```

