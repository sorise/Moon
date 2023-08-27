### [插入排序](#)

**介绍**：  把待排序的记录按其关键码值的大小逐个插入到一个已经排好的有序序列中，直到所有的记录插入完为止，得到一个新的序列。

----
* 时间复杂度：O(N^2)
* 空间复杂度：O(1)
* 稳定性：稳定
* 应用场景：序列接近有序或者元素个数比较少

![img](./assets/insertionSort.gif)

#### 代码实现

* 从第一个元素开始
* 返回遍历之前的元素，凡是大于当前元素的，都后移。

```cpp
int compare(int a, int b){
    return a - b;
}

/* 插入排序 */
template<typename T, typename Compare = std::function<int(const T&,const T&)>  >
void insertSort(std::vector<T>& array, Compare compare){
    int j = 0;
    for (int i = 1; i < array.size(); i++) {
        auto temp = array[i];
        for ( j = i ; j > 0 && compare(array[j - 1], temp) > 0; j--) {
            array[j] = array[j - 1];
        }
        array[j] = temp;
    }
}

std::vector<int> scores = {34,8,64,51,32,21};

insertSort(scores, compare);

for (auto &num: scores) {
    std::cout << num << " ";
}
```

