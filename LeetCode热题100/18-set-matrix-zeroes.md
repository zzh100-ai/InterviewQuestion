# 矩阵置零
- **英文**: Set Matrix Zeroes
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/set-matrix-zeroes/

## 题目描述
给定一个 `*m* x *n*` 的矩阵，如果一个元素为 **0 **，则将其所在行和列的所有元素都设为 **0** 。请使用 **原地** 算法**。**

 

**示例 1：**

**输入：**matrix = [[1,1,1],[1,0,1],[1,1,1]]
**输出：**[[1,0,1],[0,0,0],[1,0,1]]

**示例 2：**

**输入：**matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
**输出：**[[0,0,0,0],[0,4,5,0],[0,3,1,0]]

 

**提示：**

	
- `m == matrix.length`
	
- `n == matrix[0].length`
	
- `1 31 31 - 1`

 

**进阶：**

	
- 一个直观的解决方案是使用  `O(*m**n*)` 的额外空间，但这并不是一个好的解决方案。
	
- 一个简单的改进方案是使用 `O(*m* + *n*)` 的额外空间，但这仍然不是最好的解决方案。
	
- 你能想出一个仅使用常量空间的解决方案吗？

## 代码模板
```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        
```

## Python 解法
```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        m, n = len(matrix), len(matrix[0])
        first_row = any(matrix[0][j] == 0 for j in range(n))
        first_col = any(matrix[i][0] == 0 for i in range(m))
        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][j] == 0:
                    matrix[i][0] = matrix[0][j] = 0
        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][0] == 0 or matrix[0][j] == 0:
                    matrix[i][j] = 0
        if first_row:
            for j in range(n): matrix[0][j] = 0
        if first_col:
            for i in range(m): matrix[i][0] = 0
```
