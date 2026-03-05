# LinkList



## leetcode

### 160.相交链表

- 给定两个单链表的头节点 `headA` 和 `headB`，找到两个单链表相交的起始节点。

  <img src="https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png" alt="img" style="zoom: 67%;" />

- 思路：先遍历一遍A链表，把所有的节点都加入哈希集合。再遍历一遍B链表，检查第一个在哈希集合出现的B链表结点，就是起始交点。

  ```c++
  /**
   * Definition for singly-linked list.
   * struct ListNode {
   *     int val;
   *     ListNode *next;
   *     ListNode(int x) : val(x), next(NULL) {}
   * };
   */
  class Solution {
  public:
      ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
          unordered_set<ListNode *> visited;// 存储结点索引
          // 先遍历一遍A链表，把所有的节点都加入哈希集合
          ListNode *temp = headA;
          while( temp!=nullptr ){
              visited.insert(temp);
              temp=temp->next;
          }
          // 再遍历一遍B链表，检查第一个在哈希集合出现的B链表结点，就是起始交点
          temp = headB;
          while( temp!=nullptr ){
              if(visited.count(temp)){
                  return temp;
              }
              temp=temp->next;
          }
          return nullptr;
      }
  };
  ```

- 

### 206.反转链表

-  给一个单链表，反转链表。

  <img src="https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg" alt="img" style="zoom:67%;" />

- 思路：遍历一遍链表。把每个结点的pnext指针换成前驱就好。

  ```c++
  /**
   * Definition for singly-linked list.
   * struct ListNode {
   *     int val;
   *     ListNode *next;
   *     ListNode() : val(0), next(nullptr) {}
   *     ListNode(int x) : val(x), next(nullptr) {}
   *     ListNode(int x, ListNode *next) : val(x), next(next) {}
   * };
   */
  class Solution {
  public:
      ListNode* reverseList(ListNode* head) {
          ListNode *pre = nullptr,*post = head; // 修改post指针的后驱为pre
          while(post!=nullptr){
              ListNode *temp = post->next; // 暂存当前处理结点post的后驱
              post->next=pre;
              // pre和post均后移
              pre=post;
              post=temp;
          }
          return pre;
      }
  };
  ```

### 0234.回文链表

- 判断一个链表是否为回文链表（从前向后和从后向前读都相同的序列）。

  <img src="https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg" alt="img" style="zoom:67%;" />

- 思路：遍历链表，把值复制到数组中，然后定义首尾双指针，遍历判断元素是否相等。

  ```c++
  class Solution {
  public:
      bool isPalindrome(ListNode* head) {
          vector<int> nums;
          // 将链表元素移动到数组
          ListNode * p = head;
          while( p!=nullptr ){
              nums.push_back(p->val);
              p = p->next;
          }
          // 首尾双指针，判断是否为回文序列
          for(int front = 0,end = nums.size()-1;front<end;front++,end--){
              if(nums[front]!=nums[end]) return false;
          }
          return true;
      }
  };
  ```

  

-  

### 141.环形链表

-  判断链表中是否有环。

  <img src="https://assets.leetcode.cn/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png" alt="img" style="zoom:67%;" />

-  思路：哈希表，遍历链表，每次需要判断当前顶点是否访问过。如果访问过，则说明是环形链表。

  ```c++
  class Solution {
  public:
      bool hasCycle(ListNode *head) {
          unordered_set<ListNode*> visited;
          ListNode *p = head;
          while( p!=nullptr ){ // 如果是环形链表则一直无法退出该循环
              if( visited.count(p)){
                  return true;
              }
              visited.insert(p);
              p=p->next;
          }
          return false;
      }
  };
  ```



### 142.环形链表2

- 给定一个链表，返回链表开始入环的第一个节点。无环则返回null。

- 思路：哈希表。遍历链表并检测是否在哈希表中出现。出现则返回结点，反之返回null。

  <img src="https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png" alt="img" style="zoom:67%;" />

  ```c++
  class Solution {
  public:
      ListNode *detectCycle(ListNode *head) {
          unordered_set<ListNode*> visited;
          ListNode *p = head;
          while(p!=NULL){
              if(visited.count(p)){
                  return p;
              }
              visited.insert(p);
              p=p->next;
          }
          return NULL;
      }
  };
  ```

-  

### 21.合并两个有序链表

- 将两个升序链表合并为一个新的 **升序** 链表并返回。

- 思路：新建一个链表。双指针法，遍历两个链表，每次将最小的元素插入到新链表中。

  ```c++
  class Solution {
  public:
      ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
          ListNode *listMerge = new ListNode(-1); // 存储合并后的链表，虚拟头节点
          ListNode *p = listMerge; // 始终指向链表的尾部
          ListNode *l1 = list1, *l2 = list2;
          while(l1!=NULL&&l2!=NULL){
              if( l1->val < l2->val){
                  p->next = new ListNode(l1->val);
                  l1=l1->next;
              }else{
                  p->next = new ListNode(l2->val);
                  l2=l2->next;
              }
              // 移动到尾部，等待下一次插入
              p=p->next;
          }
          if(l1){
              p->next=l1;
          }else{
              p->next=l2;
          }
          ListNode* result = listMerge->next;
          return result;
      }
  };
  ```

-  

### 2.两数相加

-  两个链表，存储两个数字。链表的每个结点存储一位数，而且数字是逆序排列的。

  <img src="https://assets.leetcode.cn/aliyun-lc-upload/uploads/2021/01/02/addtwonumber1.jpg" alt="img" style="zoom:67%;" />

  ![image-20260305213634630](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260305213634630.png)

- 思路：新建一个链表来存储结果。遍历两个链表，开始求和（注意考虑进位）。

  ```c++
  class Solution {
  public:
      ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
          ListNode* res = new ListNode(-1);
          ListNode* p = res; // 始终指向尾部
          int carry = 0; //进位
          while(l1!=NULL||l2!=NULL){
              int sum = 0;// 每一位的相加结果
              if(l1!=NULL){
                  sum += l1->val;
                  l1=l1->next;
              }
              if(l2!=NULL){
                  sum += l2->val;
                  l2=l2->next;
              }
              if(carry) sum++;// 上一轮的进位
              carry = (sum>9)?1:0; // 这一轮的进位
              // 存储结果
              ListNode *temp = new ListNode(sum%10);
              p->next = temp;
              p=p->next;
          }
          // 处理最高位的进位
          if(carry) p->next = new ListNode(1);
  
          return res->next;
      }
  };
  ```

- 

### 19.删除链表的倒数第N个结点

-  删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

  ![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

- 思路：双指针法，右指针先走n步（左右指针插值为n），然后左右指针同时走，最终右指针到达左边界时，左指针指向倒数第n个结点。

  ```C++
  class Solution {
  public:
      ListNode* removeNthFromEnd(ListNode* head, int n) {
          ListNode *dummy = new ListNode(0,head);
          ListNode *l=dummy,*r=dummy;
          int i = 0;
          while( i<n ){
              r=r->next;
              i++;
          }
          while( r->next!=NULL ){
              // pre = pre->next;
              r=r->next;
              l=l->next;
          }
          // 删除结点
          // pre->next = l->next;
          ListNode *temp = new ListNode();
          temp = l->next;
          l->next = temp->next;
          delete temp;
  
  
          return dummy->next;
      }
  };
  ```

  

- 

