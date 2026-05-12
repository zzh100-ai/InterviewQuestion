# 回文链表
- **英文**: Palindrome Linked List
- **难度**: 简单
- **题目**: https://leetcode.cn/problems/palindrome-linked-list/

## 题目描述
给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

**输入：**head = [1,2,2,1]
**输出：**true

**示例 2：**

**输入：**head = [1,2]
**输出：**false

 

**提示：**

	
- 链表中节点数目在范围`[1, 105]` 内
	
- `0 <= Node.val <= 9`

 

**进阶：**你能否用 `O(n)` 时间复杂度和 `O(1)` 空间复杂度解决此题？

## 代码模板
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        
```

## Python 解法
```python
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        if not head: return True
        slow = fast = head
        while fast and fast.next:
            slow = slow.next; fast = fast.next.next
        prev = None
        while slow:
            nxt = slow.next; slow.next = prev; prev = slow; slow = nxt
        while prev:
            if head.val != prev.val: return False
            head = head.next; prev = prev.next
        return True
```
