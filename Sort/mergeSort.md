#### 归并排序(MergeSort)

[LC.912](https://leetcode-cn.com/problems/sort-an-array/)

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