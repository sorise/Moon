

### [选择排序](#)

**介绍**： 选择排序是一种简单直观的排序算法，无论什么数据进去都是 O(n²) 的时间复杂度。所以用到它的时候，数据规模越小越好。唯一的好处可能就是不占用额外的内存空间了吧。 **每次吧最小的放在前面**。

----

**原理**：每一趟在待排序元素中选取关键字最小/最大的元素加入有序子序列。

* 复杂度：总是 O(^2)
* 稳定性：由于交换过程，所以并不稳定。
  * 2 2 1 进过一次选择后编程  1 2 2。 第一个2 到最后去了





![img](./assets/selectionSort.gif)



首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置。

再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。

重复第二步，直到所有元素均排序完毕。

```cpp
template<typename T, typename Compare = std::function<int(const T&,const T&)>>
void chooseSort(std::vector<T>& array, Compare compare){
    auto count = array.size();
    for (int i = 0; i < count - 1; ++i) {
        auto min = i;
        for (int j = i + 1; j < count; ++j) {
            if (compare(array[j], array[min]) < 0 ){
                min = j;
            }
        }
        std::swap(array[i], array[min]);
    }
}
```

