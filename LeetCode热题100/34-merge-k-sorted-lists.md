# 合并 K 个升序链表
- **英文**: Merge k Sorted Lists
- **难度**: 困难
- **题目**: https://leetcode.cn/problems/merge-k-sorted-lists/

## 题目描述
给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

 

**示例 1：**

**输入：**lists = [[1,4,5],[1,3,4],[2,6]]
**输出：**[1,1,2,3,4,4,5,6]
**解释：**链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6

**示例 2：**

**输入：**lists = []
**输出：**[]

**示例 3：**

**输入：**lists = [[]]
**输出：**[]

 

**提示：**

	
- `k == lists.length`
	
- `0 <= k <= 10^4`
	
- `0 <= lists[i].length <= 500`
	
- `-10^4 <= lists[i][j] <= 10^4`
	
- `lists[i]` 按 **升序** 排列
	
- `lists[i].length` 的总和不超过 `10^4`

## 代码模板
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        
```

## Python 解法
```python
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        import heapq
        heap = []
        for i, lst in enumerate(lists):
            if lst: heapq.heappush(heap, (lst.val, i, lst))
        dummy = ListNode(); cur = dummy
        while heap:
            _, i, node = heapq.heappop(heap)
            cur.next = node; cur = cur.next
            if node.next: heapq.heappush(heap, (node.next.val, i, node.next))
        return dummy.next
```
