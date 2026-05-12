# 二叉树展开为链表
- **英文**: Flatten Binary Tree to Linked List
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/

## 题目描述
给你二叉树的根结点 `root` ，请你将它展开为一个单链表：

	
- 展开后的单链表应该同样使用 `TreeNode` ，其中 `right` 子指针指向链表中下一个结点，而左子指针始终为 `null` 。
	
- 展开后的单链表应该与二叉树 **先序遍历** 顺序相同。

 

**示例 1：**

**输入：**root = [1,2,5,3,4,null,6]
**输出：**[1,null,2,null,3,null,4,null,5,null,6]

**示例 2：**

**输入：**root = []
**输出：**[]

**示例 3：**

**输入：**root = [0]
**输出：**[0]

 

**提示：**

	
- 树中结点数在范围 `[0, 2000]` 内
	
- `-100 <= Node.val <= 100`

 

**进阶：**你可以使用原地算法（`O(1)` 额外空间）展开这棵树吗？

## 代码模板
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def flatten(self, root: Optional[TreeNode]) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        
```

## Python 解法
```python
class Solution:
    def flatten(self, root: Optional[TreeNode]) -> None:
        cur = root
        while cur:
            if cur.left:
                pre = cur.left
                while pre.right: pre = pre.right
                pre.right = cur.right; cur.right = cur.left; cur.left = None
            cur = cur.right
```
