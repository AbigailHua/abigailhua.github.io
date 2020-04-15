---
layout:     post
title:      "剑指Offer"
subtitle:   ""
date:       2020-04-15 21:30:00
author:     "AH"
header-img: "img/post-bg-2015.jpg"
tags:
    - 刷题
---

# 剑指Offer

## 1.二维数组中的查找

### 题目描述

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

### 思路

从左下（或右上）开始搜索，如果当前元素==target，返回true；>，向上；<，向右。

### 代码

```C++
class Solution {
public:
    bool Find(int target, vector<vector<int>> array) {
        if(array.empty()) return 0;
        int n = array.size(), m = array[0].size();
        int i=n-1, j=0;
        int piv;
        while(i>=0 and j<m){
            piv = array[i][j];
            if(target==piv) return 1;
            else if(target>piv) j++;
            else i--;
        }
        return 0;
    }
};
```



## 2. 替换空格

### 题目描述

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

### 思路

第一遍正向遍历数空格；

第二遍逆向遍历挪数据

### 代码

```C++
class Solution {
public:
void replaceSpace(char *str,int length) {
        int cnt=0;
        for(int i=0; i<length; i++){
            if(str[i]==' ') ++cnt;
        }
        int pt=length-1;
        while(pt>=0){
            if(str[pt]!=' '){
                str[pt+2*cnt] = str[pt];
            }
            else{
                str[pt+2*cnt] = '0';
                str[pt+2*cnt-1] = '2';
                str[pt+2*cnt-2] = '%';
                --cnt;
            }
            --pt;
        }
	}
};
```



## 3. 从尾到头打印链表

### 题目描述

输入一个链表，按链表从尾到头的顺序返回一个ArrayList。

### 思路

1. 建一个栈，遍历链表入栈。

2. 弹栈，入vector

### 代码

```C++
#include <stack>
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        stack<int> S;
        vector<int> res;
        // push
        while(head!=NULL){
            int tmp = head->val;
            head = head->next;
            S.push(tmp);
        }
        // pop
        while(!S.empty()){
            int top = S.top();
            S.pop();
            res.push_back(top);
        }
        return res;
    }
};
```



## 4. 重建二叉树

### 题目描述

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

### 思路

递归

### 代码

```C++
class Solution {
public:
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        if(pre.size()==0) return NULL;
        TreeNode* root = new TreeNode(pre[0]);
        int i;
        for(i=0; i<vin.size();i++)
            if(vin[i]==pre[0]) break;
        
        vector<int> left_pre(pre.begin()+1, pre.begin()+i+1);
        vector<int> left_vin(vin.begin(), vin.begin()+i);
        vector<int> right_pre(pre.begin()+i+1, pre.end());
        vector<int> right_vin(vin.begin()+i+1, vin.end());
        
        root->left = reConstructBinaryTree(left_pre, left_vin);
        root->right = reConstructBinaryTree(right_pre, right_vin);
        return root;
    }
};
```



## 5. 用两个栈实现队列

### 题目描述

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

### 思路

stack1负责push，stack2负责pop。push和pop操作仅在另一个栈为空的时候进行。

### 代码

```C++
class Solution
{
public:
    void push(int node) {
        int tmp;
        if(stack1.empty()){
            while(!stack2.empty()){
                tmp = stack2.top();
                stack2.pop();
                stack1.push(tmp);
            }
        }
        stack1.push(node);
    }

    int pop() {
        int tmp;
        if(stack2.empty()){
            while(!stack1.empty()){
                tmp = stack1.top();
                stack1.pop();
                stack2.push(tmp);
            }
        }
        tmp = stack2.top();
        stack2.pop();
        return tmp;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```



## 6. 旋转数组的最小数字

### 题目描述

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。
输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。
例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。
NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

### 思路

遍历数组，return第一个比前一个数小的数

### 代码

```C++
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        int len = rotateArray.size();
        if(len==0) return 0;
        int prev = rotateArray[0], cur;
        for(int i=1; i<len; i++){
            cur = rotateArray[i];
            if(prev>cur) return cur;
            prev = cur;
        }
    }
};
```



## 7. 斐波那契数列

### 题目描述

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。

n<=39

### 思路

DP

### 代码

```C++
class Solution {
public:
    int Fibonacci(int n) {
        if(n<=1) return n;
        int * arr = new int[n];
        arr[0] = 0;
        arr[1] = 1;
        
        for(int i=2; i<n; i++)
            arr[i] = arr[i-1] + arr[i-2];
        
        return arr[n-2] + arr[n-1];
    }
};
```



## 8. 跳台阶

### 题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

### 思路

同斐波那契，只是起始条件要包含两格台阶。

### 代码

```C++
class Solution {
public:
	// Recursive
    int jumpFloor(int number) {
        if(number<=2) return number;
        return jumpFloor(number-1)+jumpFloor(number-2);
    }
    
    // Loop
    int jumpFloor(int number) {
        if(number<=2) return number;
        int * arr = new int[number];
        arr[0] = 0;
        arr[1] = 1;
        arr[2] = 2;
        for(int i=3; i<number; i++)
            arr[i] = arr[i-1] + arr[i-2];
        
        return arr[number-1]+arr[number-2];
    }
};
```



