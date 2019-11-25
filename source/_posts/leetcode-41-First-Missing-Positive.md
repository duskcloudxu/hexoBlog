---
title: leetcode 41 First Missing Positive
date: 2019-11-24 22:07:31
tags:
- Array
- Mapping
- Mathematic
---

## Description

Given an unsorted integer array, find the smallest missing positive integer.

**Example 1:**

```
Input: [1,2,0]
Output: 3
```

**Example 2:**

```
Input: [3,4,-1,1]
Output: 2
```

**Example 3:**

```
Input: [7,8,9,11,12]
Output: 1
```

**Note:**

Your algorithm should run in *O*(*n*) time and uses constant extra space.

## Clues

At the first time, I ran into an opposite direction: we should search the all the numbers between the interval `min(nums),max(nums)` by some special tactics, and, unsurprisingly,  failed in coming up with one.

After took a second look and tried serval examples, I found that maybe all the result number would be small no matter how big numbers in the unordered array is. It reminds my that, maybe total number of all the possible results is quite small or just of O(n). It would not take long to find the trick for this problem after you noticed that: for a given array $A$ of size $n$, in the worst case, where all elements are $1,2,3,4,..,n$, the smallest missing positive number is n+1. if any element of $A$ is bigger than $n$, then there must be an "empty slot" in the interval $[1,n]$, therefore the missing positive should be that number. if there are multiple numbers bigger than n, then the answer should be the smallest number among all the "empty slots".

The tricky part of this problem is that, you need to find the property of this number we are finding, instead of diving into thinking about different optimization tactics in the very beginning. Do not be like me. LOL.

## CODE

```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        //init all the possible result
        vector<int>possibleNums(nums.size()+1,0);
        for(auto num:nums){
            if(num>=1&&num<=nums.size()){
                possibleNums[num]++;
            }
        }
        //find the first missing possible number, if res have not been updated,
        //then this array must be like [1,2,3,...,n](or in any order), so return
        //n+1.
        int res=nums.size()+1;
        for(int i=1;i<possibleNums.size();i++){
            if(possibleNums[i]==0)return i;
        }
        return res;
        
    }
};
```

