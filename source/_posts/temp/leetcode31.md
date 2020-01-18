# Leetcode 31

## 题面

> 31. Next Permutation
>
> Medium
>
> Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.
>
> If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).
>
> The replacement must be **in-place** and use only constant extra memory.
>
> Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
>
> ```
> 1,2,3` → `1,3,2`
> `3,2,1` → `1,2,3`
> `1,1,5` → `1,5,1
> ```

## 思路

首先要确定这个lexicographically next greater是个什么定义。

（也是这道题的坑点

我一开始的思路是：

- 将其作为一个数字来处理，123=$1\times10^2+2\times10^1++3\times10^0$
- 所谓lexicographically next greater就是交换一个逆序对的数字位置，且这个逆序对交换以后对这个数字的大小变化是最小的。

然后实现以后就被样例摁在地上摩擦（躺

回到正确的思路上来：

什么是lexicographically next greater?

那么我们得看字符串比大小是一个怎么样的机制：

> 两个字符串$A$,$B$从左到右进行比较，在第一个两个字符串不同的字符位置$i$，若$A_i>B_i$，则$A>B$,反之亦然。

那一个刚好在字典顺序上只比输入字符串$A$大一位的，且包含的元素的集合相同的字符串$B$是怎么样的？

- 它在第一位不一样的字符位置$i$上存在$B_i>A_i$
- 不存在一个字符串$C$使$A<C<B$

那如果说存在$C$使得$A<C<B$，那$C$该满足什么条件？

前提是字符位置$i$上存在$B_i>A_i$，那么，$A_i<=C_i<=B_i$

把这个条件拆分开来看：

1. $A_i=C_i<B_i$  
   -  在这种情况下，存在后续位置$j$使得$A_j<C_j$。

2. $A_i<C_i=B_i$
   - 在这种情况下，存在后续位置$j$使得$C_j<B_j$。

3. $A_i<C_i<B_i$



再次明确问题，我们现在已知$A$，求$B$,且要求不存在满足$A<C<B$的$C$

如何消灭条件1？

设定$S(A,a,b)$为$A$从位置$a$到位置$b$的子串，n为A串长度。

- 寻找到一个$i$,满足子序列$S(A,i+1,n)$是一个字典逆序的字符串，这样就不存在**由相同字符集合组成的**$C$满足条件1

  > 如[2,3,1],那么$i=0$,因为S(A,1,2)=[3,1],很明显再怎么排也不会有C能够满足条件一

如何消灭条件3？

- 将$i$与$S(A,i+1,n)$中最小的一个大于$A_i$的字符$A_{minMax}$对换，组成$S(B,0,i)$的部分，则不存在**由相同字符集合组成的**$C$满足条件3

  > 如[2,3,1],$i=0$,$A_{minMax}=3$那么S(B,0,0)=3

如何消灭条件2？

- 在上步交换做完以后，将交换完毕的数组称为$T$,$S(T,i+1,n)$中剩下元素排列成字典顺序，组成$S(B,i+1,n)$,则不存在**由相同字符集合组成的**$C$满足条件2

  > 如上一步我们交换了$A_i$和$A_{minMax}$,那么我们就把变换后的数组称为$T$吧，$T=[3,2,1]$,然后将$S(T,i+1,n)$变为字典顺序：$[1,2]$，将其作为$S(B,i+1,n)$,即得$B=[3,1,2]$ 

总结一下，这道题的~~马后炮~~解题思路是，将题目所说的lexicographically next great的定义给定义清晰，然后把问题明确到next great就是中间不存在$A<C<B$的意思，将$C$存在的可能性给分开讨论，然后找出一种直接由$A$衍生出符合不存在$A<C<B$的可能性的$B$的方法。



直接的来说方法，找到最后一段 不能再变大的子序列（即降序子序列），那么这个子序列肯定是需要重置的，然后在子序列中找到子序列前一个数字的上界数字`α`，因为子序列是降序的所以从右往左找第一个第一个大于`α`就行（你想要二分我也不拦着……），然后交换它们，并把交换过后的子序列给按升序排列。





## 代码（javascript,JS)

> Runtime: 64 ms, faster than 87.57% of JavaScript online submissions forNext Permutation.
>
> Memory Usage: 34.9 MB, less than 61.40% of JavaScript online submissions for Next Permutation.

```javascript
	/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var nextPermutation = function(nums) {
    let target=nums.length-2;
    while(target!==-1){
        if(nums[target]<nums[target+1])break;
        target--;
    }
    if(target===-1){
        nums=nums.sort((a,b)=>a-b);
        return;
    }
    else{
        let lb=findMinMax(target,nums);
        let temp=nums[lb];
        nums[lb]=nums[target];
        nums[target]=temp;
        let res=nums.splice(target+1,nums.length-target-1);
        nums.splice(target+1,0,...res.sort((a,b)=>a-b));
    }
};
function findMinMax(target,arr){
    let tar=arr[target];
    let minMax=tar;
    let minMaxIndex;
    for(let i=target+1;i<arr.length;i++){
        if(arr[i]>tar){
            minMax=Math.min(minMax,arr[i]);
            minMaxIndex=i;
        }
    }
    return minMaxIndex
}
```