## 9. 变态跳台阶

### 题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

### 思路

挡板法

### 代码

```C++
class Solution {
public:
    int jumpFloorII(int number) {
        if(number==0) return 0;
        return (int)pow(2, number-1);
    }
};
```



## 10. 矩形覆盖

### 题目描述

我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？



比如n=3时，2*3的矩形块有3种覆盖方法：

<img src="https://uploadfiles.nowcoder.com/images/20200218/6384065_1581999858239_64E40A35BE277D7E7C87D4DCF588BE84" height="400">

### 思路

同斐波那契

### 代码

```C++
class Solution {
public:
    int rectCover(int number) {
        if(number<=2) return number;
        int *arr = new int[number+1];
        arr[0] = 0;
        arr[1] = 1;
        arr[2] = 2;
        for(int i=3; i<number+1; i++)
            arr[i] = arr[i-1] + arr[i-2];
        return arr[number];
    }
};
```



## 11. 二进制中1的个数

### 题目描述

输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

### 思路

很巧妙的方法，记补码的n从右到左第一个1在位置p，则补码的n-1与补码的n在p左侧的所有位完全一样，但从p位置向右（含）全部取反。所以n和n-1按位与就可以消除n最右侧的1。

### 代码

```C++
class Solution {
public:
     int  NumberOf1(int n) {
         int cnt = 0;
         while(n!=0){
             n = n & (n-1);
             cnt++;
         }
         return cnt;
     }
};
```



## 12. 数值的整数次方

### 题目描述

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

保证base和exponent不同时为0

### 思路

快速幂算法：

