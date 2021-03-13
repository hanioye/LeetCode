# 剑指Offer个人题解

先把代码给出来，感觉题目都还好，主要是透过题目看清后面的基础知识点，进行延伸。
比如求全排列，是不是只会dfs去搜就好了，是不是要了解下[next_permutation](https://blog.csdn.net/sgh666666/article/details/87953158)背后的原理？</br>
比如求1-n有多少个1，虽然可以用数学推出，但是我们是不是可以了解下数位DP的知识？</br>
碰到Top K 问题我们是不是可以总结下快排？堆？
说到排序，是否牢记于心各个排序的特点？</br>
比如  60. n 个骰子的点数  这道题没写好的, 是不是要训练一下DP 与 记忆化搜索?



# 数组中重复的数字

[数组中重复的数字](https://www.nowcoder.com/practice/623a5ac0ea5b4e5f95552655361ae0a8?tpId=13&tqId=11203&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```c++
class Solution {
public:
    // Parameters:
    //        numbers:     an array of integers
    //        length:      the length of array numbers
    //        duplication: (Output) the duplicated number in the array number
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    bool duplicate(int numbers[], int length, int* duplication) {
        if(!numbers || length <= 0)return false;
        
        for(int i = 0;i<length;i++){
            while(i != numbers[i]){
                if(numbers[i] == numbers[numbers[i] ])
                    {duplication[0] = numbers[i];return true;}
                swap(numbers[i],numbers[numbers[i] ]);
            }
        }
        return false;
    }
};
```











# 二维数组中的查找

[二维数组中的查找](https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e?tpId=13&tqId=11154&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        int row = 0, col = array[0].size()-1;
        
        while(row <= array.size()-1 && col >=0){
            if(array[row][col] == target)return true;
            else if(array[row][col] < target)row++;
            else col--;
        }
        return false;
    }
};
```









# 替换空格

[替换空格](https://www.nowcoder.com/practice/4060ac7e3e404ad1a894ef3e17650423?tpId=13&tqId=11155&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
	void replaceSpace(char *str,int length) {
        int cnt = 0;
        for(int i = 0;i<length;i++)if(str[i] == ' ')++cnt;
        
        for(int i = length-1;i>=0;i--){
            if(str[i] != ' '){
                str[i+2*cnt] = str[i];
            }
            else{
                --cnt;
                str[i + 2*cnt] = '%';
                str[i + 2*cnt+1] = '2';
                str[i + 2*cnt+2] = '0';
            }
        }
	}
};
```





# 从尾到头打印链表

[从尾到头打印链表](https://www.nowcoder.com/practice/d0267f7f55b3412ba93bd35cfa8e8035?tpId=13&tqId=11156&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int>ans;
    
    void solve(ListNode* head){
        if(!head)return ;
            
        solve(head->next);
        ans.push_back(head->val);
    }
    
    vector<int> printListFromTailToHead(ListNode* head) {
        solve(head);
        return ans;
    }
};
```

# 重建二叉树

[重建二叉树](https://www.nowcoder.com/practice/8a19cbe657394eeaac2f6ea9b0f6fcf6?tpId=13&tqId=11157&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    
    TreeNode* solve(vector<int>::iterator pre,vector<int>::iterator in,int len){
        if(len == 0)return nullptr;
        TreeNode *T = new TreeNode(*pre);
        int lenL = 0;
        while(*(in+lenL)!=*pre)++lenL;
        int lenR = len - lenL - 1;
        T->left = solve(pre+1,in,lenL);
        T->right = solve(pre+lenL+1,in+lenL+1,lenR);
        return T;
    }
    
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        return solve(pre.begin(),vin.begin(),pre.size());
    }
};
```

#  二叉树的下一个结点

 [二叉树的下一个结点](https://www.nowcoder.com/practice/9023a0c988684a53960365b889ceaf5e?tpId=13&tqId=11210&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
/*
struct TreeLinkNode {
    int val;
    struct TreeLinkNode *left;
    struct TreeLinkNode *right;
    struct TreeLinkNode *next;
    TreeLinkNode(int x) :val(x), left(NULL), right(NULL), next(NULL) {
        
    }
};
*/
class Solution {
public:
    TreeLinkNode* GetNext(TreeLinkNode* node)
    {
        if(!node)return nullptr;
        
        if(node->right){//有右儿子
            node = node->right;
            while(node->left){
                node = node -> left;
            }
            return node;
        }
        
        if(node->next && node->next->left == node)return node->next;//是爸爸的左儿子
        if(node->next){
            node = node->next;
            while(node->next &&node->next->left != node)node = node->next;//找到第一个左爸爸（qwq）
            return node->next;
        }
        
        return nullptr;
    }
};
```

# 用两个栈实现队列

[用两个栈实现队列](https://www.nowcoder.com/practice/54275ddae22f475981afa2244dd448c6?tpId=13&tqId=11158&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)



```cpp
class Solution
{
public:
    void push(int node) {
        s1.push(node);
    }

    int pop() {
        if(s2.size()){
            int x = s2.top();
            s2.pop();
            return x;
            
        }
        else{
            
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();
            }
            
            if(s2.size()){
            int x = s2.top();
            s2.pop();
            return x;
            
            }
        }
        return 0;
        
    }

private:
    stack<int> s1;
    stack<int> s2;
};
```

# 斐波那契数列

[斐波那契数列](https://www.nowcoder.com/practice/c6c7742f5ba7442aada113136ddea0c3?tpId=13&tqId=11160&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

：矩阵快速幂另外总结

```cpp
class Solution {
public:
    int Fibonacci(int n) {
        long long a[111];
        a[0] = 0;a[1] = 1;a[2] = 1;
        for(int i = 3;i<=n;i++)
            a[i] = a[i-1] + a[i-2];
        
        return a[n];
    }
};
```

# 跳台阶

[跳台阶](https://www.nowcoder.com/practice/8c82a5b80378478f9484d87d1c5f12a4?tpId=13&tqId=11161&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    int jumpFloor(int number) {
        int a[111];
        a[0] = 1;a[1] = 1;
        for(int i = 2;i<=number;i++)
            a[i] = a[i-1] + a[i-2];
        return a[number];
    }
};
```

# 矩形覆盖

[矩形覆盖](https://www.nowcoder.com/practice/72a5a919508a4251859fb2cfb987a0e6?tpId=13&tqId=11163&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    int rectCover(int n) {
        int a[111];
        a[0] = 0;a[1]=1;a[2]=2;
        for(int i = 3;i<=n;i++)a[i] = a[i-1] +a[i-2];
        return a[n]; 
    }
};
```

# 变态跳台阶

[变态跳台阶](https://www.nowcoder.com/practice/22243d016f6b47f2a6928b4313c85387?tpId=13&tqId=11162&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    int jumpFloorII(int num) {
        int ans = pow(2,num-1);
        return ans;
    }
};
```

# 旋转数组的最小数字

[旋转数组的最小数字](https://www.nowcoder.com/practice/9f3231a991af4f55b95579b44b7a01ba?tpId=13&tqId=11159&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    int minNumberInRotateArray(vector<int> a) {
        if(!a.size())return 0;
        
        int mid,l = 0,r = a.size()-1;
        
        while(l+1<r){
            mid = l+r>>1;
            if(a[mid] == a[l])l++;
            if(a[mid] == a[r])r--;
            if(a[mid] > a[r])l = mid;
            if(a[mid] < a[l])r = mid;
        }
        return min(a[l],a[r]);
        
    }
};
```

# 矩阵中的路径

[矩阵中的路径](https://www.nowcoder.com/practice/c61c6999eecb4b8f88a98f66b273a3cc?tpId=13&tqId=11218&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    bool hasPath(char* matrix, int rows, int cols, char* str)
    {
        if(!matrix || rows < 1 || cols < 1 || !str)return false;
        bool *flag = new bool[rows*cols];
        memset(flag,false,sizeof(bool)*rows*cols);
 
        for(int i = 0;i<rows;i++){
            for(int j = 0;j<cols;j++){
                if(dfs(matrix,rows,cols,i,j,str,0,flag)) return true;
            }
        } 

        delete[] flag;
        return false;
    }
 
 
    bool dfs(char *matrix,int rows,int cols,int i,int j,char *str,int k,bool *flag){
        int index = i*cols+j;
        if(i<0 || i>=rows || j<0 || j >=cols ||
            matrix[index]!=str[k] || flag[index] == true)
            return false;
 
        if(str[k+1] == '\0')return true;
 
        flag[index] = true;
        if(dfs(matrix,rows,cols,i-1,j,str,k+1,flag)
            || dfs(matrix,rows,cols,i+1,j,str,k+1,flag)
            || dfs(matrix,rows,cols,i,j-1,str,k+1,flag)
            || dfs(matrix,rows,cols,i,j+1,str,k+1,flag))
            return true;
 
        flag[index] = false;
         return false;
 
    }
 
 
};

```

# 机器人的运动范围

[机器人的运动范围](https://www.nowcoder.com/practice/6e5207314b5241fb83f2329e89fdecc8?tpId=13&tqId=11219&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    int dx[4] = {1,-1,0,0};
    int dy[4] = {0,0,1,-1};
    int get(int x){
        int ans = 0;
        while(x)
        {
           ans += x%10;
            x/=10;
        }
        return ans;
    }
     
    int movingCount(int threshold, int rows, int cols)
    {
        bool *flag = new bool[rows*cols];
        memset(flag,false, rows*cols);
        return dfs(0,0,threshold,rows,cols,flag);
    }
     
    int dfs(int i,int j,int threshold,int rows,int cols,bool flag[]){
        if(i<0 || i>=rows || j<0 || j>=cols || get(i) + get(j) > threshold || flag[i*cols+j])
            return false;
        flag[i*cols+j] = true;
        int ans = 1;
        for(int ii = 0;ii < 4;ii++)
            {
                int iii = i+dx[ii],jjj = j+dy[ii];
                ans+=dfs(iii,jjj,threshold,rows,cols,flag);
            }
        //flag[i*cols+j] = false;
        return ans;
    }
};

```

#                                                              343. 整数拆分

[343. 整数拆分](https://leetcode-cn.com/problems/integer-break/)

```cpp
class Solution {
public:
    int integerBreak(int n) {
        int dp[1111];
        memset(dp,0,sizeof dp);
        
        dp[1] = 1;
        for(int len = 2;len<=n;len++)
            for(int i = 1;i<len;i++){
                dp[len] = max(dp[len],max(i*(len-i),dp[i]*(len-i)));
            }
        return dp[n];
        
    }
};

int integerBreak(int n) {
    if(n == 2)
        return 1;
    if(n == 3)
        return 2;
    int a = 1;
    while(n > 4){
        n = n - 3;
        a = a * 3;
    }
    return a * n;
}
```

# 二进制中1的个数

[二进制中1的个数](https://www.nowcoder.com/practice/8ee967e43c2c4ec193b040ea7fbb10b8?tpId=13&tqId=11164&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
     int  NumberOf1(int n) {
         int ans = 0;
         while(n){
             ans++;
             n = n&(n-1);
         }
         return ans;
     }
};
```

# 数值的整数次方

[数值的整数次方](https://www.nowcoder.com/practice/1a834e5e3e1a4b7ba251417554e07c00?tpId=13&tqId=11165&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    
    double myPow(double base,long long n){
        double c = 1;
        for(;n;n>>=1){
            if(n&1)c*=base;
            base = base*base;
        }
        return c;
    }
    
    double Power(double base, int exponent) {
        long long n = exponent;
        if(n == 0)return 1.0;
        if(n>0)return myPow(base,n);
        else return 1/myPow(base,-n);
        
    }
};
```
# 删除链表中重复的结点

[删除链表中重复的结点](https://www.nowcoder.com/practice/fc533c45b73a41b0b44ccba763f866ef?tpId=13&tqId=11209&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead)
    {
        auto head = new ListNode(-1);
        head->next = pHead;
        auto cur = pHead;
        auto last = head;
        
        
        while(cur && cur->next){
            if(cur->val == cur->next->val){
                int tmp = cur->val;
                cur = cur->next;
                while(cur && tmp == cur->val)cur = cur->next;
                last->next = cur;
            }else{
                last = cur;
                cur = cur->next;
            }
        }
        return head->next;
    }
};  
```
# 正则表达式匹配

[正则表达式匹配](https://www.nowcoder.com/practice/45327ae22b7b413ea21df13ee7d6429c?tpId=13&tqId=11205&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    bool match(char* str, char* pattern)
    {
        int m = strlen(str),n = strlen(pattern);
 
        bool dp[m+11][n+11];
        memset(dp,0,sizeof dp);
        dp[0][0] = true;
 
        for(int i = 0;i<=m;i++)
            for(int j = 1;j<=n;j++){
                if(pattern[j-1] == '*')
                    dp[i][j] = dp[i][j-2] || (i>0 && str[i-1] == pattern[j-2] || pattern[j-2] == '.') && dp[i-1][j];
                else dp[i][j] = i>0 && (str[i-1] == pattern[j-1] || pattern[j-1] == '.') && dp[i-1][j-1];
 
            }
 
        return dp[m][n];
    }
};
class Solution {
public:
    bool match(char* str, char* pattern)
    {
        if(*str == 0 && *pattern == 0)return true;
        if(*str != 0 && *pattern ==0)return false;//str还有，pattern 没了
         
        if(*(pattern+1) != '*'){
            if(*str == *pattern || *str != 0 && *pattern == '.'){//匹配
                return match(str+1,pattern+1);//向前走
            }
            return false;//不匹配，返回false
        }
         
        else{
            if(*str == *pattern || *str!=0 && *pattern == '.')//当前能匹配
                return match(str,pattern+2) || match(str+1,pattern);//直接跳过下个* 或者 看带着*，str继续走（可匹配多个）
            return match(str,pattern+2);
        }
         
    }
};
```
# 表示数值的字符串

[表示数值的字符串](https://www.nowcoder.com/practice/6f8c901d091949a5837e24bb82a731f2?tpId=13&tqId=11206&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:

	void check(char *&s,bool &flag){
		while(isdigit(*s)){
			++s;
			flag = true;
		}
	}
    bool isNumeric(char* s)
    {
        if(!s)return false;
        if(*s == '-' || *s == '+')++s;
        bool flag = false;
        check(s,flag);
        if(*s == '.' && *(s+1)!='e' && *(s+1)!='E')check(++s,flag);
         
        
        if(flag && (*s == 'e' || *s == 'E')){
        	++s;
        	flag = false;
        	if(*s == '-' || *s == '+')++s;
        	check(s,flag);
        }

        return flag && *s == 0;
    }

};


```
# 调整数组顺序使奇数位于偶数前面

[调整数组顺序使奇数位于偶数前面](https://www.nowcoder.com/practice/beb5aa231adc45b2a5dcc5b62c93f593?tpId=13&tqId=11166&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

冒泡排序!


```cpp
class Solution {
public:
    void reOrderArray(vector<int> &a) {//冒泡
        int n = a.size();
        for(int p = n-1;p>=0;p--){
        	int flag = 0;
        	for(int i = 0;i<p;i++){
        		if( (a[i+1]&1) && a[i]%2==0 ){
        			swap(a[i],a[i+1]),flag = 1;
        		}

        	}
        	if(flag == 0)break;
        }
    }
};

```
# 链表中倒数第k个结点

[链表中倒数第k个结点](https://www.nowcoder.com/practice/529d3ae5a407492994ad2a246518148a?tpId=13&tqId=11167&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        auto i = pListHead,ii = pListHead;
        long long x = k;
        while(ii && x--)ii=ii->next;//条件不要换
        if (x > 0)return nullptr;
        while(ii)ii = ii->next,i = i->next;
        return i;
    }
};
```
# 链表中环的入口结点

[链表中环的入口结点](https://www.nowcoder.com/practice/253d2c59ec3e4bc68da16833f79a38e4?tpId=13&tqId=11208&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* EntryNodeOfLoop(ListNode* pHead)
    {
        if(!pHead)return nullptr;
        if(!pHead->next)return nullptr;
        auto fast = pHead,slow = pHead;
        fast = fast->next->next;
        slow = slow->next;
        while(fast != slow){
            fast = fast->next->next;
            slow = slow->next;
        }
        
        fast = pHead;
        
        while(fast != slow){
            fast = fast->next;
            slow = slow->next;
        }
        return fast;
        
        
    }
};
```
# 反转链表

[反转链表](https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca?tpId=13&tqId=11168&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
//非递归,双指针
class Solution {
public: 
    ListNode* ReverseList(ListNode* head) {
        auto cur = head;
        ListNode* pre = nullptr;
        while(cur){
            auto nxt = cur->next;
            cur->next = pre;
            pre = cur;
            cur = nxt;
        }
        return pre;
    }
};

//递归
class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
         
        if(!pHead->next  || !pHead)return pHead;
        ListNode* node = ReverseList(pHead->next);
        //回溯过程进行翻转
        pHead->next->next = pHead;
        pHead->next = nullptr;
        return node;
    }
};
```
# 合并两个排序的链表

[合并两个排序的链表](https://www.nowcoder.com/practice/d8b6b4358f774294a89de2a6ac4d9337?tpId=13&tqId=11169&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* Merge(ListNode* l1, ListNode* l2)
    {
         ListNode *head = new ListNode(0);
        ListNode *cur = head;

        while(l1 && l2)
        {
        	if(l1->val < l2->val){
        		cur->next = l1;
        		cur = cur->next;
        		l1 = l1->next;
        	}
        	else {
                cur->next = l2;
                cur = cur->next;
                l2 = l2->next;
            }
        }
        cur->next = l1? l1:l2;
        return head->next;
    }
    
};
```
# 树的子结构(不是子树)

[树的子结构(不是子树)](https://www.nowcoder.com/practice/6e196c44c7004d15b1610b9afca8bd88?tpId=13&tqId=11170&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    
    bool isSub(TreeNode* r1,TreeNode* r2){
        if(r2==nullptr)return true;
        if(r1==nullptr)return false;
        if(r1->val != r2->val)return false;
        return isSub(r1->left,r2->left) && isSub(r1->right,r2->right);
    }
    bool HasSubtree(TreeNode* r1, TreeNode* r2)
    {
        if(!r1 || !r2)return false;
        return isSub(r1,r2) || HasSubtree(r1->left,r2) || HasSubtree(r1->right,r2);
    }
};
```
# 二叉树的镜像

[二叉树的镜像](https://www.nowcoder.com/practice/564f4c26aa584921bc75623e48ca3011?tpId=13&tqId=11171&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    void Mirror(TreeNode *pRoot) {
        
        if(pRoot){
            Mirror(pRoot->left);
            Mirror(pRoot->right);
            
            swap(pRoot->left,pRoot->right);
        }
    }
};
class Solution {
public:
    void Mirror(TreeNode *pRoot) {
        
        if(pRoot){
            Mirror(pRoot->left);
            Mirror(pRoot->right);
            
            swap(pRoot->left,pRoot->right);
        }
    }
};
```
# 对称的二叉树

[对称的二叉树](https://www.nowcoder.com/practice/ff05d44dfdb04e1d83bdbdab320efbcb?tpId=13&tqId=11211&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    bool is(TreeNode* left,TreeNode* right){
        if(!left && !right)return true;
        if(left == nullptr || right == nullptr)return false;
        
        if(left->val != right->val)return false;
        
        return is(left->left,right->right) && is(left->right,right->left);
    }
    bool isSymmetrical(TreeNode* pRoot)
    {
        if(!pRoot)return true;
        return is(pRoot->left,pRoot->right);
    }

};
```
# 顺时针打印矩阵

[顺时针打印矩阵](https://www.nowcoder.com/practice/9b4c81a02cd34f76be2659fa0d54342a?tpId=13&tqId=11172&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
    	int n = matrix.size();
    	int m = matrix[0].size();

    	int circle = (min(n,m)+1)/2;
    	std::vector<int> v;
    	for(int i = 0;i<circle;i++){
    		//左->右
    		for(int j = i;j<m-i;j++){
    			v.push_back(matrix[i][j]);
    		}
            //up->down
    		for(int j = i+1;j<n-i;j++)
    			v.push_back(matrix[j][m-i-1]);
            //right->left
    		for(int j = m-i-2; j>=i && n-i-1!=i;j--)
    			v.push_back(matrix[n-i-1][j]);
            //down->up
    		for(int j = n-i-2;j>i && m-i-1!=i;j--)
    			v.push_back(matrix[j][i]);
    	}

    	return v;
    }
};
```
# 包含min函数的栈

[包含min函数的栈](https://www.nowcoder.com/practice/4c776177d2c04c2494f2555c9fcc1e49?tpId=13&tqId=11173&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    stack<int>a,b;
    void push(int value) {
        a.push(value);
        if(!b.empty()&&value <= b.top())b.push(value);
        else if(!b.empty())b.push(b.top());
        else if(b.empty())b.push(value);
    }
    void pop() {
        if(!a.empty())  a.pop(),b.pop();
    }
    int top() {
         return a.top();
    }
    int min() {
         return b.top();
    }
};
```
# 栈的压入、弹出序列

