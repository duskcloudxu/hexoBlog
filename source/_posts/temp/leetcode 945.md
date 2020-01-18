# leetcode 945

## 题面

945.Minimum Increment to Make Array Unique

Medium

Given an array of integers A, a *move* consists of choosing any `A[i]`, and incrementing it by `1`.

Return the least number of moves to make every value in `A`unique.

 

**Example 1:**

```
Input: [1,2,2]
Output: 1
Explanation:  After 1 move, the array could be [1, 2, 3].
```

**Example 2:**

```
Input: [3,2,1,2,1,7]
Output: 6
Explanation:  After 6 moves, the array could be [3, 4, 1, 2, 5, 7].
It can be shown with 5 or less moves that it is impossible for the array to have all unique values.
```

 

**Note:**

1. `0 <= A.length <= 40000`
2. `0 <= A[i] < 40000`

## 思路

初看很简单，但是还是有一定难度的题目（你菜

首先我初见是做出来了，但是不是最优解。具体方法是将数组进行一个排列，然后统计每个数字有多少个重复项，再依次进行处理。

处理的方法也很简单，首先先找到当前这个数组的一个空着的最小数字min

> 例如对于`[1,1,3]`来说，最小数字min是2。

对于任意一个数字来说，若其有重复项，那么将重复项和当前的最小数字进行比较，如果当前的最小数字小于这个重复项，则进行增加，直到找到比重复项大的，不在原数组中的一个最小的数字为止。然后将这个数字记录下来，并增加sum。

然后我在一次VC中再次遇到了这个题目，当时脑子不大清楚，所以直接暴力过了，就是从头到尾扫一遍，之前没见过的加到map里，发现这个东西是map里已经有了的那么不断自增，直到自增出来的数字是map里没有的，再放入map并增加moves。

最后结合别人的题解，想出了这道题的一个类dp解法。即，我们将一个已经排序好的，没有重复项的序列S称为一个这样的问题A的解，那假设这个问题A是问题B的子问题，且A的原数组和B的原数组只差一个元素E，那么我们就可以从把E插入序列S中，并得出B的解。

具体判断方法是：

1. 若E大于S的最后一个元素$S_{end}$，那么S+E就是问题B的解，其moves等于A的moves
2. 若E小于S的最后一个元素$S_{end}$，那么E要变为$S_{end}+1$，其moves等于A的moves加$S_{end}+1-E$

## 代码

```javascript
/**
 * @param {number[]} A
 * @return {number}
 */
var minIncrementForUnique = function(A) {
    let sum=0;
    A=A.sort((a,b)=>a-b);
    for(let i=1;i<A.length;i++){
        let cur=A[i];
        let last=A[i-1]
        if(cur>last)continue;
        else{
            A[i]=last+1;
            sum+=A[i]-cur;
        }
    }
    return sum
};
```



