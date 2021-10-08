### Matrix

[832.翻转图像](https://leetcode-cn.com/problems/flipping-an-image/)

```
# 思路：遍历每一行并进行反转，得到反转每一行；然后遍历图片，反转每一个像素
# 时间复杂度：O(MN)   空间复杂度：O(MN)
# 时间：9mins

class Solution:
    def flipAndInvertImage(self, image: List[List[int]]) -> List[List[int]]:
        flip_image = []
        for row in image:
            flip_image.append(row[::-1])
        for i in range(len(flip_image)):
            for j in range(len(flip_image[0])):
                flip_image[i][j] = 0 if flip_image[i][j]==1 else 1
        
        return flip_image

思考：图像关于1和0的翻转，有2种思路
1、用 1- 当前值， 得到 0 和 1 的翻转
2、用 1^ 当前值， 得到 0 和 1 的反转，其中 ^ 表示异或操作，相同为0，相异为1
```

[867.转置矩阵](https://leetcode-cn.com/problems/transpose-matrix/)

```
# 时间：7mins
# 思路：先根据矩阵大小创建转置矩阵trans_m，然后将matrix[i][j] 赋值给trans_m[j][i]
# 时间复杂度：O(MN)，空间复杂度：O(MN)
class Solution:
    def transpose(self, matrix: List[List[int]]) -> List[List[int]]:
        nRows, nCols = len(matrix), len(matrix[0])
        trans_m = [[0 for i in range(nRows)] for j in range(nCols)]
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                trans_m[j][i] = matrix[i][j]
        return trans_m
```

[54.螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

```
# 思路1：螺旋矩阵的顺序 (0,0)-->(0,n)-->(m,n)-->(m,0)-->(1,0)-->...
# 如何判断为最终停止位置？--设置辅助矩阵visited用来标记该元素已经被访问,
# 当辅助矩阵被访问元素的个数等于矩阵的长度时，矩阵访问完成
# 时间复杂度 O(MN)  空间复杂度 O(MN)
# 时间：65mins
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        m, n = len(matrix), len(matrix[0])
        visited = [[False for _ in range(n)] for _ in range(m)]
        res = []   # 用来记录螺旋顺序的访问元素
        directions = [[0, 1], [1, 0], [0, -1], [-1, 0]]
        i, j = 0, 0
        directionIndex = 0
        for _ in range(m*n):
            res.append(matrix[i][j])
            visited[i][j] = True
            # 给出下一个元素的位置
            nextRow, nextColumn = i+directions[directionIndex][0], j+directions[directionIndex][1]
            # 判断该元素位置是否越界，如果越界，则改变螺旋方向
            if not (0 <=nextRow < m and 0 <=nextColumn< n and not visited[nextRow][nextColumn]):
                directionIndex = (directionIndex + 1) % 4
            i += directions[directionIndex][0]
            j += directions[directionIndex][1]
        return res

# 思路2：设置上下左右四个位置，按照顺序自增、自减来达到螺旋访问的目的
# 终止条件：当上大于下或者左大于右时
# 时间复杂度 O(MN)  空间复杂度 O(1)
# 时间：50mins
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        m, n = len(matrix), len(matrix[0])
        res = []
        # 注意初始条件中 down和right分别为m-1和n-1,这样则不会out of range
        up, down, left, right = 0, m-1, 0, n-1
        while True:
            for i in range(left, right+1):  # 注意边界条件
                res.append(matrix[up][i])
            # 访问完成后删除该行
            up += 1
            if down < up:
                break
            for i in range(up, down+1):
                res.append(matrix[i][right])
            right -= 1
            if right < left:
                break
            for i in range(right, left-1, -1):
                res.append(matrix[down][i])
            down -= 1
            if down < up:
                break
            for i in range(down, up-1, -1):
                res.append(matrix[i][left])
            left += 1
            if right < left:
                break
        return res
```

[73.矩阵置零](https://leetcode-cn.com/problems/set-matrix-zeroes/)

```
# 思路：找到矩阵中所有为0的元素，并使用列表记下相应的行和列，
# 遍历该列表，将相应的行和列置为0
# Time: O(MN)  Space: O(MN)
# 时间：8mins
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        zeroLists = []
        m, n = len(matrix), len(matrix[0])
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == 0:
                    zeroLists.append((i,j))
        # 遍历列表
        for pos in zeroLists:
            row, col = pos
            for i in range(m):
                matrix[i][col] = 0
            for j in range(n):
                matrix[row][j] = 0
        
        return matrix


# 改进思路：遍历矩阵找到为0的元素，使用2个list分别记录需要置为0的行和列，
# 最多需要 m + n个额外空间
# Time: O(MN)  Space: O(M+N)
# 时间：18mins
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        zeroRows, zeroCols = set(), set()
        m, n = len(matrix), len(matrix[0])
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == 0:
                    zeroRows.add(i)
                    zeroCols.add(j)
                # 遍历列表
        # 将相应的行列置零
        for row in zeroRows:
            for j in range(n):
                matrix[row][j] = 0
        for col in zeroCols:
            for i in range(m):
                print(matrix[i][col])
                matrix[i][col] = 0

        return matrix


# 改进思路：采用矩阵的第一行和第一列作为标志位来记录置0的列和行，同时设立2个
# 标志位来记录第一行和第一列是否为0
# Time: O(MN)  Space: O(1)
# 时间：
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        row0_zeroFlag, col0_zeroFlag = False, False
        rows, cols = len(matrix), len(matrix[0])
        for i in range(rows):
            for j in range(cols):
                # 如果第0行或者第0列存在元素为0，则将相应的标志位置为True
                if matrix[i][j] == 0 and i==0:
                    row0_zeroFlag = True
                if matrix[i][j] == 0 and j==0:
                    col0_zeroFlag = True
                if matrix[i][j] == 0:
                    # 将相应的行和列置为0
                    matrix[0][j] = 0
                    matrix[i][0] = 0
        
        # 访问第一行，将第一列往后相应的列置为0
        for j in range(1, cols):
            if matrix[0][j] == 0:
                for i in range(1, rows):
                    matrix[i][j] = 0
        # 访问第一列，将第一行往后的相应的行置为0
        for i in range(1, rows):
            if matrix[i][0] == 0:
                for j in range(1, cols):
                    matrix[i][j] = 0
        # 如果row0_zer0Flag为True, 将第一行置为0
        if row0_zeroFlag:
            for j in range(cols):
                matrix[0][j] = 0
        # 如果cols_zeroFlag为True，将第一列置为0
        if col0_zeroFlag:
            for i in range(rows):
                matrix[i][0] = 0
        
        return matrix
```