[栈的压入、弹出序列](https://www.nowcoder.com/practice/d77d11405cc7470d82554cb392585106?tpId=13&tqId=11174&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        stack<int>s;
        int j  = 0;
        for(int i = 0;i<pushV.size();i++){
            s.push(pushV[i]);
            while(s.size() && s.top() == popV[j])s.pop(),j++;
        }
        return s.empty();
    }
};
```
# 从上往下打印二叉树

[从上往下打印二叉树](https://www.nowcoder.com/practice/7fe2212963db4790b57431d9ed259701?tpId=13&tqId=11175&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        queue<TreeNode*> q;
        q.push(root);
        vector<int>ans;
        if(!root)return ans;
        while(q.size()){
            auto x = q.front();q.pop();
            ans.push_back(x->val);
            if(x->left)q.push(x->left);
            if(x->right)q.push(x->right);
        }
        return ans;
    }
};
```



```cpp
class Solution {
public:
        vector<vector<int> > Print(TreeNode* root) {
            
            vector<vector<int> > ans;
            if(!root)return ans;
            
            queue<TreeNode*>q;
            q.push(root);
            
            while(!q.empty()){
                
                int n = q.size();
                vector<int>v;
                for(int i = 0;i<n;i++){
                    auto x = q.front();q.pop();
                    v.push_back(x->val);
                    if(x->left)q.push(x->left);
                    if(x->right)q.push(x->right);
                }
                ans.push_back(v);
                
                
            }
            
            return ans;
        }
    
};
```
# 按之字形顺序打印二叉树

