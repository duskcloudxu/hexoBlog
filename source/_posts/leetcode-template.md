---
title: leetcode template
date: 2020-01-28 16:30:16
tags:
- leetcode
- template
---

Here is the collection of best practices of common algorithm pattern I have seen on leetcode.

## LinkedList

### Fast and Slow Pointers

#### Java

``` java
/*
If there is an middle node in the linkedList(e.g. 1->2->3->4->5, 3 is the middle node, versus 1->2->3->4, there is no middle node). then slow would point at the node after the middle node. Otherwise slow would point at the first node of the second half linkedlist.
*/

ListNode slow=head,fast=head;
while(fast!=null&&fast.next!=null)
{
    slow=slow->next;
    fast=fast->next->next;
}
```

### Reverse Linkedlist

#### Java

``` java
public ListNode reverse(ListNode head){
    ListNode former=null;
    while(head!=null){
        ListNode temp=head.next;
        head.next=former;
        former=head;
        head=temp;
    }
    return former;
}

```

