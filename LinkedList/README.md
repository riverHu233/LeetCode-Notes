### 链表

[LeetBook链表](https://leetcode-cn.com/leetbook/read/linked-list/jy291/)

设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性：val 和 next。
val 是当前节点的值，next 是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个
属性 prev 以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。
在链表类中实现这些功能：

get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。  
addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。  
addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。  
addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val 的节点。如果 index 等于链表的长度，
则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。  
deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。

```
class LinkedNode:
    def __init__(self, val):
        self.val = val
        self.next = None

class MyLinkedList:

    def __init__(self):
        self.pHead = None

    def get(self, index: int) -> int:
        if index<0 or self.pHead == None:
            return -1
        else:
            idx = 0
            cur = self.pHead
            while cur.next and idx <index:   # 遍历cur直到链表结束或者找到index位置
                idx += 1
                cur = cur.next
            if idx == index:
                return cur.val
            else:
                return -1

    def addAtHead(self, val: int) -> None:
        newNode = LinkedNode(val=val)
        newNode.next = self.pHead
        self.pHead = newNode

    def addAtTail(self, val: int) -> None:
        newNode = LinkedNode(val=val)
        if self.pHead is None:
            self.pHead = newNode
        else:
            cur = self.pHead
            while cur.next is not None:
                cur = cur.next
            cur.next = newNode

    def addAtIndex(self, index: int, val: int) -> None:
        # 情况1：index小于等于0或者链表为空，头部插入；
        # 情况2：index找到合适插入位置；
        # 情况3：index长度等于链表长度，在尾部插入；
        # 情况4：index长度大于链表长度，不插入；

        if index <= 0 or self.pHead == None:  # 情况1
            self.addAtHead(val)
        else:
            idx = 0
            cur = self.pHead
            while cur.next is not None and idx < index-1:
                idx += 1
                cur = cur.next
            if idx == index-1:  # 情况2
                node = LinkedNode(val)
                node.next = cur.next
                cur.next = node
            if idx == index:  # 情况3
                self.addAtTail(val)

    def deleteAtIndex(self, index: int) -> None:
        # 情况1：如果index小于0或链表为空，则无效
        # 情况2：如果index等于0，则需要替换头节点
        # 情况3：如果index有效，则删除第index个节点，

        if self.pHead:
            if index == 0:
                # delete from head
                temp = self.pHead
                self.pHead = temp.next
            elif index > 0:
                idx = 0
                prev = self.pHead
                # find the previous link node of index-th node
                while prev.next is not None and idx < index-1:
                    prev = prev.next
                    idx += 1
                if idx == index-1:
                    cur = prev.next
                    if cur is not None:
                        prev.next = cur.next
```

####[判断一个链表是否有环LeetCode.141](https://leetcode-cn.com/problems/linked-list-cycle/)
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

# 思路1：快慢指针--慢指针每移动一步，快指针移动两步，如果链表有环的话，
# 最终快指针会追上慢指针一圈;
# 初始化：快慢指针都在头节点，不同的是快指针移动两步，慢指针移动一步
# 时间复杂度：O(N)  空间复杂度：O(1)
# 时间：20mins
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        # 当链表为空或者链表的下一个节点为空，则不存在环
        if not head or not head.next:
            return False
        fast, lowest = head, head
        # 由于快指针移动较块，因此如果快指针无法继续，则链表不存在环
        while fast.next is not None:
            lowest = lowest.next
            fast = fast.next
            if fast.next is not None:
                fast = fast.next
            else:
                return False
            if lowest == fast:
                return True
        else:
            return False
# 疑问：为什么题解解析中判断语句 "while fast and fast.next:" 也可用来执行表达式 fast = fast.next.next
# 答：fast.next and fast.next.next 即保证 fast.next.next不为None; 而fast and fast.next 则只保证 fast = fast.next.next
表达式可执行，并不保证 fast.next.next不为None, 相应的判断则等到下一次循环
```