[按之字形顺序打印二叉树](https://www.nowcoder.com/practice/91b69814117f4e8097390d107d2efbe0?tpId=13&tqId=11212&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    vector<vector<int> > Print(TreeNode* pRoot) {
        
    	bool flag = false;//奇数
    	queue<TreeNode*>q;
    	vector< vector<int> >ans;
    	if(!pRoot)return ans;
    	q.push(pRoot);
    	while(q.size()){
    		std::vector<int> v;
    		int n = q.size();	
    		for(int i = 0;i<n;i++){
    			auto x= q.front();q.pop();
    			v.push_back(x->val);
    			if(x->left)q.push(x->left);
    			if(x->right)q.push(x->right);
    		}
    		if(flag)reverse(v.begin(),v.end());
    		ans.push_back(v);
    		flag = !flag;
    	}
    	return ans;

    }
    
};
```
# 33. 二叉搜索树的后序遍历序列

[33. 二叉搜索树的后序遍历序列](https://www.nowcoder.com/practice/a861533d45854474ac791d90e447bafd?tpId=13&tqId=11176&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
 
    bool is(std::vector<int>::iterator begin, std::vector<int>::iterator end){
        int root = *end;
        
        if(begin >= end)return true;
        auto it = begin;
        for(it = begin;it<end;it++){
            if(*it > root)break;
        }
         
        for(auto i = it;i<end;i++){
            if(*i < root){return false;}
        }
 
        return is(begin,it-1) && is(it,end-1);
         
    }
 
    bool VerifySquenceOfBST(vector<int> sequence) {
        if(sequence.size() == 0)return false;
        return is(sequence.begin(),sequence.end()-1);
    }
};
```
# 二叉树中和为某一值的路径

