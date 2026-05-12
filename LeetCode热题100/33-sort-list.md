# 排序链表
- **英文**: Sort List
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/sort-list/

## 题目描述
给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。

 

**示例 1：**

输入：head = [4,2,1,3]
输出：[1,2,3,4]

**示例 2：**

输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]

**示例 3：**

输入：head = []
输出：[]

 

提示：

	
- 链表中节点的数目在范围 `[0, 5 * 104]` 内
	
- `-105 5`

 

进阶：你可以在 `O(n log n)` 时间复杂度和常数级空间复杂度下，对链表进行排序吗？

## 代码模板
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        
```

## Python 解法
```python
class Solution:
    def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head or not head.next: return head
        slow, fast = head, head.next
        while fast and fast.next: slow = slow.next; fast = fast.next.next
        mid, slow.next = slow.next, None
        left, right = self.sortList(head), self.sortList(mid)
        dummy = ListNode(); cur = dummy
        while left and right:
            if left.val <= right.val: cur.next = left; left = left.next
            else: cur.next = right; right = right.next
            cur = cur.next
        cur.next = left or right
        return dummy.next
```
