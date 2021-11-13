#### 选择排序(SelectSort)
**算法思想**：每次(第i次)选择一个关键字最小的元素，作为有序子序列中的第i个元素，当数组元素只剩下一个时，排序结束。

时间复杂度：O(n2)  空间复杂度：O(1)

由于选择排序是从每一次中找出最小的元素的关键字(位置索引)，因此直接将arr[i]和arr[minIdx]进行交换即可，所以空间复杂度为O(1)。
同时，由于发生元素的交换，所以选择排序也是一种不稳定的排序算法。

[912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/)
```
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        """
        选择排序 -- Time: O(n2)  Space: O(1)
        实现思路：每一次找到最小的元素，将该元素与nums[i]进行交换
        """
        # 注意循环执行到n-1个元素就可以终止了，最后一个元素有序
        for i in range(len(nums)):
            # minN用来记录最小元素的位置
            minN = i
            for j in range(i+1, len(nums)):
                if nums[minN] > nums[j]:
                    minN = j
            # 将minN位置元素与nums[i]进行交换
            nums[i], nums[minN] = nums[minN], nums[i]
        return nums
```

#### 堆排序(HeapSort)
**算法思想**：堆排序是一种**树形选择排序**方法。特点：将待排序序列视为一棵**完全二叉树**的顺序存储结构，利用完全二叉树中双亲节点和孩子节点
之间的内在关系，再当前无序区中选择关键字最大(或最小)的元素。

**堆排序的2个问题**：  
1、如何由一个无序序列建成一个堆？(如何建堆，通过向上调整方法构成一个堆)  
2、如何输出堆顶元素之后，调整剩余元素成为一个新的堆？(交换堆底元素和堆顶元素，进行向下调整)

**堆的定义**：  
小根堆：L(i)<=L(2i) 且 L(i)<=L(2i+1)  (1<=i<= n//2)  
大根堆：L(i)>=L(2i) 且 L(i)>=L(2i+1)  (1<=i<= n//2)  

#### 堆排序算法：
堆排序算法的关键是**构建初始堆**，对初始序列建堆，是一个反复筛选的过程。其中，n个节点的完全二叉树，最后一个结点是第[n//2]个结点的孩子。  
**调整顺序**：从第[n//2]个结点为根的子树开始筛选，直到根节点为止。  
**调整过程(以大根堆为例)**: 对第[n//2]个结点为根的子树开始筛选，**若根节点的关键字小于左右孩子中关键字较大者，则交换**，使该子树成为
堆。之后依次向前对各结点([n//2]-1 ~ 1)为根的子树进行筛选，看该结点值是否大于其左右结点的值。若不大于，则将左右结点中的较大值与之交换。
**交换后可能会破坏下一级的堆，于是继续采用上述方法构建下一级的堆，直到以该结点为根的子树构成堆为止。**  
**重复以上调整过程，直到根结点。**

#### 时间复杂度和空间复杂度分析：
建堆时间复杂度为：O(n), 之后有n-1次向下调整操作，每次调整的时间复杂度为O(h) -- 其中h代表堆的高度， 因此在最好、最坏和平均情况下，堆
排序的时间复杂度为O(n+nlog2n), 即`O(nlog2n)`。  
空间复杂度为O(1), 仅使用了常数个辅助单元。这是因为堆排序是通过操作原数组使其满足堆的特征，是完全二叉树的数组表示。因此针对堆排序所进行
的操作都是在原数组上进行的，只额外需要常数个辅助单元。

#### 堆的插入和删除操作：
**删除操作**：由于堆顶元素为最大值或者为最小值，**删除堆顶元素时，先将堆的最后一个元素与堆顶元素交换，**此时，由于堆的性质被破坏，需要
对此时的根节点进行**向下调整操作**。  
**插入操作**：先**将新结点放在堆的末端**，再对这个新结点执行**向上调整操作**。

#### 堆的应用：堆经常被用来实现优先级队列，优先级队列常用于操作系统的作业调度和消息队列等

[912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/)
```
# 建堆算法
def maxHeapify(arr, i, heapSize):
    # 将元素k位置通过向下进行调整使其满足大根堆特性
    # 此时序列[i, heapSize-1]中除关键字i外均满足堆的定义
    j, temp = 2*i+1, arr[i]
    while j <= heapSize-1:
        if j < heapSize-1 and arr[j] < arr[j+1]:
            j += 1
        if temp >= arr[j]:
            break
        else:
            arr[i] = arr[j]
            i = j
        # 从0开始，因此下一个根节点为2*j+1
        j = j*2+1
    arr[i] = temp

def buildMaxHeap(arr):
    heapSize = len(arr)
    # 由于数组下标从0开始，因此最后一个父节点为 (heapSize-1)//2, 根节点用0表示
    for i in range((heapSize-1)//2, -1, -1):
        # 每次调整 i 位置，使得父节点值大于左右孩子结点
        maxHeapify(arr, i, heapSize)

def heapSort(arr):
    # 建堆
    buildMaxHeap(arr)
    # 将堆顶元素拿出来与堆中最后一个交换，并对前面heapSize-1个元素调整成为大根堆
    # 只需要进行n-1次调整
    for i in range(len(arr)-1, -1, -1):  
        # 堆顶元素arr[0]与堆中最后一个元素arr[i]交换
        arr[i], arr[0] = arr[0], arr[i]
        maxHeapify(arr, 0, i)

# 堆排序算法
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        heapSort(nums)
        return nums
```

#### 简单版本的堆排序(递归调整以i为父节点的子树，使其满足堆的特性)
```
def heapify(arr, n, i):
    largest = i
    left = 2*i + 1
    right = 2*i + 2
    if left<n and arr[largest]<arr[left]:
        largest = left
    if right<n and arr[largest]<arr[right]:
        largest = right
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)

def heapSort(arr):
    n = len(arr)
    # Build a maxheap
    for i in range((n-2)//2, -1, -1):
        heapify(arr, n, i)

    # pop 堆顶元素并调整
    for i in range(n-1, -1, -1):
        arr[i], arr[0] = arr[0], arr[i]
        heapify(arr, i, 0)
    
    return arr
```