# 二叉树的最大深度
- **英文**: Maximum Depth of Binary Tree
- **难度**: 简单
- **题目**: https://leetcode.cn/problems/maximum-depth-of-binary-tree/

## 题目描述
给定一个二叉树 `root` ，返回其最大深度。

二叉树的 **最大深度** 是指从根节点到最远叶子节点的最长路径上的节点数。

 

**示例 1：**

 

输入：root = [3,9,20,null,null,15,7]
输出：3

**示例 2：**

输入：root = [1,null,2]
输出：2

 

**提示：**

	
- 树中节点的数量在 `[0, 104]` 区间内。
	
- `-100 <= Node.val <= 100`

## 代码模板
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        
```

## Python 解法
```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))
```