[二叉树中和为某一值的路径](https://www.nowcoder.com/practice/b736e784e3e34731af99065031301bca?tpId=13&tqId=11177&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    
    vector< vector<int> >ans;
    vector<int> v;
    
    void solve(TreeNode* root,int x)
    {
        if(!root)return;
         v.push_back(root->val);
         if(!root->left && !root->right && x - root->val == 0)ans.push_back(v);
         else {
             if(root->left)solve(root->left,x - root->val);
             if(root->right)solve(root->right,x - root->val);
         }
         v.pop_back();
         
    }
    
    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        solve(root,expectNumber);
        return ans;
    }
};
```
# 复杂链表的复制

[复杂链表的复制](https://www.nowcoder.com/practice/f836b2c43afc4b35ad6adc41ec941dba?tpId=13&tqId=11178&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
/*
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
public:
    RandomListNode* Clone(RandomListNode* pHead)
    {
        if(!pHead)return nullptr;
        
        auto cur = pHead;
        
        while(cur){//先创建，连在一起
                RandomListNode *node = new RandomListNode(cur->label);
                node->next = cur->next;
                cur->next = node;
                cur = node->next;
        }
        
        cur = pHead;
        while(cur){
            if(cur->random){
                cur->next->random = cur->random->next;
            }
            cur=cur->next->next;
        }
        
        auto head = pHead->next;
        cur = head;
        auto pcur = pHead;
        while(pcur){
            pcur->next = pcur->next->next;
            
            if(cur->next)
                cur->next = cur->next->next;
            pcur = pcur->next;
            cur = cur->next;
        }
        return head;
    }
};
```
# 二叉搜索树与双向链表

