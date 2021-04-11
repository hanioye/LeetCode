# 线形DP

### 背包问题

背包问题是线形DP中非常重要且典型的问题。



#### 0/1背包

> **0/1背包模型** : 给定N个物品，其中第i个物品的体积为V~i~，价位为W~i~。有一容积为M的背包，要求选择一些物品放入背包，使得物品总体积不超过M的前提下，物品的价值和达到最大。

​	

 dp[ i, j ] 代表从前i个物品中选出总重量不超过j的物品时总价值的最大值。

 dp[ i, j ] = max{ dp[ i-1, j], dp [ i-1 , j - v~i~ ] }    **(j>=v~i~)**.

 dp[ i, j ] = dp[ i-1, j ]                                        **(j<v~i~)**.



**0/1背包代码实现:**

```c++
vector< vector<int> > dp(N+1,vector<int >(M+1,0));
for(int i=1;i<=N;i++)
        for (int j = 0; j <= M; j++)
        {
            if (j < v[i])dp[i][j] = dp[i - 1][j];
            else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - v[i]] + w[i]);
        }
```



**代码实现优化:** 

**滚动数组** : 我们的dp数组是用了一个N*M空间大小的二维数组，实际上迭代的过程中，我们在计算dp[i]时只用到了dp[i]和dp[i-1], 所以只用两个二维数组就可表示状态转移:

```c++
vector< vector<int> > dp(2,vector<int >(M+1,0));
for(int i=1;i<=N;i++)
        for (int j = 0; j <= M; j++)
        {
            if (j < v[i])dp[i % 2][j] = dp[(i - 1) % 2][j];
            else dp[i % 2][j] = max(dp[(i - 1) % 2][j], dp[(i - 1) % 2][j - v[i]] + w[i]);
        }
```



**重复一维数组**: 再来观察下二维dp数组，如果将二维数组打印出来的话，会发现:**当前点(i,j)依赖左上的点(i-1,j-v[i])和上面的点(i-1,j),**我习惯称为:左上依赖。这种依赖关系是可以将二维数组优化为倒叙一维数组去解决的:

```c++
vector<int> dp(M+1,0);//一维数组
for(int i=1;i<=N;i++)
        for (int j = M; j >= v[i]; j--)//倒叙
        {
        		dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
        }
```

**注意:重复利用一维数组可以节约内存空间，但使用不好也有可能留下bug，所以要格外小心。新手比较好的建议是先按照题意去写二维的，然后可以优化的话，在优化成一维的(滚动数组、重复一维数组)。**





##### 0/1背包练习题:

[POJ-3624 0/1背包模板题](https://vjudge.net/problem/POJ-3624)

```c++
#include<iostream>
#include<algorithm>
using namespace std;
int dp[13000];//0
int w[3500], v[3500];
int main()
{
    int N, M; cin >> N >> M;
    for (int i = 1; i <= N; i++)
        cin >> w[i] >> v[i];
       for(int i=1;i<=N;i++)
        for (int j = M; j >= w[i]; j--)   
        {
                dp[j] = max(dp[j], dp[j - w[i]] + v[i]);
         }
    cout << dp[M];
}
```



[474. 一和零](https://leetcode-cn.com/problems/ones-and-zeroes/)

```c++
class Solution {
public:
    int dp[1111][1111];
    int findMaxForm(vector<string>& strs, int m, int n) {
        
        int size_str = strs.size();
        memset(dp,0,sizeof(dp));


        for(int i = 0;i<size_str;i++)
        {
            int v_0=0,v_1=0;
            for(auto x:strs[i])x=='0'?v_0++:v_1++;
            for(int j = n;j>=v_1;j--)
                for(int k = m;k>=v_0;k--)
                dp[j][k] = max(dp[j][k],dp[j-v_1][k-v_0]+1);
        }        
        return dp[n][m];
    }
};
```



[416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

```c++
class Solution {
public:
    bool canPartition(vector<int>& nums) {

        int sum = accumulate(nums.begin(),nums.end(),0);
        if(sum&1) return false;
        sum = (sum>>1);
        vector<bool>dp(sum+11,false);
        dp[0] = 1;
        int n = nums.size();
        for(int i = 0;i<n;++i)
            for(int j = sum;j>=nums[i];--j)
                dp[j] = dp[j] || dp[ j - nums[i] ];

        return dp[sum];

    }
};
```





## TODO:

## 区间DP

石子合并问题等





## 状态压缩DP

## 数位DP

相对比较难理解，面试比较少见



