#### 动态规划
**动态规划题目特点**：

学习视频：[动态规划算法讲解](https://www.bilibili.com/video/BV1xb411e7ww)
+ 1.计数
    - 有多少种方式走到右下角
    - 有多少种方法选出k个数使得和是sum
+ 2.求最大最小值
    - 从左上角走到右下角路径的最大数字和
    - 最长上升子序列长度
+ 3.求存在性
    - 取石子游戏，先手是否必胜
    - 能不能选出k个数使得和是Sum
    
**解动态规划问题的流程**:
+ 确定状态
+ 状态转移方程
+ 初始条件和边界情况
+ 计算顺序

[221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)
```
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        """
        思路：先找到只包含1的最大方形，然后根据方形的长和宽返回正方形的面积
        如何找到最大方形？采用动态规划的方法，dp[i][j]代表的是以matrix[i][j]为
        右下角的正方形的最大边长maxSide，
        情况1：如果matrix[i][j]为0，则dp[i][j]也为0
        情况2：如果matrix[i][j]为1，而dp[i][j]则是取决于其左方、上方、左上方的取值
        状态转移方程为： dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1
        Time: O(m*n)   Space: O(m*n)  -- 其中，m,n为矩形的长和宽
        时间：50mins(参考题解)
        """
        m, n = len(matrix), len(matrix[0])
        # 建立初始化数组dp
        dp = [[0 for _ in range(n)] for _ in range(m)]
        # maxSide表示最大正方形的边长
        maxSide = 0
        for i in range(m):
            for j in range(n):
                if i==0 or j==0:
                    dp[i][j] = int(matrix[i][j])
                else:
                    if matrix[i][j] == '0':
                        dp[i][j] = 0
                    else:
                        dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1
                maxSide = max(maxSide, dp[i][j])
        return maxSide*maxSide
```

[121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)
+ 解法1：暴力求解(TLE)。遍历求解第i天买进，第j天卖出，其中i<j。 Time:O(n2)  Space:O(1)
+ 解法2：动态规划。利用minPrice记录当前的股票最低价格，同时利用maxProfit来记录当前以最低价卖出的最大利润，
执行顺序为0-n, 最后返回最大利润即可。 Time:O(n)  Space:O(1)
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        """
        暴力求解：计算从第n天开始卖出的最大利润
        Time: O(n2)  Space: O(1)
        时间：5mins(TLE)
        """
        maxProfit = 0
        for i in range(len(prices)):
            for j in range(i, len(prices)):
                maxProfit = max(maxProfit, prices[j]-prices[i])
        return maxProfit

# 动态规划
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        """
        使用minPrices记录当前的最低股价，使用maxProfit来记录当前的最大利益
        执行的顺序：从左至右
        Time: O(N)  Space: O(1)
        时间：5mins
        """
        maxProfit, minPrice = 0, int(1e9)   # 用一个大数来表示即可
        for price in prices:
            maxProfit = max(maxProfit, price-minPrice)
            minPrice = min(price, minPrice)
        return maxProfit
```

[198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)
```
class Solution:
    def rob(self, nums: List[int]) -> int:
        """
        思路：每一个位置都有2种状态，偷窃或者不偷窃，同时该状态会影响相邻位置的状态，状态转移方程为：
        dp[i] = max(nums[i]+dp[i-2], dp[i-1]), dp[i] 代表偷到当前位置所能够得到的最大金额
        状态转移方程的意思：当选择偷当前第i家时，那么最大的金额就是当前金额加上i-2的最大金额，和不偷i，偷i-1
        家时的最大金额。
        初始条件：当只有一户人家时，直接偷窃dp[0]=num[0]; 当只有2户人家时，选择金额大的进行偷窃dp[1] = max(nums[0], nums[1])
        Time: O(n)    Space: O(n)
        时间：40mins(做题+题解)
        """
        n = len(nums)
        if n <= 2:
            return max(nums)
        # 初始化条件
        dp = [i for i in nums]
        dp[1] = max(nums[0], nums[1])
        for i in range(2, n):
            dp[i] = max(nums[i]+dp[i-2], dp[i-1])
        return dp[n-1]

# 空间优化：在dp数组中，dp[i]的值只取决于dp[i-1], nums[i]和dp[i-1], 因此可使用2个变量来替代dp数组
class Solution:
    def rob(self, nums: List[int]) -> int:
        """
        优化：使用2个变量 prev, cur 来替代dp数组；其中prev表示dp[i-2], cur表示dp[i-1]
        Time: O(N)  Space: O(1)
        """
        n = len(nums)
        if n <= 2:
            return max(nums)
        prev, cur = nums[0], max(nums[0], nums[1])
        for i in range(2, n):
            temp = max(nums[i]+prev, cur)
            prev, cur = cur, temp
        return cur
```


[213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)
```
class Solution:
    def rob(self, nums: List[int]) -> int:
        """
        由于存在首尾相连的情况，因此需要分2种情况：
        1. 不偷第一间房子所得到的收益profit1
        2. 不偷最后一间房子所得到的收益profit2
        最终，选择2者的最大值即可
        Time: O(2n)   Space: O(2n)
        时间：28mins(题解+代码)
        """
        n = len(nums)
        if n<=2:
            return max(nums)
        def robProfit(nums):
            n = len(nums)
            if n<= 2:
                return max(nums)
            dp = [i for i in nums]
            dp[1] = max(nums[0], nums[1])
            for i in range(2, n):
                dp[i] = max(dp[i-2]+nums[i], dp[i-1])
            return dp[n-1]

        return max(robProfit(nums[1:]), robProfit(nums[:-1]))

# 代码优化，用2个变量来替换数组
class Solution:
    def rob(self, nums: List[int]) -> int:
        def robProfit(nums):
            prev, cur = 0, 0
            for i in nums:
                prev, cur = cur, max(i+prev, cur)
            return cur
        n = len(nums)
        if n<=2:
            return max(nums)

        return max(robProfit(nums[1:]), robProfit(nums[:-1]))
```


[337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: TreeNode) -> int:
        """
        思路：选择o节点，使用f(o)表示选中的情况子树的最大和，g(o)表示不选择o节点的情况子树最大和；
        Situation1： o被选中情况下子树，其左右节点均不能被选中，因此 f(o) = g(l) + g(r), 其中l, r分别
        表示o节点的左子树和右子树； Situation2： o不选择情况下，g(o) = max{f(l), g(l)} + max{f(r), g(r)}
        采用2个列表存储分别选中当前节点和不选中当前节点的取值，然后使用后序遍历. 
        Time: O(n)  Space:  O(2n)
        时间：50mins
        """
        def dp(node):
            # 递归终止条件
            if not node:
                return [0, 0]
            left = dp(node.left)
            right = dp(node.right)
            return [node.val + left[1] + right[1], max(left[0], left[1]) + max(right[0], right[1])]
        out = dp(root)
        return max(out[0],out[1])

# Solution 1 -- 暴力递归搜索
class Solution:
    def rob(self, root: TreeNode) -> int:
        """
        思路：比较根节点和4个孙子节点所选取的最大价值value1，和根节点的2个孩子节点所选取的最大价值value2，
        选取其中大的那个
        时间：15mins(TLE)
        """
        if root is None:
            return 0
        
        money = root.val
        if root.left:
            money += self.rob(root.left.left) + self.rob(root.left.right)
        if root.right:
            money += self.rob(root.right.left) + self.rob(root.right.right)

        return max(money, self.rob(root.left) + self.rob(root.right))

# Solution 2 -- 带备忘录的递归搜索
memo = {}
class Solution:
    def rob(self, root: TreeNode) -> int:
        """
        时间：6mins
        """
        if root is None:
            return 0
        if root in memo:
            return memo[root]
        
        money = root.val
        if root.left:
            money += self.rob(root.left.left) + self.rob(root.left.right)

        if root.right:
            money += self.rob(root.right.left) + self.rob(root.right.right)

        memo[root] = max(money, self.rob(root.left)+self.rob(root.right))
        return memo[root]
```

#### 二维动态规划问题
[1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)