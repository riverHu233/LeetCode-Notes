### 排序算法

[LeetCode 912](https://leetcode-cn.com/problems/sort-an-array/)


#### 冒泡排序`(Bubble Sort)`
时间复杂度：`O(n2)`   ;&nbsp;&nbsp; 空间复杂度：`O(1)`  
**思想**：给定待排序列，从前往后（或者从后往前）挨个元素进行比较，如果前面的数比后面的数大，则交换，通过该种
方式，直到序列第一趟比较完成，最终待排序列最大的元素会交换到最后一个位置；待下一趟排序的时候，最后一个位置的元素不参
与排序，经过n-1次排序就能将所有元素都排好序。 由于冒泡排序使用的是交换2个元素的位置，不需要借助额外的数组（
当然在进行元素交换的时候，还是借助了常数个辅助单元），因此空间复杂度为`O(1)`。

**冒泡排序每一趟比较完成都会将一个元素放到其最终的位置上**

```
# 冒泡排序(超出时间限制)
# 时间复杂度：O(n2)  空间复杂度: O(1)
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        for i in range(len(nums)):
            for j in range(len(nums)-i):
                # 一趟交换下来，最大的元素放到最终的位置上，同时不再参与接下来的排序
                if j > 0 and nums[j-1] > nums[j]:
                    nums[j-1], nums[j] = nums[j], nums[j-1]
                    # temp = nums[j]
                    # nums[j] = nums[j-1]
                    # nums[j-1] = temp
        return nums
```


#### 快速排序`(Quick Sort)`
时间复杂度：`最好--O(nlogn)`  `最坏--O(n2)`  `平均--O(nlogn)`   
空间复杂度：`O(nlogn)`

**快速排序算法是对冒泡算法的一种改进，其特性不变--即经过一趟排序，将基准元素送到最终的位置上去**

**思想**：快速排序是在冒泡排序的基础上，基于分治思想，选取一个基准元素pivot，通过一趟排序，使得待排序表
分为独立的2部分，使得前半部分的所有元素均小于基准元素，后半部分的所有元素均大于等于基准元素，从而完成一趟
快速排序将基准元素放置到最终的位置上。随后，递归的对2个子部分进行快速排序，直到每一个划分中只有一个元素
或者为空时，排序结束。


**为什么快速排序的最坏算法时间复杂度为O(n2),其仍然是一种好的算法？**   
1、快速排序算法的运行时间与划分区间是否对称有关，最坏情况下是待排序列有序，所划分的分区一边为n-1,一边为0；
该种情况下才会导致时间复杂度为O(n2); 而在实际情况下，通过选取合适的划分方法可有效避免最坏情况的发生，如取
待排序表头、中间和末尾3个元素取平均作为基准；或者随机从当前表中选择基准元素，从而让最坏情况几乎不会发生。

```
# 快速排序(超出时间限制)--这是由于当数组有序时，快速排序将退化成普通交换排序，时间复杂度为O(n2)
# 时间复杂度(最好)：O(nlogn) 最差O(n2)  空间复杂度：O(logn)

class Solution:
    def sortArray(self, nums):
        def quickSort(nums, low, high):
            if low<high:
                pivotPos = self.partition(nums,low,high)
                quickSort(nums, low, pivotPos-1)
                quickSort(nums, pivotPos+1, high)
        low, high = 0, len(nums)-1
        quickSort(nums, low, high)
        return nums

    def partition(self, nums, low, high):
        '''
        nums: 待排序的序列，注意不是子序列，而是原序列
        low: 待排子序列的起始位置
        high: 待排子序列的终止位置

        return： 返回基准元素最终放置的位置
        '''

        # 设置当前第一个元素作为基准元素，此时nums[low]空缺，
        # 因此先从高到底遍历，找到比pivot小的元素放到pivot位置上
        pivot = nums[low]
        while low < high:
            # !!!注意判断的边界条件，将大于等于基准的元素放在右边，而不是
            # 大于基准的元素放在右边，小于基准的元素放在左边，这样会导致
            # 等于基准的元素陷入死循环
            while low < high and nums[high] >= pivot:
                high -= 1
            # 将比pivot小的元素放到pivot的左边
            nums[low] = nums[high]
            while low < high and nums[low] < pivot:
                low += 1
            # 将比pivot大的元素放到pivot的右边
            nums[high] = nums[low]
        # 将pivot放到最终的位置上
        nums[low] = pivot
        # 返回pivot最终的位置
        return low
```

#### 针对快速排序遇到有序数组退化的问题，可采用随机选取主元pivot来解决该问题
**通过随机选取枢轴元素pivot来避免最坏情况的发生**，主要改动在于partition函数中随机选取pivot，然后将选取元素与`arr[low]`位置元素
进行交换，其余部分保持不变。
```
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        """
        快速排序--Time:O(nlog2n)  Space:O(log2n) -- 通过测试
        """
        import random
        def partition(arr, low, high):
            """
            随机挑选序列中的元素作为pivot
            """
            rnd = random.randint(low, high)
            # 交换rnd和最左侧元素位置
            arr[rnd], arr[low] = arr[low], arr[rnd]
            pivot = arr[low]
            while low<high:
                # 注意要添加小于等于号，否则当出现pivot和比较元素相同时陷入死循环
                while low<high and arr[high]>=pivot:
                    high -= 1
                arr[low] = arr[high]
                while low<high and arr[low]<pivot:
                    low += 1
                arr[high] = arr[low]
            arr[low] = pivot
            return low

        def quickSort(arr, low, high):
            if low<high:
                pivotPos = partition(arr, low, high)
                quickSort(arr, low, pivotPos-1)
                quickSort(arr, pivotPos+1, high)
        
        low, high = 0, len(nums)-1
        quickSort(nums, low, high)
        return nums
```