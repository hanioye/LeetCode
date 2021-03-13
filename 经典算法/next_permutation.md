# 简述

> [next_permutation](http://www.cplusplus.com/reference/algorithm/next_permutation/)STL模板库函数是求给定一个自然数列, 求该数列的**下一个排列**. 是一种优秀的全排列生成算法. 
>
> 比如1,2,3的下一个排列是1,3,2, 再下一个是2,1,3 .....
>
> ```
> 1 2 3
> 1 3 2
> 2 1 3
> 2 3 1
> 3 1 2
> 3 2 1
> ```
>
> 

# 算法原理

这里只给出算法原理, 不提供严格的数学证明. (数学归纳法证明). 

先来观察下规律 : 

排列P0 : 1 2 3 4 5

排列P1 : 1 2 3 5 4

排列P2 : 1 2 4 3 5

排列P3 : 1 2 4 5 3

排列P4 : 1 2 5 3 4

排列P5 : 1 2 5 4 3

排列P6 : 1 3 2 4 5

...

1. **P0->P1很显然.**

2. **P5->P6. 排列 P5 : (1 2 5 4 3) 的子序列 (5 4 3 )是严格递减的, 此时, 我们将元素2和(5 4 3)中的任意一个调换, 必能能得到比P5大的排列, 但不是下一个排列, 要保证下一个排列, 必须保证元素2前面的元素不动, 并将(5 4 3)元素倒序第一个比2大的元素(也就是3)和2调换,得到排列 : 1 3 5 4 2,  后面三个元素依旧是降序的, 对其reverse便可得到排列P5的下一个排列P6 : 1 3 2 4 5.**



复杂度分析:

​		最好的情况为排列的最右边的2个元素构成一个最小的增序子集, 交换次数为1, 复杂度为O(1), 最差的情况为1个元素最小，而右面的所有元素构成减序子集, 这样需要先将第1个元素换到最右, 然后反转右面的所有元素.  交换次数为1+(n-1)/2, 复杂度为O(n). 因为各种排列等可能出现，所以平均复杂度即为O(n).



# 算法实现

给出next_permutation算法实现. PS : pre_permutation怎么写?

```cpp
  template < typename T>
  bool Next_permutation(T first,T last){
    if(first == last)return false;//空区间
    T i = first;
    i++;
    if(i == last)return false;//1个元素
    i = last;
    i--;

    while(1){
      T ii = i;
      i--;//逆序找出第一个 *i<*ii;

      if(*i < *ii){
        T j = last;
        while(!(*i < *--j));//倒序找到第一个比*i大的元素
        iter_swap(i,j);//交换*i，*j
        reverse(ii,last);//ii之后的全部逆序
        return true;
      }
      if(i == first){//进行到最前面了，全部逆序返回false
        reverse(first,last);
        return false;
      }
    }
  }
```







# 应用

[46. 全排列](https://leetcode-cn.com/problems/permutations/) 

本题当然可以DFS搜索去做, 但是时间复杂度显然没有next_permutatio来的好.

同类型的题: [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/), [31. 下一个排列](https://leetcode-cn.com/problems/next-permutation/), [60. 排列序列](https://leetcode-cn.com/problems/permutation-sequence/).

```cpp
//[46. 全排列](https://leetcode-cn.com/problems/permutations/) 
class Solution {
public:

    template < typename T>
    bool Next_permutation(T first,T last){
        if(first == last)return false;//空区间
        T i = first;
        i++;
        if(i == last)return false;//1个元素
        i = last;
        i--;
    
        while(1){
            T ii = i;
            i--;//逆序找出第一个 *i<*ii;
    
            if(*i < *ii){
                T j = last;
                while(!(*i < *--j));//倒序找到第一个比*i大的元素
                iter_swap(i,j);//交换*i，*j
                reverse(ii,last);//ii之后的全部逆序
                return true;
            }
            if(i == first){//进行到最前面了，全部逆序返回false
                reverse(first,last);
                return false;
            }
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        std::vector<std::vector<int> > ans;
        do{
        	ans.push_back(nums);
        }while(Next_permutation(nums.begin(),nums.end()));
        return ans;
    }
    
};
```









