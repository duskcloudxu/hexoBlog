---
title: max queue implementation
date: 2019-10-22 16:06:02
tags:
- algorithm
- max queue

---

## Introduction

Recently I am viewing some interview questions in my spare time and found a very interesting interview question:

> implement a queue with a push, pop, and findMax in constant time

This problem seems easy in the very beginning. I thought I just need to use a variable to keep max value whenever a value was pushed in the queue. But the queue has a pop method and you never know when the max value you keep would be popped out.



Ok, let's find if there is another solution. I know a very similar problem:

> implement a stack with push(), pop(), and findMax() method in constant time.

Now we should remember that the stack is a FILO data structure, which means that the first element pushed into the stack would be last out. Using this property of stack, we can store state of current max value in each element of stack. If we need to know the current max value in stack, we just visit the max value stored on the top of stack. if we pop out some elements, then a bottom element would be the new top element, and the current state of the whole stack would be as same as it was inserted, which means the  max value stored in it is still right.



But that is not the case in queue since queue is a FIFO data structure. But still, we can make use of this property. FIFO means the first element pushed in would be first out. which means, if there is an element B pushed later than element A, then it should be popped out later then element A.  



For a queue at each state, we should save it current max value and also the value that would be the max value after all the element in the front pushed out, and let us call it `maxValueQueue`. By using that strategy, we would retrieve max value of a queue in the constant time at any state. For example, if we have a sample data `[3,2,1,5,3,1]` and the method calling order is `[push, push, push, push, pop,pop,pop, push]`.  So the first three iteration could be like that:

`[3]`

`[3,2]`

`[3,2,1]`

and in next push operation, it is obvious that we would push element `5`. But wait, our strategy is to **state the max value after all the elements in the front pushed out**. `5` is pushed later than `3`,`1`,`2`,`4`, therefore, it should be popped out later than any of element pushed earlier than it.  Therefore, the `maxValueQueue` should be changed, and all the value smaller than `5` should become `5`. So our `maxValueQueue` become that:

`[3,2,1]`->`[5,5,5,5]`

and in each pop, we just pop the original queue and `maxValueQueue` at the same time.

But wait, can we do better there?

Sure, how about just do not change the former value smaller than the current max value, but drop it directly?

use the former example, if we need to insert current max value `5`, we just pop element from back, until there is not element bigger than `5`.

`[3,2,1]`->`[5]`

And in case of pop element, you just pop the first element if it is equal to the element that going to be popped out.

## Code

This question is common in many interviews as well as some coding challenge, such as  `Leetcode 239`:

``` C++
class Solution {
public:
	deque<int>startMaxNums;
	queue<int>storeQueue;
	void push(int n) {
		if (!storeQueue.size()) {
			storeQueue.push(n);
			startMaxNums.push_back(n);
			return;
		}
		while (startMaxNums.size() && startMaxNums.back() < n) {
			startMaxNums.pop_back();
		}
		startMaxNums.push_back(n);
		storeQueue.push(n);
	}
	void pop() {
		if (storeQueue.front() == startMaxNums.front()) {
			startMaxNums.pop_front();
		}
		storeQueue.pop();

	}
	int findMax() {
		return startMaxNums.front();
	}
	void printList() {
	}
	vector<int> maxSlidingWindow(vector<int>& nums, int k) {
		if (!nums.size())return {};
		vector<int>res;
		for (int i = 0;i < k;i++) {
			push(nums[i]);
		}
		res.push_back(findMax());
		for (int i = k;i < nums.size();i++) {
			pop();
			push(nums[i]);
			res.push_back(findMax());
		}
		return res;
	}
};
```

