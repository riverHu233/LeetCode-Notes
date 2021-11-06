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