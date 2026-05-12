# 搜索二维矩阵 II
- **英文**: Search a 2D Matrix II
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/search-a-2d-matrix-ii/

## 题目描述
编写一个高效的算法来搜索 `*m* x *n*` 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：

	
- 每行的元素从左到右升序排列。
	
- 每列的元素从上到下升序排列。

 

示例 1：

输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
输出：true

示例 2：

输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
输出：false

 

**提示：**

	
- `m == matrix.length`
	
- `n == matrix[i].length`
	
- `1 9 9`
	
- 每行的所有元素从左到右升序排列
	
- 每列的所有元素从上到下升序排列
	
- `-109 9`

## 代码模板
```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        
```

## Python 解法
```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m, n = len(matrix), len(matrix[0])
        r, c = 0, n - 1
        while r < m and c >= 0:
            if matrix[r][c] == target: return True
            elif matrix[r][c] > target: c -= 1
            else: r += 1
        return False
```
