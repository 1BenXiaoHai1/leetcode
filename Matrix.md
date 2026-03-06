# Matrix



## leetcode

### 73.矩阵置零

- 一个矩阵，如果有一个元素为0，则将其所在行列的元素均设为0。

  <img src="https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg" alt="img" style="zoom: 67%;" />

- 思路：第一遍遍历矩阵，标记出现0元素的行和列。第二遍遍历矩阵，将标记的行列元素设为0。

  ```c++
  class Solution {
  public:
      void setZeroes(vector<vector<int>>& matrix) {
          int rowLen = matrix.size();
          int colLen = matrix[0].size();
          vector<bool> row(rowLen),col(colLen); // 标记出现0的行和列
          // 遍历矩阵，标记出现0的行和列
          for(int i=0;i<rowLen;i++){
              for(int j=0;j<colLen;j++){
                  if( matrix[i][j]==0 ){
                      row[i] = true;
                      col[j] = true;
                  }
              }
          }
          // 遍历矩阵，将在出现0的行和列的元素设为0
          for(int i=0;i<rowLen;i++){
              for(int j=0;j<colLen;j++){
                  if( row[i]||col[j]){
                      matrix[i][j] = 0;
                  }
              }
          }
      }
  };
  ```

- 

### 54.螺旋矩阵

-  将矩阵按照 **顺时针螺旋顺序** ，返回元素。

  <img src="C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260306214453307.png" alt="image-20260306214453307"  />

- 思路：利用“模拟法”，通过一个**方向数组**控制遍历路径，并在遇到边界或已访问节点时自动“右转”。

  ![image-20260306222445781](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260306222445781.png)

  ```c++
  class Solution {
      int directions[4][2] = {{0,1},{1,0},{0,-1},{-1,0}}; // 方向向量-右下左上，逆时针方向
  public:
      vector<int> spiralOrder(vector<vector<int>>& matrix) {
          int rowLen = matrix.size(),colLen = matrix[0].size();
          vector<int> ans(rowLen*colLen);
          int i=0,j=0; // 当前遍历的矩阵元素
          int di = 0; // 当前的前进方向
          for(int k=0; k<rowLen*colLen; k++){
              ans[k] = matrix[i][j];
              matrix[i][j] = INT_MAX;
              // 计算下一个位置，判断是否需要进行转向
              int x = i + directions[di][0]; // 行位置
              int y = j + directions[di][1]; // 列位置
              if( x<0 || x>=rowLen || y<0 || y>=colLen || matrix[x][y]==INT_MAX ){
                  di = (di+1)%4; // 右转90°
              }
              i = i + directions[di][0];
              j = j + directions[di][1];
          }
          return ans;
      }
  };
  ```

- 

### 48.旋转矩阵

-   *n* × *n* 的二维矩阵 `matrix` 表示一个图像。将图像顺时针旋转 90 度。不要使用另一个矩阵。

  <img src="https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg" alt="img" style="zoom:67%;" />

  ![image-20260306223714633](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260306223714633.png)

- 思路：先上下交换，再沿着对角线交换，就能得到最终元素了。

  ![8314c8f07fbc63bfeffea66756a8ca15](C:\Users\nwu\xwechat_files\wxid_bnf7qfxsd6jk22_0b29\temp\RWTemp\2026-03\8314c8f07fbc63bfeffea66756a8ca15.jpg)

  ```c++
  class Solution {
  public:
      void rotate(vector<vector<int>>& matrix) {
          int n = matrix.size();
          // 遍历矩阵，上下交换
          for(int i=0; i<n/2; i++){
              for(int j=0; j<n; j++){
                  swap(matrix[i][j],matrix[n-1-i][j]);
              }
          }
          // 遍历矩阵，沿对角线交换(只用遍历下三角)
          for(int i=0;i<n;i++){
              for(int j=0;j<i;j++){
                  swap(matrix[i][j],matrix[j][i]);
              }
          }
      }
  };
  ```

- 



### 240.搜索二维矩阵

-  搜索 `m x n` 矩阵 `matrix` 中的一个目标值 `target` 。

  <img src="https://assets.leetcode.cn/aliyun-lc-upload/uploads/2020/11/25/searchgrid2.jpg" alt="img" style="zoom: 67%;" />

  - 每行的元素从左到右升序排列。
  - 每列的元素从上到下升序排列。

- 思路：对矩阵的每一行分别执行二分查找。

  ```c++
  class Solution {
  public:
      bool searchMatrix(vector<vector<int>>& matrix, int target) {
          for(auto& row : matrix){
              // 在有序范围 [begin, end) 中，查找第一个大于或等于 target 的元素位置
              auto it = lower_bound(row.begin(),row.end(),target);
              if( it!=row.end() && *it==target ){
                  return true;
              }
          }
          return false;
      }
  };
  ```

- 