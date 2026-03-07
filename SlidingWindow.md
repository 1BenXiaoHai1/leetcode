# Sliding Window滑动窗口

**滑动窗口 (Sliding Window)** 是算法设计中一种极其高效的技术，主要用于解决**数组**或**字符串**中关于**连续子区间**的问题（如：最长/最短子串、满足特定条件的子数组等）。

核心思想是：**维护一个窗口（由左右两个指针界定），根据条件动态地扩大或缩小窗口范围，从而在一次遍历（ O(n)）内解决问题，避免了暴力解法的双重循环O(n^2)**。**双指针的变体**。

滑动窗口本质上是**同向双指针**（两个指针都从左向右移动）：

1. **右指针 (`right`)**：负责**扩张**窗口，将新元素纳入考量。
2. **左指针 (`left`)**：负责**收缩**窗口，当窗口内的数据不满足条件时，移除左侧元素，直到重新满足条件。

 **总结口诀**

- **求最值**（最长/最短） -> **滑动窗口**。
- **右扩左缩**：右边进，左边出。
- **while 循环**：用于在条件不满足时疯狂收缩左边界。
- **单调性**：利用“右指针右移，左指针只需右移（不需要回溯）”的单调性质。

## leetcode

### 3.无重复字符的最长子串

- 给定一个字符串 `s` ，请你找出其中**不含有重复字符**的 **最长 子串** 的长度。

  ![image-20260307100324382](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260307100324382.png)

- 思路：**“滑动窗口” (Sliding Window)** 算法的一种具体实现变体：**固定左边界，贪心地扩展右边界**。

  ![image-20260307110240571](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260307110240571.png)

  ```c++
  class Solution {
  public:
      int lengthOfLongestSubstring(string s) {
          unordered_set<char> ccWindow;
          int ans = 0;// 最大长度
          int n = s.size();
          // 固定左指针，尽可能伸展右指针，以每个位置开头能走多远
          // right初始值为-1，因为此时窗口内没有任何元素，right始终指向当前窗口的最后一个有效字符
          for(int right=-1,left=0;left<n;left++){
              if( left!=0 ){ // 左指针每向右移动一格后(+1),移除原先最左侧的字符
                  ccWindow.erase(s[left-1]);
              }
              // 右指针下一个位置没有超过边界，且右指针下一个所指的元素还没有出现过
              while( right + 1 < n && !ccWindow.count(s[right+1])){
                  ccWindow.insert(s[right+1]);
                  right++;
              }
  
              ans = max(ans,right-left+1);
          }
          return ans;
      }
  };
  ```

  

- 

### 438.找到字符串中所有字母异位词

- 两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串。

  ![image-20260307093047442](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260307093047442.png)

- 思路：**在字符串 s 中构造一个长度为与字符串 p 的长度相同的滑动窗口**，并在滑动中维护窗口中每种字母的数量；当**窗口中每种字母的数量与字符串 p 中每种字母的数量相同**时，则说明当前窗口为字符串 p 的异位词。

  ```c++
  class Solution {
  public:
      vector<int> findAnagrams(string s, string p) {
          vector<int> ans;
          int sLen = s.size(),pLen = p.size();
  
          if( sLen<pLen ) return vector<int> {};
          vector<int> sCount(26,0); // 
          vector<int> pCount(26,0); // p
  
          // 初始化s的第一个窗口 和 p 的计数
          for( int i=0; i < pLen; i++ ){
              pCount[p[i]-'a']++;
              sCount[s[i]-'a']++;
          }
          // 判断第一个窗口是否为异位词
          if( sCount==pCount ){
              ans.push_back(0);// 第一个窗口为异位词
          }
  
          // 滑动窗口
          for( int i=0; i<sLen - pLen; i++){
              sCount[s[i] - 'a']--; // 移出最左边的字符
              sCount[s[pLen+i]-'a']++; // 移入右边的新字符
              if( sCount==pCount ){ // 判断新窗口是否为异位词
                  ans.push_back(i+1); // 新的窗口的起始位置
              }
          }
          return ans;
  
      }
  };
  ```

- 