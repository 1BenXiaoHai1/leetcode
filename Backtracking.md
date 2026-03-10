# Backtracking

这是一种通过探索所有可能的候选解来找出问题解的算法。如果候选解被确认不是一个有效的解（或者至少不是最后一个有效的解），回溯法会通过在上一步进行一些变化来丢弃该解，即“回溯”并尝试其他可能性。

```c++
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }
    // 在当前决策层，遍历所有可能的选项
    for (选择 : 本层集合中的元素) {
        处理节点;
        backtracking(路径, 选择列表); // 递归，基于当前的选择，进入下一层决策。
        撤销处理; // 回溯，当递归调用返回时（意味着下方的路要么走通了已保存结果，要么走不通失败了），需要恢复现场。
    }
}
```

- **试探性**：分步构建解决方案，每一步都尝试一个选项。
- **剪枝**：一旦发现当前路径无法得到正确解，立即放弃该路径（回溯），避免无效计算。
- **深度优先搜索 (DFS)**：通常基于深度优先搜索策略实现。

![image-20260310095522137](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310095522137.png)

## leetcode

### 46.全排列

-  给定一个不含重复数字的数组 `nums` ，返回其 **所有可能的全排列**。

  ![image-20260310093031215](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310093031215.png)

- 思路：全排列问题。

  ![image-20260310100944863](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310100944863.png)

  ![image.png](https://pic.leetcode.cn/0bf18f9b86a2542d1f6aa8db6cc45475fce5aa329a07ca02a9357c2ead81eec1-image.png)

  ```c++
  class Solution {
  public:
      void dfs(vector<int>& nums, vector<bool> &visited, vector<vector<int>> &res, vector<int> &path){
          // 终止条件
          if( path.size()==nums.size() ){ // 数组元素遍历完了，构建了一个完整的序列
              res.push_back(path);
              return;
          }
          for( int i=0; i<nums.size(); i++ ){ // 选择本层集合中的元素
              if(visited[i]) continue; // 剪枝：如果该元素已被使用，跳过
              // 处理元素
              visited[i]=true; // 标记nums[i]已被占用
              path.push_back(nums[i]); // 将当前的“选择”添加到路径记录中
              // 递归处理
              dfs(nums,visited,res,path); // 当递归返回时（意味着以当前 nums[i] 开头的所有排列都已经找完了），需要恢复现场。
              // 撤销处理
              path.pop_back();
              visited[i]=false;
          }
      }
  
      vector<vector<int>> permute(vector<int>& nums) {
          vector<vector<int>> res; // 存放全排列的结果
          vector<bool> visited(nums.size(),false); // 标记数组，记录哪些元素已经被使用过
          vector<int> path; // 存储当前解空间的路径，即当前正在构建的排列
          dfs(nums,visited,res,path);
  
          return res;
      }
  };
  ```

-  

### 78.子集

解决此类**组合/子集**问题的通用回溯模板

```c++
void backtrack(起始位置 index) {
    收集结果; // 子集问题特有
    
    for (i = index; i < 总数; i++) {
        做选择;
        backtrack(i + 1); // 关键点：下一层从 i+1 开始
        撤销选择;
    }
}
```



- 一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的**子集**。

  ![image-20260310101142617](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310101142617.png)

- 思路：

  ![image-20260310111346525](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310111346525.png)

  ```c++
  class Solution {
  public:
      void dfs(vector<int>& nums, vector<vector<int>> &res, vector<int> &path,int index){
          // 子集问题不需要等到终止条件才收集，因为每一个节点都是一个合法的子集
          res.push_back(path);
          // 选择本层集合中的元素。index保证不重复选取，只能向后选取
          // 如果当前选了 nums[i]，下一层只能从 i+1 及其之后的元素中选，不能回头选前面的
          for( int i=index;i<nums.size();i++ ){ // 之前的元素（下标小于 index 的）已经被处理过
              // 处理元素
              path.push_back(nums[i]); // 将当前的“选择”添加到路径记录中
              dfs(nums,res,path,i+1); // 
              // 撤销处理
              path.pop_back(); 
          }
          return;
      }
  
      vector<vector<int>> subsets(vector<int>& nums) {
          vector<vector<int>> res; // 存储子集的结果
          vector<int> path; // 存储当前的路径
          dfs(nums,res,path,0);
  
          return res;
      }
  };
  ```

- 

### 17.电话号码的字母组合

-  一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

  <img src="https://pic.leetcode.cn/1752723054-mfIHZs-image.png" alt="img" style="zoom: 25%;" />

  ![image-20260310112400305](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310112400305.png)

- 思路：

  ![image-20260310122148156](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310122148156.png)

  ```c++
  class Solution {
      // 构建hash表
      unordered_map<char,string> digitsToLetters{
          {'2', "abc"},
          {'3', "def"},
          {'4', "ghi"},
          {'5', "jkl"},
          {'6', "mno"},
          {'7', "pqrs"},
          {'8', "tuv"},
          {'9', "wxyz"}
      };
  public:
      void dfs(vector<string>& res,string& digits,string& path,int index){
          // 终止条件，长度相同，已经为每一个数字都选择了一个字母
          if(path.length() == digits.length()){
              res.push_back(path);
              return;
          }
          // 获取当前层的数字和对应的字母串
          char digit = digits[index];
          string letters = digitsToLetters[digit];
          // 遍历当前数字对应的所有字母
          for(int i = 0;i<letters.length();i++){
              // 处理元素
              path.push_back(letters[i]); // 将当前字母加入路径
              // 递归处理
              dfs(res,digits,path,index+1); // 处理下一个数字 (index + 1)
              // 撤销处理
              path.pop_back(); // 回溯，移除最后一个字母，尝试下一个字母
          }
      }
      vector<string> letterCombinations(string digits) {
  
          vector<string> res; // 存储字母组合结果
          string path; // 存储当前组合的结果
          dfs(res,digits,path,0);
  
          return res;
      }
  };
  ```

- 

### 39.组合总和

-  一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 所有 **不同组合** ，并以列表形式返回。`candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。

  ![image-20260310122524770](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310122524770.png)

- 思路：**排列组合搜索问题**，采用回溯法求解。

  ![fig1](https://assets.leetcode.cn/solution-static/39/39_fig1.png)

  ![image-20260310125717365](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310125717365.png)

  ```c++
  class Solution {
  public:
      void dfs(vector<vector<int>>& res,vector<int>& nums,vector<int>& path,int sum,int target,int index){
          // 终止条件
          if(sum==target){ // 路径和为target
              res.push_back(path);
              return;
          }
          if (sum > target) return; // 剪枝
          // 只向后选择数字，不会“回头”选前面的数字
          for(int i=index;i<nums.size();i++){
              // 处理元素
              path.push_back(nums[i]); // 将当前数字加入路径
              // 递归遍历
              // 注意传入i，因为数字允许重复使用，所以下一层仍然可以从当前位置 i 开始选
              dfs(res,nums,path,sum+nums[i],target,i); 
              // 回溯
              path.pop_back();
          }
      }
  
      vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
          vector<vector<int>> res; // 存放所有符合条件的组合
          vector<int> path; // 存放当前正在尝试的组合
          int sum = 0; // 当前路径和
          dfs(res,candidates,path,sum,target,0);
  
          return res;
      }
  };
  ```

- 

### 括号生成

- 数字 `n` 代表生成括号的对数，用于生成所有可能的并且 **有效的** 括号组合。

  ![image-20260310125809480](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310125809480.png)

- 思路：

  ![image-20260310132545395](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310132545395.png)

  - 在每一步递归中，面临两个选择：**放左括号** 或 **放右括号**。
    - **添加左括号 `'('`**
      - **逻辑**：只要已用的左括号数量 `left` 还没达到上限 `n`，我们就可以放心地加一个左括号。
      - **安全性**：加左括号永远不会导致非法（只要总数不超），因为它总是可以被后续的右括号匹配。
    - **添加右括号 `')'`**
      - **逻辑**：只有当 **已用的右括号数量 `right` 少于 已用的左括号数量 `left`** 时，才能加右括号。
      - 关键点：
        - 如果 `right == left`，说明目前的左括号都已经有了对应的右括号（或者还没开始），此时再加右括号就会变成 `... )`，导致右括号多于左括号，非法。
        - 如果 `right > left`，这种情况在之前的步骤就被拦截了，根本不会发生。

  ```c++
  class Solution {
  public:
      void dfs(vector<string>& res,string& path,int n, int left, int right){
          // 终止条件，括号字符串的长度==n * 2
          if(path.size() == n*2){
              res.push_back(path);
              return;
          }
          // 添加左括号 '('
          // 条件：左括号数量还没用完（小于 n）
          if (left < n) {
              path.push_back('(');           // 做选择
              dfs(res, path, n, left + 1, right); // 递归：左括号数+1
              path.pop_back();               // 撤销选择（回溯）
          }
  
          // 添加右括号 ')'
          // 条件：右括号数量少于左括号数量（保证有效性，不会出现 "())" 这种情况）
          if (right < left) {
              path.push_back(')');           // 做选择
              dfs(res, path, n, left, right + 1); // 递归：右括号数+1
              path.pop_back();               // 撤销选择（回溯）
          }
  
  
  
      }
  
      vector<string> generateParenthesis(int n) {
          vector<string> res;
          string path;
          dfs(res,path,n,0，0);
          return res;
      }
  };
  ```

- 

### 单词搜索

- 一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。判断word是否在board中。单词必须按照字母顺序，通过相邻的单元格内的字母构成。

  ![image-20260310152459985](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310152459985.png)

  ![image-20260310152126424](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310152126424.png)

- 思路：

  ![image-20260310154344363](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310154344363.png)

  ```c++
  class Solution {
      int rows,cols;
  public:
      bool exist(vector<vector<char>>& board, string word) {
          rows = board.size();
          cols = board[0].size();
          // 对每个元素进行dfs(board上每个位置元素都有可能是单词的起点)
          for(int i=0; i<rows; i++){
              for(int j=0; j<cols;j++){
                  if(dfs(board,word,i,j,0)) return true;
              }
          }
          return false;
      }
  
      bool dfs( vector<vector<char>>& board,string& word,int i,int j,int k){
          // 终止条件
          if( i>=rows||i<0||j>=cols||j<0||board[i][j]!=word[k]) { // 越界、字母不一致
              return false;
          }
          if( k==word.size()-1 ) {
              return true; // 长度为word长度（字母均一致）
          }
          // 元素处理
          board[i][j]='\0'; // 将遍历过的字母设置为空，代表已访问
          // 递归遍历，上下左右四个方向
          bool res = dfs(board,word,i+1,j,k+1)||
                      dfs(board,word,i-1,j,k+1)||
                      dfs(board,word,i,j+1,k+1)||
                      dfs(board,word,i,j-1,k+1);
          board[i][j] = word[k]; //  回溯，将 board[i][j] 元素还原至初始值
  
          return res;
      }
  };
  ```

-  

### 131.分割回文串

- 将 `s` 分割成一些 子串，使每个子串都是 **回文串** 。返回所有可能的分割方案。

  ![image-20260310160537533](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310160537533.png)

- 思路：

  ![image-20260310165056480](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310165056480.png)

  ![image-20260310165307951](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260310165307951.png)

  ```c++
  class Solution {
  public:
      bool judge(string s){
          if( s.size()==0||s.size()==1 ) return true;
          for( int j=s.size()-1,i=0;i<=j;j--,i++ ){
              if (s[j]!=s[i]){
                  return false;
              }
          }
          return true;
      }
  
      void dfs(vector<vector<string>>& res,vector<string>& path,string s,int index){
          // 终止条件
          if(index==s.size()){
              res.push_back(path);
              return;
          }
          // 从 index 开始，截取不同长度的子串 s[index...i]
          for(int i=index;i<s.size();i++){
              string t = s.substr(index,i-index+1); // 获取子串
              if(judge(t)){ // 剪枝：只有当子串是回文时，才继续递归
                  // 处理元素
                  path.push_back(t); // 将当前回文子串添加进路径
                  // 递归处理
                  dfs(res,path,s,i+1); 
                  // 回溯，撤销选择
                  path.pop_back();
              }
          }
      }
  
      vector<vector<string>> partition(string s) {
          vector<vector<string>> res;
          vector<string> path;
          dfs(res,path,s,0);
  
          return res;
      }
  };
  ```

- 