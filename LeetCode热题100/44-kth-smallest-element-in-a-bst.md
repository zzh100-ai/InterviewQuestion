# 二叉搜索树中第 K 小的元素
- **英文**: Kth Smallest Element in a BST
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/kth-smallest-element-in-a-bst/

## 题目描述
给定一个二叉搜索树的根节点 `root` ，和一个整数 `k` ，请你设计一个算法查找其中第 `k`** **小的元素（`k` 从 1 开始计数）。

 

**示例 1：**

**输入：**root = [3,1,4,null,2], k = 1
**输出：**1

**示例 2：**

**输入：**root = [5,3,6,2,4,null,null,1], k = 3
**输出：**3

 

 

**提示：**

	
- 树中的节点数为 `n` 。
	
- `1 4`
	
- `0 4`

 

**进阶：**如果二叉搜索树经常被修改（插入/删除操作）并且你需要频繁地查找第 `k` 小的值，你将如何优化算法？

## 代码模板
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        
```

## Python 解法
```python
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        stack = []
        while True:
            while root: stack.append(root); root = root.left
            root = stack.pop(); k -= 1
            if k == 0: return root.val
            root = root.right
```
