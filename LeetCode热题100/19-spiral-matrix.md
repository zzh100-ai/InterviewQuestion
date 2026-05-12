# 螺旋矩阵
- **英文**: Spiral Matrix
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/spiral-matrix/

## 题目描述
给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

 

**示例 1：**

**输入：**matrix = [[1,2,3],[4,5,6],[7,8,9]]
**输出：**[1,2,3,6,9,8,7,4,5]

**示例 2：**

**输入：**matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
**输出：**[1,2,3,4,8,12,11,10,9,5,6,7]

 

**提示：**

	
- `m == matrix.length`
	
- `n == matrix[i].length`
	
- `1 <= m, n <= 10`
	
- `-100 <= matrix[i][j] <= 100`

## 代码模板
```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        
```

## Python 解法
```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        res = []
        while matrix:
            res += matrix.pop(0)
            if matrix and matrix[0]:
                for row in matrix: res.append(row.pop())
            if matrix: res += matrix.pop()[::-1]
            if matrix and matrix[0]:
                for row in matrix[::-1]: res.append(row.pop(0))
        return res
```