[二叉搜索树与双向链表](https://www.nowcoder.com/practice/947f6eb80d944a84850b0538bf0ec3a5?tpId=13&tqId=11179&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct Trejavascript:void(0);eNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    TreeNode *head = nullptr,*cur = nullptr;
    
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        solve(pRootOfTree);
        return head;
    }
    
    void solve(TreeNode* root){
        if(!root)return;
        
        solve(root->left);
        
        if(cur == nullptr){
            cur = root;
            head = root;
        }
        else{
            cur->right = root;
            root->left = cur;
            cur = root;
        }
        
        solve(root->right);
            
    }
};
```
# 序列化二叉树

[序列化二叉树](https://www.nowcoder.com/practice/cf7e25aa97c04cc1a68c8f040e71fb84?tpId=13&tqId=11214&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    int num;
     
    char* Serialize(TreeNode *root) {   
        if(!root)return nullptr;
        string s;
 
        solve(root,s);
        char *ans = new char[s.size()+1];
        for(int i = 0;i<s.size();i++)ans[i] = s[i];
            ans[s.size()] = '\0';
        return ans;
    }
 
    void solve(TreeNode *root,string &s){
        if(!root){
            s+="#,";
            return;
        }
 
        s+=to_string(root->val);
        s+=",";
        solve(root->left,s);
        solve(root->right,s);
    }
 
 
 
 
 
 
    TreeNode* Deserialize(char *str) {
        if(!str || str[0] == '\0')return nullptr;
        int num = 0;
        return solve1(str,num);
    }
 
    TreeNode* solve1(char *str,int &num){
        int v = 0;
         
        if(str[num] == '#'){
            num+=2;
            return nullptr;
        }
        while(str[num]!=',' && str[num]!='\0'){
            v = v*10+str[num]-'0';
            num++;
        }
         
 
        TreeNode *node = new TreeNode(v);
        if(str[num] == '\0')return node;
        else num++;
        node->left = solve1(str,num);
        node->right = solve1(str,num);
        return node;
    }
};
```
# 38. 字符串的排列

[38. 字符串的排列](https://www.nowcoder.com/practice/fe6b651b66ae47d7acce78ffdd9a96c7?tpId=13&tqId=11180&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

next_permutation实现要烂熟于心。
```cpp
class Solution {
    std::vector<string> v;
public:
    vector<string> Permutation(string str) {
        if(str.size()==0)return vector<string>();
        sort(str.begin(),str.end());
        perm(str,0,str.size()-1);
        return v;
    }

    void perm(string str,int left,int right){
        if(left == right){
            v.push_back(str);
        }
        else{
            for(int i = left;i<=right;i++){
                if(i!=left && str[left] == str[i])continue;
                swap(str[left],str[i]);
                perm(str,left+1,right);
            }
        }
    }
};
```
# 数组中出现次数超过一半的数字

[数组中出现次数超过一半的数字](https://www.nowcoder.com/practice/e8a1b01a2df14cb2b228b30ee6a92163?tpId=13&tqId=11181&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        int n = numbers.size();
        int x=numbers[0],cnt = 0;
        for(int i = 0;i<n;i++){
            if(cnt == 0){cnt = 1;x = numbers[i];}
             else if(numbers[i] == x)++cnt;
            else --cnt;
        }
        cnt = 0;
        for(int i = 0;i<n;i++){
            if(x == numbers[i])cnt++;
            else cnt--;
        }
        if(cnt <= 0)return 0;
        return x;
    }
};
```
# 最小的K个数

[最小的K个数](https://www.nowcoder.com/practice/6a296eb82cf844ca8539b57c23e6e9bf?tpId=13&tqId=11182&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
      
    int patition(vector<int> &a,int first,int last){
        int pivot =  a[first];
        int p = first;
        while(1){//双向扫
            while(a[--last]>pivot);
            while(a[++first]<pivot);
            if(first >= last)break;
            swap(a[first],a[last]);
        }
        swap(a[last],a[p]);
        return last;
    }
     
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        int len = input.size();
        if(k == len)return input;
        if(k <=  0 || k>len)return vector<int>();
  
        int start  =  0,end = len;
        int index = patition(input,start,end);
  
        while(index != k-1){
            if(index > k-1){
               index = patition(input,start,index);
            }
            else{
                index = patition(input,index+1,end);
            }
        }
  
        std::vector<int> res(input.begin(),input.begin()+k);
        return res;
    }
};
class Solution {
public:
      
    int patition(vector<int> &a,int first,int last){
        int x = a[first];//第一个为主元
        int small = first;
        for(int i = small;i<=last;i++){//单向扫
            if(a[i] < x){
                ++small;
                swap(a[i],a[small]);
            }
        }
        swap(a[small],a[first]);
        return small;
}
     
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        int len = input.size();
        if(k == len)return input;
        if(k <=  0 || k>len)return vector<int>();
  
        int start  =  0,end = len-1;
        int index = patition(input,start,end);
  
        while(index != k-1){
            if(index > k-1){
               index = patition(input,start,index-1);
            }
            else{
                index = patition(input,index+1,end);
            }
        }
  
        std::vector<int> res(input.begin(),input.begin()+k);
        return res;
    }
};
class Solution {//最大堆
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        priority_queue<int> pq;
         
        int n = input.size();
        if(k<=0 || k > n)return vector<int>();
         
        for(int i = 0;i<k;i++)pq.push(input[i]);
        for(int i = k;i<n;i++)if(input[i] < pq.top())
            pq.pop(),pq.push(input[i]);
         
        vector<int>ans;
        for(int i = 0;i<k;i++)ans.push_back(pq.top()),pq.pop();
        return ans;
    }
};

```
# 数据流中的中位数

[数据流中的中位数](https://www.nowcoder.com/practice/9be0172896bd43948f8a32fb954e1be1?tpId=13&tqId=11216&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    priority_queue<int>pda;
    priority_queue<int,vector<int>,greater<int> >pxiao;
    
    void Insert(int num)
    {
        if(pda.empty() || num < pda.top())pda.push(num);
        else pxiao.push(num);
        
        if(pda.size() == pxiao.size() + 2){
           pxiao.push(pda.top());
            pda.pop();
           
        }
        if(pda.size() + 1  == pxiao.size()){
            pda.push(pxiao.top());
            pxiao.pop();
        }
    }

    double GetMedian()
    { 
        return pda.size() == pxiao.size()?(pda.top()+pxiao.top())/2.0:pda.top();
    }

};
```
# 41.2 字符流中第一个不重复的字符

[41.2 字符流中第一个不重复的字符](https://www.nowcoder.com/practice/00de97733b8e4f97a3fb5c680ee10720?tpId=13&tqId=11207&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp


class Solution
{
public:
	string s = "";
	char hs[256] ;
  //Insert one char from stringstream
    void Insert(char ch)
    {	
  		s+=ch;
  		++hs[ch];       
    }
  //return the first appearence once char in current stringstream
    char FirstAppearingOnce()
    {
    	for(int i = 0;i<s.size();i++){
    		if(hs[s[i]] == 1)return s[i];
    	}
    	return '#';
    }

};
```
# 整数中1出现的次数（从1到n整数中1出现的次数

