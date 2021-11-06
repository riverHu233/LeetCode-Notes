[TwoSum](https://leetcode-cn.com/problems/two-sum/)
+ 思路1：遍历数组，暴力求解找出满足条件的数；若不存在，则返回空[]。 Time: O(N2)  Space: O(1)
+ 思路2：哈希表思路，通过哈希表记录数组中的元素和相应元素下标，若存在target-num元素存在，则返回相应的index；
如不存在，则返回空列表[]。  Time: O(N)  Space: O(N)  

**注意！**: 由于题目本身未说明数组是否按序排列，因此不能使用二分查找！！

```
# 思路：遍历数组，通过二分查找找出是否存在让和等于目标的元素, 注意，该题目
# 并没有说明整数数组有序，因此此题用二分查找是行不通的！！！ -- 该思路不可行！！！

# 思路1：遍历查找，输出满足条件的i, j
# Time: O(N2)  Space: O(1)
# 时间： 3mins
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        n = len(nums)
        for i in range(n):
            for j in range(i+1, n):
                if (nums[i] + nums[j]) == target:
                    return i, j


# 思路2：采用字典记录各元素(哈希表)，如果存在匹配，则直接返回
# Time: O(N)  Space: O(N)
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashset = dict()
        
        for i,num in enumerate(nums):
            if target-num in hashset:
                return [hashset[target-num], i]
            
            hashset[num] = i
        return []
```

[ThreeSum](https://leetcode-cn.com/problems/3sum/)
+ 思路：排序+双指针。 Time:O(n2), Space:O(1)
```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        """
        排序+双指针：题目不需要返回数组下标，先对数组进行排序，然后使用双指针进行搜索
        问题：如何去除重复解(暴力法无法避免重复解) -- 
        双指针法：其中 a = -(b+c) = target, b从左边开始取值，c从右边开始取值，
        当 b+c大于target时，需要缩小b+c的和，将c的指针左移，最终，b > c 退出循环。
        如何避免重复解：1, 当a 当前取值和 a+1取值相同时，a ++ 
                        2, b 当前取值和 b+1取值相同时，b ++ 
        Time: O(n2)  Space: O(n)
        """
        ans = []
        # 数组排序
        nums.sort()
        for i in range(len(nums)):
            if i>0 and nums[i] == nums[i-1]:
                continue
            target = - nums[i]
            k = len(nums) - 1
            for j in range(i+1, len(nums)):
                if j>i+1 and nums[j] == nums[j-1]:
                    continue
                while j<k and (nums[k] + nums[j]) > target:
                    k -= 1
                # 终止条件
                if j == k:
                    break
                if (nums[k] + nums[j]) == target:
                    ans.append([nums[i], nums[j], nums[k]])
        return ans
```