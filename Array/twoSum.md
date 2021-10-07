[TwoSum](https://leetcode-cn.com/problems/two-sum/)
+ 思路1：遍历数组，暴力求解找出满足条件的数；若不存在，则返回空[]。 Time: O(N2)  Space: O(1)
+ 思路2：哈希表思路，通过哈希表记录数组中的元素，若存在target-num元素存在，则返回相应的index；
如不存在，则返回空列表[]。  Time: O(N)  Space: O(N)  

**注意！**: 由于题目本身未说明数组是否按序排列，因此不能使用二分查找！！

```
# 思路：遍历数组，通过二分查找找出是否存在让和等于目标的元素, 注意，该题目
# 并没有说明整数数组有序，因此此题用二分查找是行不通的！！！ -- 该思路不可行！！！
# Time：O(NlogN)  Space: O(1)

def binary_search(arr, target):
    low, high = 0, len(arr)-1
    while low<=high:
        mid = (low+high)//2
        if arr[mid] <= target:
            low = mid + 1
        elif arr[mid] > target:
            high = mid - 1
    return low-1

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for idx, i in enumerate(nums):
            find_target = target - i
            find_idx = binary_search(nums, find_target)
            if (nums[find_idx] + nums[idx]) == target and find_idx != idx:
                return idx, find_idx


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
