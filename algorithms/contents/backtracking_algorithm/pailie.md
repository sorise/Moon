

#### [46. 全排列](https://leetcode.cn/problems/permutations/)

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。





* 使用visited 数组来避免重复

```cpp
class Solution {
private:
    vector<vector<int>> ans;
    
public:

    void dfs(vector<int>& nums, vector<int>& visit, vector<int>& track, int l){
        if(l == nums.size()){
            ans.push_back(track);
        }else{
            for(int i = 0; i < nums.size(); i++){
                if(visit[i]) continue;
                visit[i] = true;
                track.push_back(nums[i]);
                dfs(nums, visit,track, l + 1);
                track.pop_back();
                visit[i] = false;
            }
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> visit(nums.size(), false);
        vector<int> track;
        dfs(nums, visit, track, 0);
        return ans;
    }
};
```

* 使用交换元素来避免重复

稍微优化：

```cpp
class Solution {
private:
    vector<vector<int>> ans;
    
public:

    void dfs(vector<int>& nums, int l){
        if(l == nums.size()){
            ans.push_back(nums);
        }else{
            for(int i = l; i < nums.size(); i++){
                swap(nums[i], nums[l]);
                dfs(nums,  l + 1);
                swap(nums[i], nums[l]);
            }
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        dfs(nums, 0);
        return ans;
    }
};
```



#### [51. N 皇后](https://leetcode.cn/problems/n-queens/)

按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。

**n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回所有不同的 **n 皇后问题** 的解决方案。

每一种解法包含一个不同的 **n 皇后问题** 的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

```cpp
class Solution {
    
public:
    bool vaild(vector<string>& table, int row, int col){
        /* 判断列方向 */
        for(int i = 0; i < row; i++) {
            if(table[i][col] == 'Q') return false;
        }
        /* 检查右上方是否有皇后互相冲突 */
        int i = row - 1, j = col - 1;
        while( i >= 0 && j >= 0){
            if(table[i][j] == 'Q') return false;
            i--;
            j--;
        }
        /* 检查左上方是否有皇后互相冲突 */
        i = row - 1;
        j = col + 1;
        while( i >= 0 && j >= 0){
            if(table[i][j] == 'Q') return false;
            i--;
            j++;
        }
        return true;
    }

    void dfs(vector<vector<string>>& ans, vector<string>& table, int row, int n){
        if(row == n){
            ans.push_back(table);
            return;
        }
        for(int col = 0; col < n; col++){
            if(!vaild(table, row, col)) continue;
            table[row][col] = 'Q';
            dfs(ans, table, row + 1, n);
            table[row][col] = '.';
        }
    }

    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> ans;
        vector<string> table(n, string(n, '.'));
        dfs(ans, table, 0, n);
        return ans;
    }
};
```

