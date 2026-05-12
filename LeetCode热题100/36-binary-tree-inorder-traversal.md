# 二叉树的中序遍历
- **英文**: Binary Tree Inorder Traversal
- **难度**: 简单
- **题目**: https://leetcode.cn/problems/binary-tree-inorder-traversal/

## 题目描述
给定一个二叉树的根节点 `root` ，返回 *它的 **中序** 遍历* 。

 

**示例 1：**

**输入：**root = [1,null,2,3]
**输出：**[1,3,2]

**示例 2：**

**输入：**root = []
**输出：**[]

**示例 3：**

**输入：**root = [1]
**输出：**[1]

 

**提示：**

	
- 树中节点数目在范围 `[0, 100]` 内
	
- `-100 <= Node.val <= 100`

 

**进阶:** 递归算法很简单，你可以通过迭代算法完成吗？

## 代码模板
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        
```

## Python 解法
```python
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res, stack, cur = [], [], root
        while cur or stack:
            while cur: stack.append(cur); cur = cur.left
            cur = stack.pop(); res.append(cur.val); cur = cur.right
        return res
```
