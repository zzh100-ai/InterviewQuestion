# 对称二叉树
- **英文**: Symmetric Tree
- **难度**: 简单
- **题目**: https://leetcode.cn/problems/symmetric-tree/

## 题目描述
给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

 

**示例 1：**

**输入：**root = [1,2,2,3,4,4,3]
**输出：**true

**示例 2：**

**输入：**root = [1,2,2,null,3,null,3]
**输出：**false

 

**提示：**

	
- 树中节点数目在范围 `[1, 1000]` 内
	
- `-100 <= Node.val <= 100`

 

**进阶：**你可以运用递归和迭代两种方法解决这个问题吗？

## 代码模板
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        
```

## Python 解法
```python
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        def check(left, right):
            if not left and not right: return True
            if not left or not right: return False
            return left.val == right.val and check(left.left, right.right) and check(left.right, right.left)
        return check(root, root)
```