[整数中1出现的次数（从1到n整数中1出现的次数](https://www.nowcoder.com/practice/bd7f978302044eee894445e244c7eee6?tpId=13&tqId=11184&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
//数学规律
class Solution {
public:
    int NumberOf1Between1AndN_Solution(int n)
    {
        if(n<1)return 0;
        long long ans = 0;
        long long round = n;
        long long base = 1;

        while(round){
        	int w = round % 10;
        	round /= 10;
        	ans += round * base;
        	if(w == 0)ans += 0;
        	else if(w == 1)ans += (n%base)+1;
        	else ans += base;
        	base *= 10;
        }
        return ans;
    }

};
//数位DP
class Solution {
public:

	int dp[20][20],num[20],cnt=0;


	int dfs(int pos,int flag,int sum){
		if(pos == -1)return sum;
		if(!flag && ~dp[pos][sum])return dp[pos][sum];
		int up = flag? num[pos]:9,ans = 0;
		for(int i = 0;i<=up;++i){
			ans+=dfs(pos-1,flag && (i == up),sum+(i==1));
		}
		if(!flag)dp[pos][sum] = ans;
		return ans;
	}

	int cal(int x){
		while(x){
			num[cnt++]=x%10;
			x/=10;
		}
		return dfs(cnt-1,1,0);
	}
    int countDigitOne(int n) {
        if(!n)return 0;
        memset(dp,-1,sizeof dp);
        return cal(n);
    }
};
public int NumberOf1Between1AndN_Solution(int n) {

    int cnt = 0;

    for (int m = 1; m <= n; m *= 10) {

        int a = n / m, b = n % m;

        cnt += (a + 8) / 10 * m + (a % 10 == 1 ? b + 1 : 0);

    }

    return cnt;

}

```


# 数字以 0123456789101112131415… 的格式序列化到一个字符串中，求这个字符串的第 index 位。
```cpp
//T44
class Solution {
public:
    int digitAtIndex(int n) {
        if(n<=9){
            return n;
        }
        int len = 1;
        long count = 9;
        int start = 1;
        while(n>len*count){
            n-=len*count;
            len+=1;
            count*=10;
            start *= 10;
        }
        start += (n-1)/len;
        string str = to_string(start);
        return str[(n-1)%len]-48;
    }
};

```
# 45. 把数组排成最小的数

[45. 把数组排成最小的数](https://www.nowcoder.com/practice/8fecd3f8ba334add803bf2a06af1b993?tpId=13&tqId=11185&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    
    static bool cmp(int x,int y)
    {
        string a = to_string(x);
        string b = to_string(y);    
        return a+b < b+a;
        
    }
    string PrintMinNumber(vector<int> numbers) {
        sort(numbers.begin(),numbers.end(),cmp);
        string ans;
        for(int i = 0;i<numbers.size();i++)
            ans += to_string(numbers[i]);
        return ans;
    }
};
```
#                                              46. 把数字翻译成字符串

[46. 把数字翻译成字符串](https://leetcode-cn.com/problems/decode-ways/)


```cpp
class Solution {
public:
    int numDecodings(string s) {
    //dp[n] = dp[n-1] + dp[n-2](n-1 n能组成)
        int n = s.size();
        int *dp = new int[n+2];
     
        dp[0] = 1;dp[1] = 1;
        if(s[0] == '0')dp[1] = 0;
        for(int i = 2;i <= n;i++)
        {
            dp[i] = s[i-1] == '0'? 0:dp[i-1];
            if(  (s[i-2] == '1') || (s[i-2] == '2' && s[i-1] <= '6'  )   )
                dp[i] += dp[i-2];
        }
        
        return dp[n];
    
    }
};
```
# 47. 礼物的最大价值

[47. 礼物的最大价值](https://www.nowcoder.com/questionTerminal/72a99e28381a407991f2c96d8cb238ab)


```cpp
class Bonus {
public:
    int getMost(vector<vector<int> > board) {
        // write code here
        //dp[i][j] = dp[i-1][j] dp[i][j-1]
        int n = board.size(),m = board[0].size();
        int *dp = new int[m+1];
        for(int i = 0;i<n;i++)
            for(int j = 0;j<m;++j){
                if(j>0)
                    dp[j] = max(dp[j],dp[j-1]) + board[i][j];
                else dp[j] = dp[j] + board[i][j];
            }
        return dp[m-1];
        
    }
};
```
#                       48. 最长不含重复字符的子字符串

[48. 最长不含重复字符的子字符串](https://leetcode-cn.com/submissions/detail/13690210/)


```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
  		
        std::vector<int> pre(256,-1);
  		int start = -1;	
  		int _max = 0;
  		for(int i = 0;i<s.size();i++){
  			if(start < pre[s[i]]){
  				start = pre[s[i]];
  			}
  			pre[s[i]] = i;
  			_max = max(_max,i-start);
  		}      
        return _max;
    }
};
```
# 49. 丑数

[49. 丑数](https://www.nowcoder.com/practice/6aa9e04fc3794f68acf8778237ba065b?tpId=13&tqId=11186&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    int GetUglyNumber_Solution(int index) {
        if(index <= 0)return 0;
        
        vector<int> v(index,0);
        v[0] = 1;
        int nxt = 1;
        int p = 0;
        int p2 = 0;
        int p3 = 0;
        int p5 = 0;
        
        while(nxt < index){
            int _min = min(v[p2]*2,v[p3]*3);
            _min = min(_min,v[p5]*5);
            v[nxt] = _min;
            
            while(v[p2]*2 <= v[nxt])++p2;
            while(v[p3]*3 <= v[nxt])++p3;
            while(v[p5]*5 <= v[nxt])++p5;
            ++nxt;
        }
        return v[index-1];
    }
};
```
# 第一个只出现一次的字符

[第一个只出现一次的字符](https://www.nowcoder.com/practice/1c82e8cf713b4bbeb2a5b31cf5b0417c?tpId=13&tqId=11187&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    int FirstNotRepeatingChar(string str) {
        int n = str.size();
        if(!n)return -1;
        int a[256];
        memset(a,-1,sizeof a);
        
        for(int i = 0;i<n;i++){
            if(a[str[i]] == -1)a[str[i]] = i;
            else if(a[str[i]] >= 0)a[str[i]] = -2;
        }
        int ans = 1111111;
        for(int i = 0;i<256;i++)if(a[i]>=0)ans = min(ans,a[i]);
        return ans == 1111111? -1:ans;
    }
};
```
# 数组中的逆序对

[数组中的逆序对](https://www.nowcoder.com/practice/96bd6684e04a44eb80e6a68efc0ec6c5?tpId=13&tqId=11188&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

排序总结有

```

```
# 两个链表的第一个公共结点

[两个链表的第一个公共结点](https://www.nowcoder.com/practice/6ab1d9a29e88450685099d45c9e31e46?tpId=13&tqId=11189&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        int n = 0, m = 0;
        auto p = pHead1;
        while(p){
            n++;
            p = p->next;
        }
        p = pHead2;
        while(p){
            m++,p = p->next;
        }
        auto p1 = pHead1,p2 = pHead2;
        while(n!=m){
            if(n>m){
                p1 = p1->next;n--;
            }
            else{
                p2 = p2->next;m--;
            }
            
            
        }
        for(int i = 0;i<n;i++){
            if(p1->val == p2->val)return p1;
            p1 = p1->next,p2 = p2->next;
        }
        
        return nullptr;
    }
};
```
# 数字在排序数组中出现的次数

[数字在排序数组中出现的次数](https://www.nowcoder.com/practice/70610bf967994b22bb1c26f9ae901fa2?tpId=13&tqId=11190&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    
    
    int GetNumberOfK(vector<int> data ,int k) {
        int n = data.size();
        if(!n)return 0;
        if(data[0] > k || data[n-1] < k)return 0;
        
        //lower_bonce
        
        int l = -1,r = n,mid;
        while(l+1<r){
            mid = (l+r)>>1;
            if(data[mid] >= k)r = mid;
            else l = mid;
        }
        int a = r;
        if(data[a] != k)return 0;
        l = -1,r = n;
        while(l+1<r){
            mid = (l+r)>>1;
            if(data[mid] <= k)l = mid;
            else r = mid;
        }
        int b = r;
        
        return b-a;
        
        
        
    }
};  
```
# 54. 二叉查找树的第 K 个结点

[54. 二叉查找树的第 K 个结点](https://www.nowcoder.com/practice/ef068f602dde4d28aab2b210e859150a?tpId=13&tqId=11215&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    int cnt = 0;
    
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
        
        if(pRoot){
        auto i =  KthNode(pRoot->left,k);
        if(i)return i;
        if(++cnt == k)return pRoot;    
        auto j = KthNode(pRoot->right,k);
        if(j) return j;
        }
        return nullptr;
       
    }
};
```
# 55.1 二叉树的深度

[55.1 二叉树的深度](https://www.nowcoder.com/practice/435fb86331474282a3499955f0a41e8b?tpId=13&tqId=11191&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    int TreeDepth(TreeNode* pRoot)
    {
        if(!pRoot)return 0;
        return 1 + max(TreeDepth(pRoot->left), TreeDepth(pRoot->right) );
    }
};
```
# 55.2 平衡二叉树

[55.2 平衡二叉树](https://www.nowcoder.com/practice/8b3b95850edb4115918ecebdf1b4d222?tpId=13&tqId=11192&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    bool IsBalanced_Solution(TreeNode* pRoot) {
        return solve(pRoot)!=-1;
    }
    
    int solve(TreeNode* root){
        if(!root)return 0;
        int left = solve(root->left);
        if(left == -1)return -1;
        int right = solve(root->right);
        if(right == -1)return -1;
        return abs(left-right) > 1?-1:1+max(left,right);
    }
    
};
```
# 56. 数组中只出现一次的数字

[56. 数组中只出现一次的数字](https://www.nowcoder.com/practice/e02fdb54d7524710a7d664d082bb7811?tpId=13&tqId=11193&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    void FindNumsAppearOnce(vector<int> a,int* num1,int *num2) {
        if(a.size() < 2 )return;
        int n = a.size();
        int mxor = 0;
        for(int i = 0;i<n;++i)mxor ^= a[i];
        int index = 1;
        while((mxor & index) == 0 )index <<= 1;
        
        *num1 = *num2 = 0;
        
        for(int i = 0;i<n;++i){
            if((a[i] & index) == 0)*num1 ^= a[i];
            else *num2 ^= a[i];
        }    
        return;
    }
};

     /**
     * 数组a中只有一个数出现一次，其他数字都出现了3次，找出这个数字
     * @param a
     * @return
     */
    public static int find1From3(int[] a){
        int[] bits = new int[32];
        int len = a.length;
        for(int i = 0; i < len; i++){
            for(int j = 0; j < 32; j++){
                bits[j] = bits[j] + ( (a[i]>>>j) & 1);
            }
        }
        int res = 0;
        for(int i = 0; i < 32; i++){
            if(bits[i] % 3 !=0){
                res = res | (1 << i);
            }
        }
        return res;
    }
```
# 57.1 和为 S 的两个数字

[57.1 和为 S 的两个数字](https://www.nowcoder.com/practice/390da4f7a00f44bea7c2f3d19491311b?tpId=13&tqId=11195&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)


```cpp
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        
        int left = 0,right = array.size()-1;
        vector<int> ans;
        while(left < right){
            if(array[left] +array[right] == sum){
                ans.push_back(array[left]),ans.push_back(array[right]);
                return ans;
            }    
            else if(array[left] + array[right] > sum){
                --right;
            }
            else ++left;
        }
        
        return ans;
    }
};
```

# 和为S的连续正数序列

[和为S的连续正数序列](https://www.nowcoder.com/practice/c451a3fd84b64cb19485dad758a55ebe?tpId=13&tqId=11194&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        
        vector<vector<int> > ans;
        int l = 1,r = 2;
        while(r>l){
            int cur = (l+r) * (r-l+1)/2;
            if(cur == sum){
                vector<int>res;
                for(int i = l;i<=r;i++)res.push_back(i);
                ans.push_back(res);
                ++r;
            }
            else if(cur < sum)++r;
            else ++l;
            
        }
        return ans;
    }
};
```

#  58.1 翻转单词顺序列

[ 58.1 翻转单词顺序列  ](https://www.nowcoder.com/practice/3194a4f4cf814f63919d0790578d51f3?tpId=13&tqId=11197&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    string ReverseSentence(string str) {
        
        int n = str.size();
        reverse(str.begin(),str.end());
        int l = 0,r = 0;
        for(int i = 0;i<=n;++i){
            if(str[i] == ' ' || str[i] == 0)
                reverse(str.begin()+l,str.begin()+i), l = i+1;
           
        }
        return str;
    }
};
```
# 左旋转字符串

[左旋转字符串](https://www.nowcoder.com/practice/12d959b108cb42b1ab72cef4d36af5ec?tpId=13&tqId=11196&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    string LeftRotateString(string str, int n) {
        int sz = str.size();
        if(!n || !sz)return str;
        string s = str + str;
        n%=sz;
        string ans = s.substr(n,sz);
        return ans;

    }
};

    class Solution {
public:
    string LeftRotateString(string str, int n) 
    {
      int len = str.size();
        if(len == 0) return str;
        n %= len;
        for(int i = 0, j = n - 1; i < j; ++i, --j) swap(str[i], str[j]);
        for(int i = n, j = len - 1; i < j; ++i, --j) swap(str[i], str[j]);
        for(int i = 0, j = len - 1; i < j; ++i, --j) swap(str[i], str[j]);
        return str;
    }
};
```
# 59. 滑动窗口的最大值

[59. 滑动窗口的最大值](https://www.nowcoder.com/practice/1624bc35a45c42c0bc17d17fa0cba788?tpId=13&tqId=11217&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    vector<int> maxInWindows(const vector<int>& num,  int size)
    {
       
         vector<int> ans;
         deque<int> dq;
         int n = num.size();
         if(!n || !size)return ans;    
         for( int i = 0;i<n;++i){
            while(dq.size() && num[dq.back()] <= num[i])dq.pop_back();//单调减
            dq.push_back(i);
            
            if(i - size + 1 >=0){
                ans.push_back(num[ dq.front() ]);
                if(dq.front() == i - size + 1)dq.pop_front();
            }
        }
        return ans;
        
        
    }
};
```
# 60. n 个骰子的点数

