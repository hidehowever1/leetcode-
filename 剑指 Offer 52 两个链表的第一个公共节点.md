# 数组中最大数对和的最小值

> 输入两个链表，找出它们的第一个公共节点。
> ![图片](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)



>>  ### 思路解析：
>>> 我们使用两个指针 node1，node2 分别指向两个链表 headA，headB 的头结点，然后同时分别逐结点遍历，当 node1 到达链表 headA 的末尾时，重新定位到链表 headB 的头结点；当 node2 到达链表 headB 的末尾时，重新定位到链表 headA 的头结点。
这样，当它们相遇时，所指向的结点就是第一个公共结点。

>>> #### 代码
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        a=headA
        b=headB
        while a!=b:
            if a:
                a=a.next
            else:
                a=headB
            if b:
                b=b.next
            else:
                b=headA
        return a
```
