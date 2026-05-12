# 接雨水
- **英文**: Trapping Rain Water
- **难度**: 困难
- **题目**: https://leetcode.cn/problems/trapping-rain-water/

## 题目描述
给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

 

**示例 1：**

**输入：**height = [0,1,0,2,1,0,1,3,2,1,2,1]
**输出：**6
**解释：**上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 

**示例 2：**

**输入：**height = [4,2,0,3,2,5]
**输出：**9

 

**提示：**

	
- `n == height.length`
	
- `1 4`
	
- `0 5`

## 代码模板
```python
class Solution:
    def trap(self, height: List[int]) -> int:
        
```

## Python 解法
```python
class Solution:
    def trap(self, height: List[int]) -> int:
        if not height: return 0
        l, r = 0, len(height) - 1
        l_max = r_max = res = 0
        while l < r:
            if height[l] < height[r]:
                if height[l] >= l_max: l_max = height[l]
                else: res += l_max - height[l]
                l += 1
            else:
                if height[r] >= r_max: r_max = height[r]
                else: res += r_max - height[r]
                r -= 1
        return res
```
