# 二叉树的右视图
- **英文**: Binary Tree Right Side View
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/binary-tree-right-side-view/

## 题目描述
给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

 

**示例 1：**

输入：root = [1,2,3,null,5,null,4]

**输出：**[1,3,4]

**解释：**

**示例 2：**

输入：root = [1,2,3,4,null,null,null,5]

输出：[1,3,4,5]

**解释：**

**示例 3：**

**输入：**root = [1,null,3]

**输出：**[1,3]

**示例 4：**

输入：root = []

**输出：**[]

 

**提示:**

	
- 二叉树的节点个数的范围是 `[0,100]`
	
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
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        
```

## Python 解法
```python
class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        from collections import deque
        if not root: return []
        res, q = [], deque([root])
        while q:
            res.append(q[-1].val)
            for _ in range(len(q)):
                node = q.popleft()
                if node.left: q.append(node.left)
                if node.right: q.append(node.right)
        return res
```
