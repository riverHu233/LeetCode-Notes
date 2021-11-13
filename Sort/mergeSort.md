#### 归并排序(MergeSort)
**思想**：归并排序主要利用了分治的思想，先通过‘分’(divide)的方法将所有的待排序序列划分为一个个自身有序的序列
(1个数本身是有序的)，再通过‘治’(conquer)的方法将已经有序的2个序列归并成一个有序的序列，直到合并为一个长度为n
的有序表为止。

**归并排序的算法时间复杂度最好、最坏和平均时间复杂度都是`O(nlog2n)`, 空间复杂度为`O(n+log2n)=O(n)` 其中包括O(n)的
辅助数组空间和递归调用所占用的O(log2n)的栈空间**

[LC.912](https://leetcode-cn.com/problems/sort-an-array/)
**第一种方法超时**
```
# 归并排序(Merge): 归并是将两个或两个以上的有序表组合成一个新的有序表。
# 假定待排序表含有n个记录，可将其视为n个有序的子表，然后两两归并，直到合并为
# 一个长度为n的有序表为止（2-路归并排序）
# Time: O(NlogN)   Space: O(N)

class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:

        # merge()函数作用是将前后相邻的两个有序表归并为一个有序表，先将两端有序表A, B
        # 复制到辅助数组中，每从辅助数组中取出一个记录进行关键字的比较，将较小者放入A中，
        # 当数组B中有一段下标超出其对应的表长，将另一段的剩余部分直接复制到A中
        def merge(arr, low, mid, high):
            '''
            arr: 待排序列
            low, mid, high 分别代表2个分别有序的子序列的起始位置和终止位置
            arr[low, mid]和arr[mid+1, high]分别有序

            return arr序列，其中arr[low, high]有序
            '''
            # 辅助数组
            temp = arr[::]
            i, j, k = low, mid+1, low

            while i <=mid and j <=high:
                if temp[i] <= temp[j]:
                    arr[k] = temp[i]
                    k += 1
                    i += 1
                else:
                    arr[k] = temp[j]
                    k += 1
                    j += 1
            # 情况1和情况2只会执行一个
            while i <= mid:  # 情况1：第一个有序表未检测完，复制
                arr[k] = temp[i]
                i += 1
                k += 1
            while j <= high:  # 情况2：第二个有序表未检测完，复制
                arr[k] = temp[j]
                j += 1
                k += 1

        # 递归二分排序
        def mergeSort(arr, low, high):
            if low<high:
                mid = (low+high)//2
                mergeSort(arr, low, mid)
                mergeSort(arr, mid+1, high)
                merge(arr, low, mid, high)
        
        mergeSort(nums, 0, len(nums)-1)
        return nums
```

**可能是上面方法通过每次给arr数组赋值的方法速度较慢，因此采用temp临时数组将排序的数组存储起来，待排序完成后直接赋值给arr数组，该
方法可通过**
```
# 针对归并排序进行改进，采用空数组来存储有序元素，待排序完成后再将数组复制给arr
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        """
        归并排序改进
        """
        def merge(arr, low, mid, high):
            temp = []
            i, j = low, mid+1
            while i<=mid and j<=high:
                if arr[i] < arr[j]:
                    temp.append(arr[i])
                    i += 1
                else:
                    temp.append(arr[j])
                    j += 1
            while i<=mid:
                temp.append(arr[i])
                i += 1
            while j<=high:
                temp.append(arr[j])
                j += 1
            arr[low:high+1] = temp
        
        def mergeSort(arr, low, high):
            if low<high:
                mid = (low + high) // 2
                mergeSort(arr, low, mid)
                mergeSort(arr, mid+1, high)
                merge(arr, low, mid, high)

        mergeSort(nums, 0, len(nums)-1)
        return nums
```