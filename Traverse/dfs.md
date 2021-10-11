### 深度优先搜索算法(DFS)
**首先明确搜索的定义或者遍历的定义**：从当前图中的某一顶点出发，按照某种搜索方法沿着图中的边对图中的
所有顶点**访问且仅访问一次**。

算法思想：对所有节点进行深度优先搜索，深度优先会优先考虑最后被发现的顶点。从图中某一起始顶点v出发，
访问与v邻接且未被访问的任一顶点w1, 再访问与w1邻接且未被访问的任一顶点w2...**重复上述过程**。当
不能再继续向下访问时，依次退回到最近被访问的顶点，若它还有邻接点未被访问过，则从该点开始继续上诉
搜索过程，直到遍历完成。

[690.员工的重要性](https://leetcode-cn.com/problems/employee-importance/)
```
class Solution:
    def getImportance(self, employees: List['Employee'], id: int) -> int:
        """
        DFS递归遍历
        Time: O(N)
        Space: O(N)
        时间：25mins
        """
        # 构建员工字典
        emps = {employee.id: employee for employee in employees}
        # 深度遍历
        def dfs(id):
            sub_importances = 0
            for subordinate in emps[id].subordinates:
                sub_importances += dfs(subordinate)
            return emps[id].importance + sub_importances
        return dfs(id)
```