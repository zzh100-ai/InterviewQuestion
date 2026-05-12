# 二叉树的层序遍历
- **英文**: Binary Tree Level Order Traversal
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/binary-tree-level-order-traversal/

## 题目描述
给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

 

**示例 1：**

**输入：**root = [3,9,20,null,null,15,7]
**输出：**[[3],[9,20],[15,7]]

**示例 2：**

**输入：**root = [1]
**输出：**[[1]]

**示例 3：**

**输入：**root = []
**输出：**[]

 

**提示：**

	
- 树中节点数目在范围 `[0, 2000]` 内
	
- `-1000 <= Node.val <= 1000`

## 代码模板
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        
```

## Python 解法
```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        from collections import deque
        if not root: return []
        res, q = [], deque([root])
        while q:
            level = []
            for _ in range(len(q)):
                node = q.popleft(); level.append(node.val)
                if node.left: q.append(node.left)
                if node.right: q.append(node.right)
            res.append(level)
        return res
```
