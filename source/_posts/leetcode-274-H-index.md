---
title: leetcode 274 H-index
date: 2019-12-15 00:43:11
tags:
- leetcode
---

## 274 ,275 H-Index

### Description

274. H-Index

Medium

484810FavoriteShare

Given an array of citations (each citation is a non-negative integer) of a researcher, write a function to compute the researcher's h-index.

According to the [definition of h-index on Wikipedia](https://en.wikipedia.org/wiki/H-index): "A scientist has index *h* if *h* of his/her *N* papers have **at least** *h* citations each, and the other *N − h* papers have **no more than** *h* citations each."

**Example:**

```
Input: citations = [3,0,6,1,5]
Output: 3 
Explanation: [3,0,6,1,5] means the researcher has 5 papers in total and each of them had 
             received 3, 0, 6, 1, 5 citations respectively. 
             Since the researcher has 3 papers with at least 3 citations each and the remaining 
             two with no more than 3 citations each, her h-index is 3.
```

**Note:** If there are several possible values for *h*, the maximum one is taken as the h-index.

### Explanations

That is an interesting question, and the main challenge is to understand the new conception brought in description: `The H-index`.

It says that, Given an array of citation numbers `citi`,  an h-index is an number `h` that within the interval `[0,citi.length]` so that there are `h` paper has citations `c` >= `h`, and remaining papers has `c`<=`h`.  

#### Analysis

First of all, this definition is way too complicated, let's find a way to simplify it. 

The beginning of problem analysis is always find a certifier. Given an number h,  We examine it by iterates the whole array, and count the number of paper that has citations less or equal to `h`, namely $Num_{under}$, and number$Num_{upper}$ for papers that has citations above or equal to `h`. if $Num_{upper}$==`h`, then it is an valid `H-index`.

Emmm, I see, it is an NP problem! (LOL)

Okay, to be serious, now we knows how to examine the answer, and let's talk about more different situations. For example, we could easily know that if $Num_{upper}$ is smaller then `h`, the `h` must be bigger than any `H-index`. But what if we find $Num_{upper}$ is bigger then h? Is there any way to find the right `H-index`?



The most naïve method is to increase `h` by one, and iterate the whole array to check if $Number_{upper}$ decreased. If it really decreased, and currently get `h`=$Num_{upper}$ , okay we get the right `H-index`. if $Num_{upper}$ is still the same or it decreased but still bigger than `h` , we continuously decrease `h`, since `h` increases and $Num_{upper}$ decreases, they would become equal eventually. 



Do we really need to iterate the whole array in each increase? meh...let's reconsider our model: Set $S_{upper}$ for papers with citations >= `h`, and set $S_{under}$for papers with citations <=`h`, initial h=0, so $|S_{upper}|=|citi|$ , now we have a set $Gree$. As the name, we put the paper with maximum citation among the remaining papers with citations >=`h` into $Gree$ each time `h` increases, and when there is no such paper, we say the `h` is the max `h-index`. Since each time we select the paper with max citation, we just need to sort the array in descending order and iterator over it. 

#### Follow Up

275 says that if give you an ordered array……okay, then go for binary search, apparently.

### code

``` C++
class Solution {
public:
    int hIndex(vector<int>& citations) {
        sort(citations.begin(),citations.end(),[](auto a,auto b){return a>b;});
        int h=0;
        for(int i=0;i<citations.size();i++){
            if(citations[i]>=h+1)h++;
        }
        return h;
        
    }
};
```



