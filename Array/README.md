[367. 有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/)
```
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        """
        思路：从1遍历到n/2，如果不存在 i 的平方等于num，则返回False
        其中，处理边界要注意 num//2+1 当num=1时，不执行for循环
        Time: O(n/2)  Space: O(1)
        时间：8mins(TLE)
        """
        if num==1:
            return True
        for i in range(1, num//2+1):
            if i**2 == num:
                return True
        return False


# 进一步优化，使用while循环进行判断, 时间复杂度O(sqrt(n))
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        """
        Time: O(sqrt(n))  Space: O(1)
        """
        i = 1
        while i*i < num:
            i += 1
        return i*i == num

# 使用二分查找，在[0, n//2+1]区间进行查找，时间复杂度O(log2(n//2))
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        """
        思路：使用二分查找，从1遍历到n/2，如果不存在 i 的平方等于num，则返回False
        Time: O(n/2)  Space: O(1)
        时间：
        """
        low, high = 0, num//2+1
        while low <= high:
            mid = (low + high) // 2
            midSquare = mid**2
            if midSquare == num:
                return True
            elif midSquare < num:
                low = mid + 1
            elif midSquare > num:
                high = mid - 1
        return False
```

[268. 丢失的数字](https://leetcode-cn.com/problems/missing-number/)
```
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        """
        思路：遍历一遍，找出不在[0, n]范围内的数，利用Python语言的成员判断符
        Time: O(n2), 其中 in 成员判断符时间复杂度为O(n)  Space: O(1)
        时间：3mins
        """
        maxN = len(nums)
        for i in range(maxN+1):
            if i not in nums:
                return i

class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        """
        思路：将数组排序，找出下标和元素值不相等的该元素; 有2种情况：
        1、缺失数字介于 [0, n-1]之间  2、缺失数字为n
        Time: O(nlog2n),其中排序时间复杂度为nlog2n  Space: O(nlogn) 排序所占用堆栈大小
        时间：8mins
        """
        nums.sort()
        for idx,i in enumerate(nums):
            # 情况1
            if idx != i:  # 不要使用 nums[i] = [i] 进行判断，当情况为2时会越界
                return idx
        # 情况2
        return len(nums)

# O(N)解法--其中，集合Set(哈希表)的 in 运算符时间复杂度为O(1), 优于列表(list)
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        """
        思路：首先对所有元素建立哈希表，然后判断[0,n]哪个元素不在哈希表中
        Time: O(n)  Space:O(n) 哈希表所占大小
        时间: 12mins
        """
        hashSet = set(nums)
        for i in range(len(nums)+1):
            if i not in hashSet:
                return i

# 位运算--异或解法--Time:O(N) Space:O(1)
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        """
        思路：由于[0, n]这n+1个元素中缺了一个，因此先求前n个数的异或之和，再逐一对[0, n]每个数进行异或;
        最终的值就是缺失的元素
        异或的性质： x xor x = 0;  x xor 0 = x;
        Time: O(n)  Space: O(1)
        时间：25mins
        ans = 0
        for i in nums:
            ans ^= i
        for i in range(len(nums)+1):
            ans ^= i
        return ans
        """
        # 优化版,只需要一次循环
        ans = 0
        for idx, i in enumerate(nums):
            ans ^= i ^ idx
        return ans ^ len(nums)

# 数学运算--Time：O(N) Space：O(1)
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        """
        思路：0,1,...,n 个元素之和为 n(n+1)//2; 由于nums中缺少一个元素，因此用 n(n+1)//2减去
        nums的和，得到的差值即为缺少的元素
        Time: O(n)--其中O(n)为nums中n个元素相加所占用的时间复杂度 Space: O(1)
        时间：
        """
        res = sum(nums)  # sum() -- O(n)
        n = len(nums)   # len() -- O(1)
        return n*(n+1)//2 - res
```