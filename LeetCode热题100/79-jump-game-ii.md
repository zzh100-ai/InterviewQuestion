# 跳跃游戏 II
- **英文**: Jump Game II
- **难度**: 中等
- **题目**: https://leetcode.cn/problems/jump-game-ii/

## 题目描述
给定一个长度为 `n` 的 **0 索引**整数数组 `nums`。初始位置在下标 0。

每个元素 `nums[i]` 表示从索引 `i` 向后跳转的最大长度。换句话说，如果你在索引 `i` 处，你可以跳转到任意 `(i + j)` 处：

	
- `0 
**输入:** nums = [2,3,1,1,4]
**输出:** 2
**解释:** 跳到最后一个位置的最小跳跃数是 `2`。
     从下标为 0 跳到下标为 1 的位置，跳 `1` 步，然后跳 `3` 步到达数组的最后一个位置。

**示例 2:**

**输入:** nums = [2,3,0,1,4]
**输出:** 2

 

**提示:**

	
- `1 4`
	
- `0 <= nums[i] <= 1000`
	
- 题目保证可以到达 `n - 1`

## 代码模板
```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        
```

## Python 解法
```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        jumps = cur_end = farthest = 0
        for i in range(len(nums) - 1):
            farthest = max(farthest, i + nums[i])
            if i == cur_end: jumps += 1; cur_end = farthest
        return jumps
```
