# Binary Tree



## leetcode

### 94.二叉树的中序遍历

-  遍历二叉树，返回中序遍历序列。

  <img src="https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg" alt="img" style="zoom: 50%;" />

- 思路：递归遍历。左根右遍历序列。

  ```c++
  class Solution {
  public:
      void inorder(TreeNode* root,vector<int>& res){
          // 中序遍历-左根右
          if(!root){ // 终止条件
              return;
          }
          // 左结点
          inorder(root->left,res);
          // 根节点
          res.push_back(root->val);
          // 右结点
          inorder(root->right,res);
      }
  
      vector<int> inorderTraversal(TreeNode* root) {
          vector<int> res;
          inorder(root,res);
          return res;
      }
  };
  ```

- 

### 95.二叉树的最大深度

- 给定一个二叉树 `root` ，返回其最大深度（根节点到最远叶节点之间的节点数）。

  <img src="https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg" alt="img" style="zoom:50%;" />

  ![image-20260306122257145](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260306122257145.png)

-  思路：递归遍历。最大深度=1+max(左子树最大深度，右子树最大深度)。

  ```c++
  class Solution {
  public:
      int maxDepth(TreeNode* root) {
          if(root == nullptr) {
              return 0;
          } else {
              int left = maxDepth(root->left);
              int right = maxDepth(root->right);
              return std::max(left, right) + 1;
          }
      }
  };
  ```

- 

### 226.翻转二叉树

