# K 个一组翻转链表
- **英文**: Reverse Nodes in k-Group
- **难度**: 困难
- **题目**: https://leetcode.cn/problems/reverse-nodes-in-k-group/

## 题目描述
给你链表的头节点 `head` ，每 `k`* *个节点一组进行翻转，请你返回修改后的链表。

`k` 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 `k`* *的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

 

**示例 1：**

**输入：**head = [1,2,3,4,5], k = 2
**输出：**[2,1,4,3,5]

**示例 2：**

**输入：**head = [1,2,3,4,5], k = 3
**输出：**[3,2,1,4,5]

 

**提示：**

	
- 链表中的节点数目为 `n`
	
- `1 <= k <= n <= 5000`
	
- `0 <= Node.val <= 1000`

 

**进阶：**你可以设计一个只用 `O(1)` 额外内存空间的算法解决此问题吗？

## 代码模板
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        
```

## Python 解法
```python
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        cur, cnt = head, 0
        while cur and cnt < k: cur = cur.next; cnt += 1
        if cnt == k:
            prev, cur = None, head
            for _ in range(k):
                nxt = cur.next; cur.next = prev; prev = cur; cur = nxt
            head.next = self.reverseKGroup(cur, k)
            return prev
        return head
```
