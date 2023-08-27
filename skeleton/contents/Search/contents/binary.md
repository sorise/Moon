### [BinarySearch 二分查找](#)

**介绍**：建立在查找表已经有序的基础上，时间复制度为 O(log<sub>2</sub><sup>n</sup>)，仅适用于**有序**的**顺序表**。

----

算法实现：
```cpp
int search(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while (left <= right){
        int mid = (left + right) / 2 ;
        if (target == nums[mid]) return mid;
        if (nums[mid] > target ) right = mid - 1;
        if (nums[mid] < target ) left = mid + 1;
    }
    return -1;
}
```

