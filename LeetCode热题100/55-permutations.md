# 全排列
- **英文**: Permutations
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/permutations/

## 题目描述
给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

 

**示例 1：**

**输入：**nums = [1,2,3]
**输出：**[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

**示例 2：**

**输入：**nums = [0,1]
**输出：**[[0,1],[1,0]]

**示例 3：**

**输入：**nums = [1]
**输出：**[[1]]

 

**提示：**

	
- `1 <= nums.length <= 6`
	
- `-10 <= nums[i] <= 10`
	
- `nums` 中的所有整数 **互不相同**

## 代码模板
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        
```

## Python 解法
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res, n = [], len(nums)
        def backtrack(path, used):
            if len(path) == n: res.append(path[:]); return
            for i in range(n):
                if not used[i]:
                    used[i] = True; path.append(nums[i])
                    backtrack(path, used)
                    path.pop(); used[i] = False
        backtrack([], [False] * n)
        return res
```
