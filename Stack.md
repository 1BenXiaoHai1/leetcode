# Stack



## leetcode

### 20.有效的括号

- 给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

  ![image-20260311111902450](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311111902450.png)

- 思路：该算法利用 **栈 (Stack)** 的**“后进先出” (LIFO)** 特性来匹配括号。其基本逻辑是：**遇到左括号就压入栈中，遇到右括号就检查栈顶元素是否为对应的左括号。**

  ```c++
  class Solution {
  public:
      bool isValid(string s) {
          int n = s.size();
          if( n%2==1 ){ // 长度为偶数才能正确匹配
              return false;
          }
  
          unordered_map<char,char> rightToLeft = {
              {')', '('},
              {']', '['},
              {'}', '{'}
          };
          stack<char> stk;
          for( char ch: s){
              if(rightToLeft.count(ch)){ // 右括号则需要和栈中的左括号进行匹配
                  if( stk.empty() || stk.top() != rightToLeft[ch] ){ // 栈为空/栈顶左括号不匹配右括号
                      return false; // 匹配失败
                  }
                  stk.pop(); // 弹出栈顶元素
              }else{ // 把左括号压入栈中
                  stk.push(ch);
              }
          }
          return stk.empty();
      }
  };
  ```

- 

### 155.最小栈

- ![image-20260311114807674](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311114807674.png)

  ![image-20260311114825926](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311114825926.png)

- 思路：**辅助栈 (Auxiliary Stack)**.

  ![image-20260311115925973](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311115925973.png)

  ![image-20260311120035637](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311120035637.png)

  <img src="C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311120322111.png" alt="image-20260311120322111" style="zoom:67%;" />

  ```c++
  class MinStack {
      stack<int> valStk; // 存储入栈的元素
      stack<int> minStk; // 辅助栈，用于存储当前元素入栈时对应的栈内的最小元素
  public:
      MinStack() {
          minStk.push(INT_MAX);
      }
      
      void push(int val) {
          valStk.push(val);
          minStk.push(min(minStk.top(),val));
      }
      
      void pop() {
          valStk.pop();
          minStk.pop();
      }
      
      int top() {
          return valStk.top();
      }
      
      int getMin() {
          return minStk.top();
      }
  };
  
  /**
   * Your MinStack object will be instantiated and called as such:
   * MinStack* obj = new MinStack();
   * obj->push(val);
   * obj->pop();
   * int param_3 = obj->top();
   * int param_4 = obj->getMin();
   */
  ```

- 

### 394.字符串解码

- 给定一个经过编码的字符串，返回它解码后的字符串。编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。

  ![image-20260311121209167](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311121209167.png)

- 思路：

  ![image-20260311124129524](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311124129524.png)

  ![image-20260311124340714](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311124340714.png)

  ![image-20260311124410629](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311124410629.png)

  ![image-20260311124428930](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311124428930.png)

  ```c++
  class Solution {
  public:
      string decodeString(string s) {
          stack<char> stk;
  
          for(char c:s){
              // 把所有字符push进去，除了']'
              if (c != ']'){
                  stk.push(c);
              }else{
                  // 取出[]内的字符串
                  string sub="";
                  while(!stk.empty() && isalpha(stk.top())){
                      sub += stk.top();
                      stk.pop();
                  }   
                  // 反转字符串（从栈顶弹出，先进后出）
                  reverse(sub.begin(),sub.end());
                  stk.pop();// 去除'['
  
                  // 获取倍数数字
                  string numStr="";
                  while( !stk.empty() && isdigit(stk.top())){
                      numStr += stk.top();
                      stk.pop();
                  }
                  // 反转数字
                  reverse(numStr.begin(),numStr.end());
                  int cnt = stoi(numStr);
  
                  // 把字符串重复倍数次，再push回去
                  while( cnt>0 ){
                      for(char ch:sub){
                          stk.push(ch);
                      }
                      cnt--;
                  }
              }
          }
          // 取出栈中的str，输出
          string res="";
          while( !stk.empty() ){
              res+=stk.top();
              stk.pop();
          }
          // 反转字符串
          reverse(res.begin(),res.end());
  
          return res;
      }
  };
  ```

- 

### 739.每日温度

-  一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。

  ![image-20260311130617848](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311130617848.png)

- 思路：

  ![image-20260311132301354](C:\Users\nwu\AppData\Roaming\Typora\typora-user-images\image-20260311132301354.png)

  ```c++
  class Solution {
  public:
      vector<int> dailyTemperatures(vector<int>& temperatures) {
          int n = temperatures.size();
          vector<int> ans(n);
          stack<int> stk; // 用于存储温度的下标，需要计算天数差(j-i)
          // 遍历每一天的温度
          for( int i=0; i<n; i++ ){
              // 对于栈顶的那一天(stk.top())，找到了下一个更高温度(当前的第 i 天)
              while( !stk.empty() && temperatures[i] > temperatures[stk.top()] ){
                  int previousIndex = stk.top();
                  ans[previousIndex]=i - previousIndex; // 记录天数差
                  stk.pop();
              }
              stk.push(i); // 当前第i天对应的温度一直递减，则压入栈中
          }
          return ans;
      }
  };
  ```

- 