# 双指针

双指针的核心思想是：使用两个变量（指针）在数据结构上移动，通过特定的策略（如相向而行、同向而行等）来解决问题，通常能将时间复杂度从暴力解法的 O(n^2)降低到 O(n) 。

## leetcode

### 283.移动零

- 给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

  ![image-20260302154436233](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260302154436233.png)

- 思路：使用同向双指针-快慢指针。

  <img src="C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260302155804741.png" alt="image-20260302155804741" style="zoom:67%;" />

  - **`right` (快指针)**：负责**遍历**整个数组，寻找非零元素。
  - **`left` (慢指针)**：指向**下一个非零元素应该存放的位置**（也可以理解为“已处理好的非零序列的末尾”）。

  ```c++
  class Solution {
  public:
      void moveZeroes(vector<int>& nums) {
          int left = 0; // 慢指针：指向"下一个非零数该放哪"
          int right = 0; // 快指针：用于扫描数组
          while(right < nums.size()){
              // 判断当前right指针所指元素是否非0，非0则需要和left指针所指元素交换
              if (nums[right]){
                  swap(nums[left],nums[right]);
                  left++;
              }
              right++;
          }     
      }
  };
  ```

### 11.盛最多水的容器

-  给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

-  思路：

  ![image-20260302172448458](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260302172448458.png)

  ![image-20260302172521855](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260302172521855.png)

  ```c++
  class Solution {
  public:
      int maxArea(vector<int>& height) {
          int left = 0;
          int right = height.size() - 1;
          int max_water = 0;
          while( left < right ) {
              int w = right - left;
              int h = min(height[left],height[right]);
  
              // 计算面积并更新最大值
              int current_area = w * h;
              max_water = max(max_water,current_area);
  
              // 贪心策略，移动最短的边。移动最短的边才可能遇到更高的边
              if(height[left] < height[right]) {
                  left++;
              } else {
                  right--;
              }
  
          }
  
          return max_water;
      }
  };
  ```

  

- 

### 15.三数之和

- 一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0`

  ![image-20260302172943933](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260302172943933.png)

- 思路：

  <img src="https://pic4.zhimg.com/v2-4513934db68a6b44d364a330c008a9b1_r.jpg" alt="img" style="zoom: 40%;" />

  <img src="https://picx.zhimg.com/v2-c26ca7cdd7440af20bb141562f86dd21_r.jpg" alt="img" style="zoom:40%;" />

  ```c++
  class Solution {
  public:
      vector<vector<int>> threeSum(vector<int>& nums) {
          vector<vector<int>> result;
          int n = nums.size();
  
          if (n < 3) return result;
  
          // 排序，从小到大
          sort(nums.begin(), nums.end());
          for (int i = 0; i < n - 2; ++i) {
              // 如果当前数字大于0，因为数组已排序，后面的数都大于0，和不可能为0
              if (nums[i] > 0) break;
  
              // 如果当前数字和前一个数字相同，跳过，避免重复三元组
              if (i > 0 && nums[i] == nums[i - 1]) continue;
  
              // 双指针
              int left = i + 1;
              int right = n - 1;
              while (left < right) {
                  int sum = nums[i] + nums[left] + nums[right];
                  if (sum == 0) {
                      result.push_back({nums[i], nums[left], nums[right]});
                      // 移动左指针，跳过重复元素
                      while (left < right && nums[left] == nums[left + 1]) left++;
                      // 移动右指针，跳过重复元素
                      while (left < right && nums[right] == nums[right - 1]) right--;
                      // 移动到下一个不同的数
                      left++;
                      right--;
                  } else if (sum < 0) {
                      // 和太小，左指针右移以增大和
                      left++;
                  } else {
                      // 和太大，右指针左移以减小和
                      right--;
                  }
              }
          }
  
          return result;
      }
  };
  ```

- 