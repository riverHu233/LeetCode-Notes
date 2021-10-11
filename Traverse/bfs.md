### 广度优先搜索算法(BFS)
**首先明确搜索的定义或者遍历的定义**：从当前图中的某一顶点出发，按照某种搜索方法沿着图中的边对图中的
所有顶点**访问且仅访问一次**。

算法思想：对所有节点进行广度优先搜索，广度优先会优先考虑离起点最近的顶点，当将离起点最近的节点都访问
完成后，再考虑访问这些节点的未被访问过的邻接节点，直到遍历完成。

在BFS中，是逐步求解，不同DFS的递归与回溯，更加注重状态的选取和标记。


[690.员工的重要性](https://leetcode-cn.com/problems/employee-importance/)

```
from collections import deque
class Solution:
    def getImportance(self, employees: List['Employee'], id: int) -> int:
        """
        BFS非递归遍历
        Time: O(N)
        Space: O(N)
        时间： 
        """
        emps = {employee.id: employee for employee in employees}
        # 广度遍历,头节点入队列
        que = deque([id])
        total = 0
        while que:
            curId = que.popleft()
            employee = emps[curId]
            for sub in employee.subordinates:
                que.append(sub) 
            total += employee.importance
        return total
```


[815.公交路线](https://leetcode-cn.com/problems/bus-routes/) (**该题只能用BFS求解而不能用DFS求解**)

