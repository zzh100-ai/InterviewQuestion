# 反转链表
- **英文**: Reverse Linked List
- **难度**: 简单
- **题目**: https://leetcode.cn/problems/reverse-linked-list/

## 题目描述
给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

 

**示例 1：**

**输入：**head = [1,2,3,4,5]
**输出：**[5,4,3,2,1]

**示例 2：**

**输入：**head = [1,2]
**输出：**[2,1]

**示例 3：**

**输入：**head = []
**输出：**[]

 

**提示：**

	
- 链表中节点的数目范围是 `[0, 5000]`
	
- `-5000

## 代码模板
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        
```

## Python 解法
```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev = None
        cur = head
        while cur:
            nxt = cur.next; cur.next = prev; prev = cur; cur = nxt
        return prev
```
