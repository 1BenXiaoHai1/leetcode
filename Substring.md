# Substring

## leetcode

### 560.和为K的子数组

- 一个整数数组 `nums` 和一个整数 `k` ，统计并返回 该数组中和为 `k` 的子数组的个数（数组中元素的连续非空序列） 。

  <img src="C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260307161106388.png" alt="image-20260307161106388"  />

  ![image-20260307161123588](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260307161123588.png)

- 思路：连续子数组和问题，用**前缀和**处理。

  ![image-20260309100513743](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309100513743.png)

  ```c++
  class Solution {
  public:
      int subarraySum(vector<int>& nums, int k) {
          int n = nums.size();
          vector<int> s(n+1); // 数组长度+1
          // 构建前缀和数组
          for(int i=0; i<n; i++){
              s[i+1] = s[i]+nums[i];
          }
  
          // 遍历前缀和并统计“以当前位置结尾、和为 k 的子数组”有多少个
          unordered_map<int,int> cnt; // 用来记录某个前缀和数值(Key)出现的次数(Value)
          int ans=0;
          for( int sj:s ){
              // 在当前 sj 之前，有没有出现过值为 sj - k 的前缀和
              ans += cnt.contains(sj-k) ? cnt[sj - k]:0;  
              // 如果先 cnt[sj]++ 再查询，当 k=0 时，sj - k 就是 sj 自己。会把“当前这个位置本身”也算作一个之前的前缀和
              cnt[sj]++;
          }
          return ans;
      }
  };
  ```

- 