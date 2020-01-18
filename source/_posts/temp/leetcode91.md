# Leetcode 91

## 题面

91. Decode Ways

Medium

A message containing letters from `A-Z` is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given a **non-empty** string containing only digits, determine the total number of ways to decode it.

**Example 1:**

```
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```

**Example 2:**

```
Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

## 思路

这个题面比较直白，一看就知道用DP来做了（即分解成子问题），很明显是

- 读末尾的1位+剩下所有可能的组合
- 读末尾的2位+剩下所有可能的组合

这样。

然后起枪，被秒了，有什么好说的。

为什么呢？因为字符串里面并不guarantee没有0，而0是不可解的，这算是一个小坑，而我们应该做的呢，是把DP的过程想象成一个二叉树，每一个节点的值等于它所有后代的值的和，如果一个后代遇到当前不可解的情况，就直接返回0。

## code

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var numDecodings = function(s) {
    return decode(s,s.length)
};
var decode=function(str,n){
    if(n===1&&str[n-1]==='0'||n===0&&str[0]==='0')return 0
    if(n===1||n===0)return 1
    let lastTwo=(str[n-2]-'0')*10+(str[n-1]-'0')
    if(str[n-2]!=='0'&&str[n-1]!=='0'){
        if(lastTwo<27&&lastTwo>0)return decode(str,n-1)+decode(str,n-2)
        else return decode(str,n-1)
    }
    else{
        if(str[n-2]==='0'&&str[n-1]==='0')return 0
        else if(str[n-2]==='0')return decode(str,n-1)
        else if(lastTwo<27)return decode(str,n-2)
        else return 0
    }
}
```



