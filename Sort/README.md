### 排序算法的比较

**按照方法的不同可分为**：  
1、插入排序：[直接插入排序](./InsertSort.md#直接插入排序insertsort)、[折半插入排序](./InsertSort.md#折半插入排序)  
2、交换排序：[冒泡排序BubbleSort](./swapSort.md#冒泡排序bubble-sort)、[快速排序QuickSort](./swapSort.md#快速排序quick-sort)  
3、选择排序：[简单选择排序](./selectSort.md#选择排序selectsort)、[堆排序HeapSort](./selectSort.md#堆排序HeapSort)  
4、[归并排序MergeSort](./mergeSort.md#归并排序mergesort)


**按照时间复杂度和空间复杂度以及稳定性的比较**  

平均时间复杂度为`O(n2)`、空间复杂度为`O(1)`的排序：  
1、插入排序， 冒泡排序， 简单选择排序


平均时间复杂度为`O(nlogn)`的算法：
1、快速排序， 堆排序， 归并排序

从排序的稳定性上来说：归并排序是稳定的排序，而快速排序和堆排序都是不稳定的排序

从空间复杂度上来说： 堆排序`O(1)` >> 快速排序`O(logn)` >> 归并排序`O(n)`



