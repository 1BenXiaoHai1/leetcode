# Binary Search



## leetcode

### 35.搜索插入位置

-  一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。时间复杂度为 `O(log n)` 的算法。

-  思路：二分查找寻找元素，存在则返回索引。不存在则返回下标。

  ```c++
  class Solution {
  public:
      int searchInsert(vector<int>& nums, int target) {
          int n = nums.size();
          int left = 0,right = n-1;
          int ans = n;
          while( left<=right ){
              int mid = (left+right)/2;
              if(nums[mid]==target){
                  ans = mid;
                  break;
              }else if( nums[mid] < target ){ // 目标值太大，应该在右区间找,nums[mid]比target 小，则target肯定在mid的右边
                  left = mid+1;
              }else{ // 目标值太小，应该在左区间找,nums[mid]比target大，说明target如果插入，至少可以插在mid这个位置
                  right = mid-1;
                  ans = mid;
              }
          }
          return ans;
      }
  };
  ```

- 

### 74.搜索二维矩阵

-  给一个矩阵（每行中的整数从左到右按非严格**递增顺序**排列，每行的第一个整数大于前一行的最后一个整数），在矩阵中判断target是否存在。

  <img src="https://assets.leetcode.com/uploads/2020/10/05/mat.jpg" alt="img" style="zoom:80%;" />

  ![image-20260307173724037](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260307173724037.png)

-  思路：将矩阵每一行拼接在上一行的末尾，得到一个升序数组，在该数组上二分找到目标元素。

  ```c++
  class Solution {
  public:
      bool searchMatrix(vector<vector<int>>& matrix, int target) {
          int m=matrix.size(),n=matrix[0].size();
          // 将二维矩阵拼接为一个一维矩阵，在一维矩阵上执行二分查找
          int left = 0,right = m*n-1;
          while( left <= right ){
              int  mid = (left+right)/2;
              int val = matrix[mid/n][mid%n];
              if( val==target ){
                  return true;
              }else if( val<target ){ // 增大左区间
                  left = mid + 1;
              }else{ // 缩小右区间
                  right = mid - 1;
              }
          }
          return false;
  
      }
  };
  ```

- 

### 34.在排序数组中查找元素的第一个和最后一个位置

-  一个非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。

- 思路：二分遍历查找target。

  - 寻找 leftIdx 即为在数组中寻找第一个大于等于 target 的下标。寻找 `>= target` 不仅能找到存在的 target 的左边界，还能在 target **不存在**时，告诉我们“target 应该插入在哪里”或者“第一个比 target 大的数在哪里”。
  - 寻找 rightIdx 即为在数组中寻找**第一个大于 target 元素**的下标（下标减1就是第n个target的元素的位置），然后将下标减一。

  ```c++
  class Solution {
  public:
      int binarySearch(vector<int>& nums, int target, bool lower){
          // 寻找第一个≥target的元素的下标（相当于把二分查找的等于和大于条件合并了）
          int left=0,right=nums.size()-1,ans = nums.size();
          while( left<=right ){
              int mid = (left+right)/2;
              // lower为true，nums[mid]>=target。lower为false，nums[mid] > target
              if( nums[mid] > target || (nums[mid]>=target&&lower)){
                  right = mid - 1;
                  ans =  mid;
              }else{
                  left = mid + 1;
              }
          }
          return ans;
      }
  
      vector<int> searchRange(vector<int>& nums, int target) {
          int leftIdx = binarySearch(nums,target,true); // 寻找第一个大于等于target的元素
          int rightIdx = binarySearch(nums,target,false) - 1; // 寻找第n个等于target元素（第一个大于taget的元素，然后下标-1)
          if( leftIdx <= rightIdx && rightIdx < nums.size() && nums[leftIdx]==target && nums[rightIdx]==target ){
              return vector<int>{leftIdx,rightIdx}; // 找到了
          }
  
          return vector<int>{-1,-1};
  
      }
  };
  ```

- 

### 33.搜索旋转排序数组

-  
- 思路：
- 