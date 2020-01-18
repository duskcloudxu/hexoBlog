---
title: leetcodeLeetDP
date: 2019-12-24 23:30:16
tags:
---

## List

| 题号 | 题目链接                                                     | 讲解链接                                 | 说明                                   |
| ---- | ------------------------------------------------------------ | ---------------------------------------- | -------------------------------------- |
| 一维 |                                                              |                                          |                                        |
| 70   | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/) | [视频讲解](https://cspiration.com/login) | Done                                   |
| 62   | [Unique Paths](https://leetcode.com/problems/unique-paths/description/) | [视频讲解](https://cspiration.com/login) | Done                                   |
| 63   | [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/description/) | [视频讲解](https://cspiration.com/login) | Done                                   |
| 120  | [Triangle](https://leetcode.com/problems/triangle/description/) | [视频讲解](https://cspiration.com/login) | 很少考                                 |
| 279  | [Perfect Squares](https://leetcode.com/problems/perfect-squares/description/) | [视频讲解](https://cspiration.com/login) | Done(Essentially an mathmatic problem) |
| 139  | [Word Break](https://leetcode.com/problems/word-break/)      | [视频讲解](https://cspiration.com/login) | Done                                   |
| 375  | [Guess Number Higher or Lower II](https://leetcode.com/problems/guess-number-higher-or-lower-ii/description/) | [视频讲解](https://cspiration.com/login) | hard to understand                     |
| 312  | [Burst Balloons](https://leetcode.com/problems/burst-balloons/description/) | [视频讲解](https://cspiration.com/login) |                                        |
| 322  | [Coin Change](https://leetcode.com/problems/coin-change/description/) | [视频讲解](https://cspiration.com/login) |                                        |
| 二维 |                                                              |                                          |                                        |
| 256  | [Paint House](https://leetcode.com/problems/paint-house/description/) | [视频讲解](https://cspiration.com/login) | Done                                   |
| 265  | [Paint House II](https://leetcode.com/problems/paint-house-ii/description/) | [视频讲解](https://cspiration.com/login) | Done(half)                             |
| 64   | [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/description/) | [视频讲解](https://cspiration.com/login) | Done                                   |
| 72   | [Edit Distance](https://leetcode.com/problems/edit-distance/description/) | [视频讲解](https://cspiration.com/login) |                                        |
| 97   | [Interleaving String](https://leetcode.com/problems/interleaving-string/description/) | [视频讲解](https://cspiration.com/login) |                                        |
| 174  | [Dungeon Game](https://leetcode.com/problems/dungeon-game/description/) | [视频讲解](https://cspiration.com/login) |                                        |
| 221  | [Maximal Square](https://leetcode.com/problems/maximal-square/description/) | [视频讲解](https://cspiration.com/login) |                                        |
| 85   | [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/description/) | [视频讲解](https://cspiration.com/login) |                                        |
| 363  | [Max Sum of Rectangle No Larger Than K](https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/description/) | [视频讲解](https://cspiration.com/login) | TreeSet                                |
| 化简 |                                                              |                                          |                                        |
| 198  | [House Robber](https://leetcode.com/problems/house-robber/)  | [视频讲解](https://cspiration.com/login) |                                        |
| 213  | [House Robber II](https://cspiration.com/leetcodeClassification) | [视频讲解](https://cspiration.com/login) |                                        |
| 276  | [Paint Fence](https://leetcode.com/problems/paint-fence/description/) | [视频讲解](https://cspiration.com/login) |                                        |
| 91   | [Decode Ways](https://leetcode.com/problems/decode-ways/description/) | [视频讲解](https://cspiration.com/login) |                                        |
| 10   | [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/description/) | [视频讲解](https://cspiration.com/login) |                                        |
| 44   | [Wildcard Matching](https://leetcode.com/problems/wildcard-matching/description/) | [视频讲解](https://cspiration.com/login) |                                        |

## 279

### Description

Given a positive integer *n*, find the least number of perfect square numbers (for example, `1, 4, 9, 16, ...`) which sum to *n*.

**Example 1:**

```
Input: n = 12
Output: 3 
Explanation: 12 = 4 + 4 + 4.
```

**Example 2:**

```
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

### explanation and my thought

Well, it is straightforward to come up with the dp formula:
$$
dp[i]=min_{all\space {n^2<i}}dp[i]-n^2
$$
In other words, we just define the same problem with an smaller input m as the subproblem,  and calculate the optimal solution by reuse the result of subproblem. And it would take $O(N^2)$ to terminate since in each subproblem we need to traverse $O(N)$ subproblem to get the answer and there is totally $N$ subproblem.



The fastest solution in the discussion board is basically a mathematic one. It is pretty but could not be called as very useful one when you are in an interview. Just think about how embarrassing it would be when you gave that genius solution and your interviewer ask you about some details of this algorithm. What you want to say? Show them the paper and proof? LOL.

### Code

``` c++
map<int,int>dp;
class Solution {
public:
    int numSquares(int n) {
        if(dp.find(n)!=dp.end())return dp[n];
        if(n==0)return 0;
        if(n==1)return 1;
        int rootFloor=int(sqrt(n));
        int res=INT_MAX;
        for(int i=rootFloor;i>=1;i--){
            res=min(res,numSquares(n-i*i)+1);
        }
        dp[n]=res;
        return res;
       
    }
};
```

## [139 Word Break](https://leetcode.com/problems/word-break/)

### Description

Given a **non-empty** string *s* and a dictionary *wordDict* containing a list of **non-empty** words, determine if *s* can be segmented into a space-separated sequence of one or more dictionary words.

**Note:**

- The same word in the dictionary may be reused multiple times in the segmentation.
- You may assume the dictionary does not contain duplicate words.

**Example 1:**

```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example 2:**

```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```

**Example 3:**

```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

 

### Explanation and my thought

First Let's talk about how could you find that it is a dynamic programming problem:

Firstly, greedy is not suitable there since there could be multiple choice to split the string and the result varies by each split:

in the example:

`s`=`leetcode`, `wordDict`=`["lee","leet",""code"]`, if you do things greedily, you might choose the "lee" as the first word to split the `s` then you get stuck there since there is no word like `tcode` in the `wordDict`.

So the dynamic programming method here, and firstly let's define the subproblem, there are two choices here, the first is to define the subproblem dp[i] so that the dp[i] is the answer for a substring start at index 0 and ends at index i, and the second solution is to define the subproblem as dp[i] [j] for the subproblem with substring start at i and ends at j.

If we observe the example, we could find that every valid string could be split like dp[j]+aWordInWordDict(j is some index >0 and <n). It could be proved using contradiction, so an one dimention array is enough to solve this problem, and the dp recurrence formula is like that:
$$
dp[i]=AND_{j<i}(dp[j]\&\&isSubString[j,i]InDictory)
$$

### Code

```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        if(s.size()==0)return false;
        map<string,bool>M;
        for(string word:wordDict){
            M[word]=true;
        }
        vector<int>positions;
        positions.push_back(0);
        vector<bool>dp(s.size(),false);
        for(int i=0;i<s.size();i++){
            for(int pre:positions){
                if(M.find(s.substr(pre,i-pre+1))!=M.end()){
                    dp[i]=true;
                }
            }
            if(dp[i])positions.push_back(i+1);
        }
        return dp[dp.size()-1];
    }
};
```

