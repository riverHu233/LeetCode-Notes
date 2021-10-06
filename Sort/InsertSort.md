#### 直接插入排序(InsertSort)
**思想**：在排序过程中，将待排序的元素插入到已经有序的子序列中，经过n-1迭代，得到最终的有序序列；
插入排序不需要借助额外的空间。

```
def InsertSort(arr):
    n = len(arr)
    for i in range(1, n):
        if arr[i] < arr[i-1]:
            # 哨兵暂存待插入元素
            temp = arr[i]
            j = 0
            # 遍历有序子序列，找到合适插入位置
            for j in range(0, i)[::-1]:
                if arr[j] < temp:
                    break
                # 整体元素往后移动
                arr[j+1] = arr[j]
            arr[j+1] = temp
    return arr
```

#### 折半插入排序
**思想**：折半插入排序与直接插入排序相比，在查找插入位置的时候使用折半查找方法，更快找到插入的位置，
并将元素插入到有序子序列中，经过n-1迭代，得到最终的有序序列
```
def BinaryInsertSort(arr):
    n = len(arr)
    for i in range(1, n):
        temp = arr[i]
        low, high = 0, i-1
        # 通过二分查找来找到插入位置
        while low <= high:
            mid = low + (high - low)//2
            if arr[mid] <= arr[i]:
                low = mid + 1
            elif arr[mid] > arr[i]:
                high = mid - 1
        if low < i:
            for j in range(low, i)[::-1]:
                arr[j+1] = arr[j]
            arr[low] = temp
    return arr
```