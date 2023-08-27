### [Sequential Search 顺序查找](#)

**介绍**：从头到脚挨个找(或者反过来也OK)——对线性表的无差别的遍历，**通常用于线性表（顺序表、链式表）**。

----

时间复杂度：O(n), 有时候会增加一个**哨兵位置**，0号位为哨兵，然后从尾部向前查找。

```cpp
//顺序表（动态分配）查找的实现
typedef struct{
	ElemType *elem;
	int TableLen;
}SSTable;//sequential search table 
 
//顺序查找
//在顺序表中找到元素值为key的元素，查找成功返回元素下标，否则返回-1 
int Search_Seq(SSTable ST,ElemType key){
	int i;
	for(i=0; i<ST.TableLen && ST.elem[i]!=key; ++i);
	return i==ST.TableLen? -1 : i;
}
```

如果查找表示有序的，效率会稍微提高一点。

```cpp
public static int sequenceSearch(int[] nums, int target){

    for(int i = 0; i < nums.length; i++){
        if(nums[i] == target){
            return i;
        }
    }
    return -1;
}
```