[60. n 个骰子的点数](https://www.lintcode.com/problem/dices-sum/description)

```cpp

//offer  :60. n 个骰子的点数
class Solution {
public:
    /**
     * @param n an integer
     * @return a list of pair<sum, probability>
     */
    vector<pair<int, double>> dicesSum(int n) {
        // Write your code here


        int sum = n*6;
        long long *dp = new long long[sum+2];
        for(int i = 1;i<=6;++i)dp[i] = 1;
        
        for(int i = 2;i<=n;++i)
		{
		    for(int j = 6*i;j>=i;--j)
    		{       
    		  dp[j] = 0;
    		  for(int k = 1;k<=6 && k<=j && j-k>=i-1;++k)
    		  dp[j] += dp[j-k];
    		}
    		//dp[i-1] = 0;//第三行能用到2，但是用不到1了。即：dp[3][3] += dp[2][2] + dp[2][1]（为0）
		}

    	double tot = pow(6,n);
    	std::vector<pair<int,double> > ans;
    	for(int i = n;i<=sum;++i){
    		ans.push_back(make_pair(i,dp[i]/tot));
    	}		
    	return ans;
    }
};


//记忆化搜索
class Solution {
public:
    /**
     * @param n an integer
     * @return a list of pair<sum, probability>
     */
	long long dp[111][111*6];
	long long solve(int n,int sum)
	{
		if(sum < n || n < 0 || sum >6*n)return 0;
		if(n==1 || sum == n)return 1;
		if(dp[n][sum])return dp[n][sum];
		return dp[n][sum] = solve(n-1,sum-1) + solve(n-1,sum-2) + solve(n-1,sum-3) + solve(n-1,sum-4) + solve(n-1,sum-5) + solve(n-1,sum-6);
	}
	
    vector<pair<int, double>> dicesSum(int n) {
        
        int sum = n*6;
		memset(dp,0,sizeof(dp));
    	double tot = pow(6,n);
    	std::vector<pair<int,double> > ans;
    	for(int i = n;i<=sum;++i){
    		ans.push_back(make_pair(i,solve(n,i)/tot));
    	}		
    	return ans;
    }
};
```
# 扑克牌顺子

[扑克牌顺子](https://www.nowcoder.com/practice/762836f4d43d43ca9deb273b3de8e1f4?tpId=13&tqId=11198&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    bool IsContinuous( vector<int> a ) {
         int n = a.size();
         if(n != 5)return false;
         int *cnt = new int[18];
        int _max = INT_MIN,_min = INT_MAX;
         for(int i = 0;i<n;++i){
             if(a[i] == 0)continue;
             cnt[a[i]]++;
             _max = max(_max,a[i]);
             _min = min(_min,a[i]);
         }
        for(int i = 1;i<=13;++i)if(cnt[i] > 1)return false;
        if(_max - _min >= 5)return false;
        return true;
    }
};
```
# 62. 圆圈中最后剩下的数：约瑟夫环

[62. 圆圈中最后剩下的数](https://www.nowcoder.com/practice/f78a359491e64a50bce2d89cff857eb6?tpId=13&tqId=11199&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    int LastRemaining_Solution(int n, int m)
    {if(!n)return -1;
     if(n==1)return 0;
        return (LastRemaining_Solution(n-1,m)+m)%n;
    }
};
```
# 65. 不用加减乘除做加法
[65. 不用加减乘除做加法](https://www.nowcoder.com/practice/59ac416b4b944300b617d4f7f111b215?tpId=13&tqId=11201&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

[leetcode29](https://leetcode-cn.com/problems/divide-two-integers/) 是做除法的，可以看下。

```cpp
class Solution {
public:
    int Add(int num1, int num2)
    {
        return num2 ? Add(num1^num2,(num1&num2)<<1):num1;//有无进位
    }
};
```

# 把字符串转换成整数

[把字符串转换成整数](https://www.nowcoder.com/practice/1277c681251b4372bdef344468e4f26e?tpId=13&tqId=11202&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```cpp
class Solution {
public:
    
    int StrToInt(string str) {
        
        int n = str.size();
        if(!n)return false;
        int flag = 1;
        bool valid = true;
        if(str[0] == '-')flag = -1;
        int ans = 0;
        int i;
        for(i = 0;i<str.size();i++){
            if(str[i] > '0' && str[i] < '9')ans = ans*10+str[i]-'0';
            else if(i == 0 && str[i] == '+' || str[i] == '-' );
            else {
                valid = false;
                break;
            }
        }
        
        if(!valid)return 0;
        return ans*flag;
        
    }
};
```