-  反转二叉树，镜像对称。

  ![img](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

  ![image-20260306124739653](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260306124739653.png)

- 思路：递归遍历。先将左右子树均反转完，然后再交换当前节点的左右子树。

  ```c++
  class Solution {
  public:
      TreeNode* invertTree(TreeNode* root) {
          if( root==nullptr ) return nullptr; // 终止条件
          // 反转左子树
          TreeNode *left = invertTree(root->left); 
          // 反转右子树
          TreeNode *right = invertTree(root->right);
          // 交换当前节点的左右子树
          root->left = right;
          root->right = left;
          return root;
      }
  };
  ```

-  

## 101.对称二叉树

- 检查一棵二叉树是否轴对称。

  <img src="https://pic.leetcode.cn/1698026966-JDYPDU-image.png" alt="img" style="zoom:67%;" />

- 思路：递归遍历。

  ![image-20260306132255473](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260306132255473.png)

  <img src="https://pic.leetcode.cn/1599398062-PbkpuX-Picture1.png" alt="Picture1.png" style="zoom: 33%;" />

  ```c++
  class Solution {
  public:
      bool isSymmetric(TreeNode* root) {
          if(root==nullptr) return true;
          else return recur(root->left,root->right);
      }
      bool recur(TreeNode* L,TreeNode* R){
          if(L==nullptr&&R==nullptr) return true;
          if(L==nullptr||R==nullptr||L->val!=R->val) return false;
          return recur(L->left,R->right)&&recur(R->left,L->right);
      }
  };
  ```

-  

### 543.二叉树的直径

-  给一棵二叉树，返回树的直径。二叉树的 **直径** 是指树中任意两个节点之间最长路径的 **长度** 。

  <img src="https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg" alt="img" style="zoom:67%;" />

  ![image-20260306133659136](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260306133659136.png)

- 思路：左子树最大长度+右子树最大长度

  ```c++
  class Solution {
      int res = 0; // 存储全局最大值
  public:
      int diameterOfBinaryTree(TreeNode* root) {
          res = 0;
          maxDepth(root);
          return res;
      }
  
      int maxDepth(TreeNode * root){
          if(root==nullptr) return 0;
          int LH = maxDepth(root->left);
          int RH = maxDepth(root->right);
          res = max(res,LH+RH); // 局部最大值，经过当前 root 节点的路径长度=左深度 +右深度
          return 1+std::max(LH,RH); // 最大深度
      }
  };
  ```

- 

###  102.二叉树的层序遍历

-  返回二叉树层序遍历的结果。

- 思路：二叉树的BFS广度优先搜索。借助**队列**来实现。将本层全部节点打印到一行，并将下一层全部节点加入队列，以此类推，即可分为多行打印。

  <img src="C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260306165734237.png" alt="image-20260306165734237" style="zoom:67%;" />

  <img src="C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260306165751290.png" alt="image-20260306165751290" style="zoom:67%;" />

  <img src="C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260306165809734.png" alt="image-20260306165809734" style="zoom:67%;" />

  

  

  ```c++
  class Solution {
  public:
      vector<vector<int>> levelOrder(TreeNode* root) {
          vector<vector<int>> res;
          queue<TreeNode*> q;
          if (root != nullptr) q.push(root);
          while(!q.empty()){
              vector<int> temp; // 用来存储该层的结果
              int levelSize = q.size(); // 当前层的元素个数
              for(int i=0;i<levelSize;i++){
                  TreeNode* head = q.front();
                  q.pop();
                  temp.push_back(head->val);
                  // 将该层结点的左右孩子压入队列中
                  if( head->left!=nullptr ) q.push(head->left);
                  if( head->right!=nullptr ) q.push(head->right); 
              }
              res.push_back(temp);
          }
          return res;
      }
  };
  ```

- 

### 108.将有序数组转换为二叉搜索树

-  给定一个按升序排列的数组，转换为平衡二叉树（**平衡二叉树** 是指该树所有节点的左右子树的高度相差不超过 1，左子节点 < 根节点 < 右子节点，二叉搜索树的中序遍历是升序序列）。

  ![img](https://assets.leetcode.com/uploads/2021/02/18/btree1.jpg)

  ![image-20260306171355160](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260306171355160.png)

- 思路：

  - 首先确定**二叉搜索树的根节点**。选择**中间数字**作为二叉搜索树的根节点，这样分给左右子树的数字个数相同或只相差 1，可以使得树保持平衡。

    <img src="https://assets.leetcode.cn/solution-static/108/108_fig3.png" alt="fig3" style="zoom: 33%;" />

    -  如果数组长度是**奇数**，则根节点的选择是**唯一**的。
    -  如果数组长度是**偶数**，则可以选择**中间位置左边的数字**作为根节点或者选择**中间位置右边的数字**作为根节点。

  - 左子树和右子树分别是平衡二叉搜索树，因此可以通过**递归的方式**创建平衡二叉搜索树。

  - 三种方法

    -  中序遍历，总是选择中间位置左边的数字作为根节点——*mid*=(*left*+*right*)/2，整数除法。
    -  中序遍历，总是选择中间位置右边的数字作为根节点——*mid*=(*left*+*right*+1)/2
    -  中序遍历，选择任意一个中间位置数字作为根节点——*mid*=(*left*+*right*)/2 和 *mid*=(*left*+*right*+1)/2 两者中随机选择一个，mid = (left + right + rand.nextInt(2)) / 2;

  ```c++
  /**
   * Definition for a binary tree node.
   * struct TreeNode {
   *     int val;
   *     TreeNode *left;
   *     TreeNode *right;
   *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
   *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
   *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
   * };
   */
  class Solution {
  public:
      TreeNode* sortedArrayToBST(vector<int>& nums) {
          return buildBST(nums,0,nums.size() - 1);        
      }
      TreeNode* buildBST(vector<int>& nums,int left,int right){
          if( left>right ){
              return nullptr; // 终止条件
          }
          // 总是选择中间位置左边的数字作为根结点
          int mid = (left+right)/2; // 整数除法
          TreeNode* root = new TreeNode(nums[mid]);
          root->left = buildBST(nums,left,mid-1);// 左子树
          root->right = buildBST(nums,mid+1,right);// 右子树
          return root;
      }
  };
  ```

-  

-  

### 98.验证二叉搜索树

-  二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

  <img src="https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg" alt="img" style="zoom:67%;" />

  ![image-20260309134744892](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309134744892.png)

  - 节点的左子树只包含 **严格小于** 当前节点的数。
  - 节点的右子树只包含 **严格大于** 当前节点的数。
  - 所有左子树和右子树自身必须也是二叉搜索树。

- 思路：递归判断。

  ![image-20260309140033924](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309140033924.png)

  ```c++
  /**
   * Definition for a binary tree node.
   * struct TreeNode {
   *     int val;
   *     TreeNode *left;
   *     TreeNode *right;
   *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
   *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
   *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
   * };
   */
  class Solution {
  public:
      bool isValidBST(TreeNode* root) {
          return check(root,LONG_MIN,LONG_MAX); // 根节点的范围在(-∞,+∞)
      }
  
      bool check(TreeNode* root, long long lower, long long upper){
          if(root==nullptr){ 
              return true;
          }
           // 当前节点的值必须严格大于下界，且严格小于上界
          if( root->val <= lower || root->val >= upper ){
              return false;
          }
          // 检查左右子树的合法性
          return check(root->left,lower,root->val)&&check(root->right,root->val,upper);
      }
  };
  ```

- 

### 230.二叉搜索树中第k小的元素

-  一个二叉搜索树的根节点 `root` ，和一个整数 `k` ，找到其中第k小的元素。

  ![img](https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg)

  ![image-20260309141842344](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309141842344.png)

-  思路：中序遍历，二叉搜索树 (BST) 的中序遍历 (In-order Traversal) 是按键值增加的有序序列。

  ![image-20260309143237513](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309143237513.png)

  ```c++
  class Solution {
      int cnt = 0;
      int ans;
  public:
      int kthSmallest(TreeNode* root, int k) {
          traverse(root,k);
          return ans;
      }
  
      void traverse(TreeNode* root, int k){
          if(root==nullptr) return;
          // 中序遍历-左根右
          traverse(root->left,k);
          cnt++;
          if(cnt==k){
              ans = root->val;
              return;
          }
          traverse(root->right,k);
      }
  };
  ```

- 

### 199.二叉树的右视图

- 一个二叉树的 **根节点** `root`，站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

  <img src="https://assets.leetcode.com/uploads/2024/11/24/tmpd5jn43fs-1.png" alt="img" style="zoom: 80%;" />

  ![image-20260309144455620](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309144455620.png)

-  思路：层序遍历同时记录最后出队的元素。每一层的最右侧元素就是从右边看的结果。

  ```c++
  class Solution {
  public:
      vector<int> rightSideView(TreeNode* root) {
          vector<int> result;
          if (root == nullptr) return result;
          queue<TreeNode*> Q;
          Q.push(root);
          while(!Q.empty()){
              int len = Q.size(); // 当前层的大小
              // 层序遍历，当遍历到最后一个结点时(len-1)，把结果存储res
              for(int i=0;i<len;i++){
                  TreeNode * temp = Q.front();
                  Q.pop();
                  if(temp->left!=nullptr) Q.push(temp->left);
                  if(temp->right!=nullptr) Q.push(temp->right);
                  if(i == len - 1){
                      result.push_back(temp->val);
                  }
              }
          }
          return result;
      }
  };
  ```

- 

### 114.二叉树展开为链表

- 将二叉树的根结点 `root` 展开为一个单链表。展开后的单链表应该与二叉树**先序遍历**顺序相同。单链表应该同样使用 `TreeNode` 。

  <img src="https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg" alt="img" style="zoom: 50%;" />

  ![image-20260309153846310](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309153846310.png)

- 思路： 将二叉树展开为单链表之后，单链表中的节点顺序即为二叉树的前序遍历访问各节点的顺序。

  ```c++
  class Solution {
  public:
      void flatten(TreeNode* root) {
          vector<TreeNode*> l;
          preorderTraversal(root, l); // 先序遍历序列就是展平后的结果
          // 遍历链表，把左结点改为null，右结点指向先序序列的下一结点
          for( int i=1;i<l.size();i++ ){
              TreeNode *prev = l[i-1];
              TreeNode *curr = l[i];
              prev->left=nullptr;
              prev->right = curr;
          }
      }
  
      void preorderTraversal(TreeNode* root, vector<TreeNode*> &l){
          if( root==NULL ){
              return;
          }
          l.push_back(root);
          preorderTraversal(root->left,l);
          preorderTraversal(root->right,l);
      }
  };
  ```

  

- 

### 105.从前序与中序遍历构造二叉树

- 两个整数数组 `preorder`(**先序遍历**) 和 `inorder` (**中序遍历**)。

  <img src="https://assets.leetcode.com/uploads/2021/02/19/tree.jpg" alt="img" style="zoom:67%;" />

  ![image-20260309162949669](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309162949669.png)

- 思路：

  ![image-20260309163935402](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309163935402.png)

  ![image-20260309170158693](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309170158693.png)

  ```c++
  class Solution {
      unordered_map<int, int> nodeToIndex;
  public:
      TreeNode* buildBT(vector<int>& preorder,vector<int>& inorder, 
      int preorder_left, int preorder_right, 
      int inorder_left, int inorder_right){
          if (preorder_left > preorder_right) {
              return nullptr;
          }
  
          int preorder_root = preorder_left; // 前序遍历中的第一个节点就是根节点
          int inorder_root = nodeToIndex[preorder[preorder_root]]; // 根据前序遍历结点找根节点在中序遍历中的位置
          // 根节点构造
          TreeNode* root = new TreeNode(preorder[preorder_root]);
          int left_subtree_size = inorder_root - inorder_left; // 从中序序列推出左子树的大小
          // 递归构造左子树
          // 先序遍历中「从 左边界+1 开始的 size_left_subtree」个元素——中序遍历中「从 左边界 开始到 根节点定位-1」的元素
          root->left = buildBT(preorder, inorder, 
          preorder_left + 1, preorder_left + left_subtree_size, 
          inorder_left, inorder_root - 1
          );
          // 递归构造右子树
          // 先序遍历中「从 左边界+1+左子树节点数目 开始到 右边界」的元素——中序遍历中「从 根节点定位+1 到 右边界」的元素
          root->right = buildBT(preorder, inorder,
          preorder_left + left_subtree_size + 1, preorder_right, 
          inorder_root + 1, inorder_right
          );
          return root;
  
      }
  
      TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
          int n = preorder.size();
          // 构建哈希映射，用于快速定位中序遍历的根节点
          for( int i=0; i<n; i++ ){
              nodeToIndex[inorder[i]] = i;
          }
          return buildBT( preorder,inorder,
          0,n - 1,
          0, n - 1 );
      }
  };
  ```

- 

### 437.路径综合Ⅲ

-  一个二叉树的根节点 `root` ，和一个整数 `targetSum` ，求该二叉树里节点值之和等于 `targetSum` 的 **路径** 的数目。**路径** 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的。

  <img src="https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg" alt="img" style="zoom: 67%;" />

  ![image-20260309172150136](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309172150136.png)

  

- **思路**：**双重递归 (Double Recursion)**。**遍历树上的每一个节点**，把每个节点都当作“起点”，然后向下搜索有多少条路径的和等于 `targetSum`。

  - `dfs(node, current_sum)`: 以 `node` 为**起点**，向下寻找和为 `targetSum` 的路径数量。
  - `pathSum(root, targetSum)`: 遍历整棵树，对**每个节点**都调用一次 `dfs`。

  ```c++
  class Solution {
  public:
      int dfs(TreeNode* root, long long currentSum,long long targetSum){
          if( !root ) return 0;
          int count = 0;
          // 根
          currentSum += root->val; // 更新当前路径长度
          if(currentSum == targetSum) count++;
          // 左
          count += dfs(root->left,currentSum,targetSum);
          // 右
          count += dfs(root->right,currentSum,targetSum);
  
          return count;
      }
  
      int pathSum(TreeNode* root, int targetSum) {
          // 遍历每个结点，将其作为起点，进行深度遍历，计算和为targetSum的路径数量
          if(!root) return 0;
          // 以当前节点为起点的和为targetSum的路径数
          int count = dfs(root,0,targetSum);
          // 递归计算左子树中所有结点作为起点的路径数
          count += pathSum(root->left,targetSum);
          // 递归计算右子树中所有结点作为起点的路径数
          count += pathSum(root->right,targetSum);
  
          return count;
      }
  };
  ```

  

- 

### 236.二叉树的最近公共祖先

-  给定一个二叉树, 找到该树中两个指定节点的**最近公共祖先**。

  ![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

  ![image-20260309175344961](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309175344961.png)

- 思路：

  ![image-20260309180424158](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260309180424158.png)

  ```
  
  ```

  

- 

- 