$$
base^{exp} =\left\{
\begin{aligned}
& base &\mbox{, if exp == 1} \\
& base^{\frac{exp}{2}\cdot\frac{exp}{2}} &\mbox{, if exp is even} \\
& base^{\frac{exp}{2}\cdot\frac{exp}{2}+1} &\mbox{, if exp is odd}
\end{aligned}
\right.
$$


### 代码

```C++
class Solution {
public:
    double Power(double base, int exponent) {
        if(base==0) return 0;
        if(exponent==0) return 1;
        if(exponent==1) return base;
        if(exponent<0) return 1.0/Power(base, -exponent);
        
        double half = Power(base, exponent>>1);
        if(exponent&1) return half*half*base;
        else return half*half;
    }
};
```



## 13. 调整数组顺序使奇数位于偶数前面

### 题目描述

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

### 思路

新建一个队列

遍历数组，奇数调整位置，偶数入队

### 代码

```C++
#include<queue>
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        queue<int> even;
        int pt=0;
        int tmp;
        for(int i=0; i<array.size();i++){
            if(array[i]&1){
                array[pt++] = array[i];
            }
            else{
                even.push(array[i]);
            }
        }
        while(!even.empty()){
            tmp = even.front();
            even.pop();
            array[pt++] = tmp;
        }
    }
};
```



## 14. 链表中倒数第k个结点

### 题目描述

输入一个链表，输出该链表中倒数第k个结点。

### 思路

设置两个指针res和post，初始都指向head。post先向后移动k个，然后post和res一起向后移动，直到post为NULL，返回res。

题不难，但是一直过不了，后来发现是并不保证链表长度至少为k，所以在下面代码里一定要加一条判断（标注释的那行）。

### 代码

```C++
class Solution {
public:
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        ListNode *res, *post;
        res = post = pListHead;
        for(int i=0; i<k; i++){
            if(post==NULL) return NULL;	// IMPORTANT！！！
            post = post->next;
        }
        while(post!=NULL){
            post = post->next;
            res = res->next;
        }
        return res;
    }
};
```





## 15. 反转链表

### 题目描述

输入一个链表，反转链表后，输出新链表的表头。

### 思路

头插法

### 代码

```C++
class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
        ListNode *res = new ListNode(0), *tmp;
        
        while(pHead!=NULL){
            tmp = pHead;
            pHead = pHead->next;
            tmp->next = res->next;
            res->next = tmp;
        }
        return res->next;
    }
};
```



## 16. 合并两个排序的链表

### 题目描述

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

### 思路

没啥特别的，就是一个一个往后插，如果一个链表到底了，一个没有，就把剩下所有的接上去。

### 代码

```C++
class Solution {
public:
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        ListNode *res=new ListNode(0);
        ListNode *tmp=res;
        while(pHead1 and pHead2){
            if(pHead1->val<pHead2->val){
                tmp->next = pHead1;
                pHead1 = pHead1->next;
                tmp = tmp->next;
            }
            else{
                tmp->next = pHead2;
                pHead2 = pHead2->next;
                tmp = tmp->next;
            }
        }
        if(pHead1) tmp->next = pHead1;
        if(pHead2) tmp->next = pHead2;
        return res->next;
    }
};
```



## 17. 树的子结构

### 题目描述

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

### 思路

由于那个ps，不能直接用HasSubtree进行递归，可以写一个isSubtree，判断r1和r2是不是一样的。

如果r1和r2一样，自然是子结构；或者r1->left和r2一样，也是子结构；或者r1->right和r2一样，也是子结构。

### 代码

```C++
class Solution {
public:
    bool isSubtree(TreeNode* r1, TreeNode* r2){
        if(r2==NULL) return true;
        if(r1==NULL) return false;

        if(r1->val==r2->val)
            return isSubtree(r1->left, r2->left) and isSubtree(r1->right, r2->right);
        return false;
    }
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        if(pRoot1==NULL or pRoot2==NULL) return false;
        return isSubtree(pRoot1, pRoot2) or
            HasSubtree(pRoot1->left, pRoot2) or
            HasSubtree(pRoot1->right, pRoot2);
    }
};
```



## 18. 二叉树的镜像

### 题目描述

操作给定的二叉树，将其变换为源二叉树的镜像。

#### 输入描述:


> 二叉树的镜像定义：源二叉树 
> 	     8
> 	    /  \
> 	  6   10
>  	 / \  / \
>  	5  7 9 11
>
> 镜像二叉树
> 	     8
> 	    /  \
> 	  10   6
>  	 / \  / \
> 	11 9 7  5

### 思路

递归：

如果结点为空，或左右子树都为空，直接返回；

交换左右子树；

左子树求镜像，右子树求镜像

### 代码

```C++
class Solution {
public:
    void Mirror(TreeNode *pRoot) {
        if(pRoot==NULL or pRoot->left==NULL and pRoot->right==NULL) return;

        TreeNode *tmp=pRoot->left;
        pRoot->left = pRoot->right;
        pRoot->right = tmp;
        Mirror(pRoot->left);
        Mirror(pRoot->right);
    }
};
```



## 19. 顺时针打印矩阵

### 题目描述

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

### 思路

设定四个boundary，按顺序把数字一个一个push进vector，但是血泪教训！一定要在顺时针绕过完整一圈之后再更新boundary！

### 代码

```C++
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> res;
        if(matrix.empty()) return res;

        int i=0, j=0;
        int width=matrix[0].size(), length=matrix.size();
        int area = width*length;
        int top=0, right=width-1, bottom=length-1, left=0;

        while(res.size()<area){
            res.push_back(matrix[i][j]);
            // right
            if(i==top){
                if(j<right) j++;
                else if(j==right) i++;
                continue;
            }
            // down
            if(j==right){
                if(i<bottom) i++;
                else if(i==bottom) j--;
                continue;
            }
            // left
            if(i==bottom){
                if(j>top) j--;
                else if(j==top) i--;
                continue;
            }
            // up
            if(j==left){
                if(i>top+1) i--;
                else if(i==top+1) {j++; left++; top++; right--; bottom--;}
                continue;
            }
        }
        return res;
    }
};
```



## 20. 包含min函数的栈

### 题目描述

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

注意：保证测试中不会当栈为空的时候，对栈调用pop()或者min()或者top()方法

### 思路

stack1是保存数据的栈，除此之外还要设立一个最小元素栈stack2。

如果stack2栈空（其实也是stack1栈空）或者新元素比top更小才入栈（可以理解为栈底到栈顶，元素值越来越大）。

出栈的时候要查看待出栈元素是不是stack2栈顶，如果是的话，两个栈一起出栈。

### 代码

```c++
class Solution {
public:
    stack<int> stack1, stack2;
    void push(int value) {
        stack1.push(value);
        if(stack2.empty()) stack2.push(value);
        else if(stack2.top()>value) stack2.push(value);
    }
    void pop() {
        if(stack1.top()==stack2.top()) stack2.pop();
        stack1.pop();
    }
    int top() {
        return stack1.top();
    }
    int min() {
        return stack2.top();
    }
};
```



## 21. 栈的压入、弹出序列

### 题目描述

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

### 思路

用一个栈模拟入栈的过程，如果栈顶元素和popV的当前元素一样，就出栈，直到不一样。返回栈是否为空。

### 代码

```C++
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        if(pushV.size()==0 or popV.size()==0) return false;
        stack<int> s;

        int popIndex = 0;
        for(int i=0; i<pushV.size();i++){
            s.push(pushV[i]);
            while(!s.empty() and s.top()==popV[popIndex]){
                s.pop();
                popIndex++;
            }
        }
        return s.empty();
    }
};
```



## 22. 从上往下打印二叉树

### 题目描述

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

### 思路

本题就是二叉树的层次遍历，用队列，入队顺序：左孩子、右孩子。

复习一下二叉树的dfs和bfs：

- dfs用栈，入栈顺序：右孩子、左孩子
- bfs就是层次遍历

### 代码

```C++
class Solution {
public:
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        queue<TreeNode*> q;
        vector<int> res;
        if(!root) return res;
        TreeNode *tmp;
        q.push(root);
        while(!q.empty()){
            tmp = q.front();
            q.pop();
            res.push_back(tmp->val);
            if(tmp->left) q.push(tmp->left);
            if(tmp->right) q.push(tmp->right);
        }
        return res;
    }
};
```



