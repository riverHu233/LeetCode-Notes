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