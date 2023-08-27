### [基数排序](#)
**介绍**： 基数排序（`radix sort`）属于分配式排序（`distribution sort`），又称桶子法（`bucket sort`）或 `bin sort`。

----

基数排序是具有**稳定性**的排序算法，其时间复杂度为 **O(nlog(r) * m)**，是一个事件复杂度为线性复杂度！
* n 是算法输入
* r 是基数
* m 是堆数

#### 相关题目
[https://leetcode.cn/problems/maximum-gap/](https://leetcode.cn/problems/maximum-gap/)


#### [1. 基数排序的 C++ 实现](#)
正常实现只能用于整数处理，用于负数和浮点数需要一些其他手段！

需要理解的是 `把storeList的位置分配给每个桶` 和 `入桶` 这个两个操作的原理！

```cpp
//辅助函数，求数据的最大位数，排序需要几趟
int maxBit(vector<int> & allNumber, const int& radix){
    vector<int>::size_type len = allNumber.size();
    if (len == 0) return 0;
    int max = allNumber[0];
    for (int i = 0; i < len; ++i) {
        if (allNumber[i] > max) max = allNumber[i];
    }
    int base = 1;
    while (max/radix !=0){
        base++;
        max /=radix;
    }
    return base;
}

vector<int> sortArray(vector<int>& nums) {
    if (nums.size() <= 1) return nums;
    const int base = 10;
    //进入桶
    int radix = 1;
    int *counter = new int[base]; //计数器
    int *storeList = new int[nums.size()];
    int times = maxBit(nums, base);
    for (int i = 0; i < times; i++) {
        for (int k =0; k < base; k++)
            counter[k] = 0; //重置计数器
        //统计每个桶多少个记录
        for (int idx = 0; idx < nums.size(); idx++)
            counter[ (nums[idx]/radix)%base ]++;

        //把storeList的位置分配给每个桶
        for (int k =1; k < base; k++){
            counter[k] += counter[k - 1];
        }
        //入桶
        for (long j = (long)nums.size() - 1; j >= 0 ; j--) {
            auto mod = (nums[j] / radix)%base;
            storeList[ counter[mod] - 1] = nums[j];
            counter[mod] --;
        }
        //反过来赋值
        for (int t = 0; t < nums.size(); t++)
            nums[t] = storeList[t];
        radix = radix * base;
    }

    delete [] counter;
    delete [] storeList;
    return nums;
}
```

#### [2. 处理存在负数的情况](#)
先判断有无负数 有则找到最小值 数组所有数据都减去该值 也就是数组最小值会变为0 接下来按正整数排序 最后数组所有数据加上原最小数变为原有数据值。

```cpp
//辅助函数，求数据的最大位数，排序需要几趟
int maxBit(vector<int> & allNumber, const int& radix){
    vector<int>::size_type len = allNumber.size();
    if (len == 0) return 0;
    int max = allNumber[0];
    for (int i = 0; i < len; ++i) {
        if (allNumber[i] > max) max = allNumber[i];
    }
    int base = 1;
    while (max/radix !=0){
        base++;
        max /=radix;
    }
    return base;
}

vector<int> sortArray(vector<int>& nums) {
    if (nums.size() <= 1) return nums;
    //负数情况处理
    int min = nums[0];
    for (auto &v: nums) {
        if (v < min) min = v;
    }
    if (min < 0){
        for (auto &v: nums) {
            v -= min;
        }
    }
    //负数情况处理 END
    const int base = 10;
    int radix = 1;
    int *counter = new int[base]; //计数器
    int *storeList = new int[nums.size()];
    int times = maxBit(nums, base);
    for (int i = 0; i < times; i++) {
        for (int k =0; k < base; k++)
            counter[k] = 0; //重置计数器
        //统计每个桶多少个记录
        for (int idx = 0; idx < nums.size(); idx++)
            counter[ (nums[idx]/radix)%base ]++;

        //把storeList的位置分配给每个桶
        for (int k =1; k < base; k++){
            counter[k] += counter[k - 1];
        }
        //入桶
        for (long j = (long)nums.size() - 1; j >= 0 ; j--) {
            auto mod = (nums[j] / radix)%base;
            storeList[ counter[mod] - 1] = nums[j];
            counter[mod] --;
        }
        //反过来赋值
        for (int t = 0; t < nums.size(); t++)
            nums[t] = storeList[t];
        radix = radix * base;
    }

    //负数情况处理
    if (min < 0){
        for (auto &v: nums) {
            v += min;
        }
    }

    delete [] counter;
    delete [] storeList;
    return nums;
}
```

#### [3. 浓缩版本](#)

```cpp
vector<int> sortArray(vector<int>& nums) {
    if (nums.size() <= 1) return nums;
    const int base = 10;
    int exp = 1;
    /* -- 处理负数的情况 -- */
    int minVal = *(std::min_element(nums.begin(),nums.end()));
    if (minVal < 0){
        std::for_each(nums.begin(),nums.end(), [&](auto &v){ v -= minVal; });
    }
    /* -- 处理负数的情况 -- */
    int maxVal = *(std::max_element(nums.begin(),nums.end()));
    int counter[base];
    int storeList[nums.size()];
    while (maxVal >= exp){
        for (int & k : counter) k = 0; //重置计数器
        for (auto &v: nums) counter[(v/exp)%base]++; //计数
        for (int i = 1; i < base; i++) counter[i] += counter[i-1];
        for (int i = (int)nums.size() - 1; i >= 0 ; i--) {
            auto k = (nums[i]/exp)%base;
            storeList[ counter[k] - 1 ] = nums[i];
            counter[k]--;
        }
        //赋值完了
        for (int i = 0; i < nums.size(); i++) nums[i] = storeList[i];
        exp = exp * base;
    }
    if (minVal < 0){
        std::for_each(nums.begin(),nums.end(), [&](auto &v){ v += minVal; });
    }
    return nums;
}
```



#### 超级版本

```cpp
/* 必须都是大于等于0 */
template<typename T, typename = typename std::enable_if<std::is_integral<T>::value, T>::type>
uint16_t radixBit(T maxValue, unsigned short base){
    uint16_t bits = 1;
    while (maxValue != 0){
        maxValue /= base;
        bits++;
    }
    return bits;
}

template<typename T, typename = typename std::enable_if<std::is_integral<T>::value, T>::type>
void radixSort(std::vector<T>& values, unsigned short base = 10){
    T maxValue = values.front(), minValue = values.front();
    //处理负数
    for (const T &value: values) {
        if (value > maxValue) maxValue = value;
        if (value < minValue) minValue = value;
    }
    if (minValue < 0){
        for (T &value: values) {
            value -= minValue;
        }
    }
    //循环几次
    int bits = (int)radixBit(maxValue, base);
    std::unique_ptr<uint32_t[]> bucket= std::unique_ptr<uint32_t[]>(new uint32_t[base]);
    std::unique_ptr<T[]> temp= std::unique_ptr<T[]>(new T[values.size()]);
    int radix = 1;
    for (int i = 0; i < bits; ++i) {
        //重置一下桶
        for (int k = 0; k < base; ++k) {
            bucket[k] = 0;
        }
        for (auto &value: values) {
            auto idx = (value/radix) % base;
            bucket[idx]++;
        }
        //把storeList的位置分配给每个桶
        for (int j = 1; j < base; ++j) {
            bucket[j] += bucket[j-1];
        }
        //反向入桶
        for (long j = (long)values.size() - 1; j >= 0 ; j--) {
            auto mod = (values[j] / radix)%base;
            temp[ bucket[mod] - 1] = values[j];
            bucket[mod] --;
        }
        //反过来赋值
        for (int t = 0; t < values.size(); t++)
            values[t] = temp[t];

        radix = radix * base;
    }

    if (minValue < 0){
        for (T &value: values) {
            value += minValue;
        }
    }
}
```

