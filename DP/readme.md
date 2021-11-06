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

