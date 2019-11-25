---
title: leetcode 33引起的二分搜索思考
date: 2019-08-02 18:40:13
tags:
- leetcode
- 算法
- 二分
---
# leetcode 33引起的二分搜索思考
> 由二分引发的血案。

前段时间做过一道二分的变体，[leetcode 33](<https://leetcode.com/problems/search-in-rotated-sorted-array/>), 对里面一个用二分搜索来搜shifted的位置的方法印象很深。

顺便提一下，其实我觉得大部分人对于二分的理解应该是不够的，结果就是手写的时候被各种edge case卡到（包括我），一个比较好的理解二分搜索的文章在[这里](<https://www.zhihu.com/question/36132386>)

对于一个顺序被shift过的排序`arr`，其样子应该是以下几种情况之一：

- 1234567
- 2345671
- 3456712
- 4567123
- 5671234
- 6712345
- 7123456

观察上述数列，我们可以认识到一件事情**当最左边的数字小于最右边的时候，中间的数列必然是连续上升的**

这个性质是因为原数组的上升特性导致的，如图：

```python
a=[1, 2, 3, 4, 5, 6, 7, 8]
```



![1564735981253](1564735981253.png)

```python
def shift(arr,n):
    return arr[n:]+arr[:n]
shift(a,4)
```



![1564736216761](1564736216761.png)

以上是shift()过后的图形。

通过脑补我们可以认识到，任何的平移都不可能改变这个数组的上述性质。然后用这个特质，我们就可以对shift过的数组进行二分搜索，找出原来数组的开头在哪里。

那么如果我们把`left`设为$0$, `right`设为$arr.length-1$，`mid`为`left+(right-left)//2`, 那么就有以下情况：

- `arr[left]`<`arr[mid]`<`arr[right]`
- `arr[right]`<`arr[left]`<`arr[mid]`
- `arr[mid]`<`arr[right]`<`arr[left]`

> 可以思考一下为什么没有其他情况

对上述情况进行讨论，如果说一开始是第一种情况，那我们很明显就能发下这个数组的shift value是0。

对于第二种情况，也就是中间值大于左边值，那么我们找到的这个`mid`肯定在一开始的头前面，差不多是如下的位置：

![1564736964648](1564736964648.png)

那在这种情况下，我们只需要对$[mid,R_0]$再次搜索就好，如图：

![1564737269936](1564737269936.png)

那么，如果是第三种情况，即：`arr[mid]`<`arr[right]`<`arr[left]`呢？

类似的，我们可以知道我们的搜索范围是这种情况：

![1564741720375](1564741720375.png)

那我们下一次搜索的范围就是：$[L_0,mid ]$

这种搜索最后将会收敛于一种情况, 即$[left,left+1]$(或者$[right-1,right]$),在这个时候做一个特殊判断就行了。

![1564741625079](1564741625079.png)

### 总结

我们将一个shift过的数组分为两个区间

- $[L_0,left]$, 这个区间中所有的数字大于$arr[right]$
- $[right,R_0]$, 这个区间中的所有数字大于等于$arr[right]$,小于等于$arr[R_0]$
- 那么显然，当left+1=right的时候整个数组被分配完毕，我们要找的就是这个时候的$right$了

演示代码：

```python
def shift(arr,n):
    return arr[n:]+arr[:n]
def findOriStart(arr,left,right):
    if(left==right-1):return right
    if(arr[left]<arr[right]):return left
    mid=left+(right-left)//2
    if(arr[left]<arr[mid]):return findOriStart(arr,mid,right)
    else: return findOriStart(arr,left,mid)
    
    
    
    
a = [1, 2, 3, 4, 5, 6, 7, 8]
for i in range(len(a)):
    a=shift(a,1);
    print('for ',a, ' the start is %d'%findOriStart(a,0,len(a)-1))
```

然后写完这篇以后去用py重新试了leetcode 33（刚学，写得丑轻拍）, 被edge case卡成傻逼……最后就small case暴力解了(你list里找一个东西竟然可以报exception我是真没脾气（逃）)

```python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        #small case讨论
        if(len(nums)<3):
            res=-1;
            try: return nums.index(target)
            except:return -1;
        startInd=(findOriStart(nums,0,len(nums)-1));
        return binarySearch(nums,target,startInd)
        
def binarySearch(arr,target,SI):
    left=0
    right=len(arr)
    if(arr[trans(left,SI,len(arr))]>target):return -1
    while(left<right):
        mid=left+(right-left)//2
        realMid=trans(mid,SI,len(arr));
        if(arr[realMid]<target):left=mid+1;
        else: right=mid
    if(right==len(arr) or arr[trans(right,SI,len(arr))]!=target):return -1
    return trans(left,SI,len(arr))
        
def trans(cur,startInd,length):
    return (cur+startInd)%length
        
def findOriStart(arr,left,right):
    if(left==right):return 0;
    if(left==right-1):return right
    if(arr[left]<arr[right]):return left
    mid=left+(right-left)//2
    if(arr[left]<arr[mid]):return findOriStart(arr,mid,right)
    else: return findOriStart(arr,left,mid)

```

