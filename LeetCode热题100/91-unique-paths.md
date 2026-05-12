# 不同路径
- **英文**: Unique Paths
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/unique-paths/

## 题目描述
一个机器人位于一个 `m x n`* *网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

 

**示例 1：**

**输入：**m = 3, n = 7
**输出：**28

**示例 2：**

**输入：**m = 3, n = 2
**输出：**3
**解释：**
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下

**示例 3：**

**输入：**m = 7, n = 3
**输出：**28

**示例 4：**

**输入：**m = 3, n = 3
**输出：**6

 

**提示：**

	
- `1 9`

## 代码模板
```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        
```

## Python 解法
```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        import math
        return math.comb(m + n - 2, m - 1)
```
