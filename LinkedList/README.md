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
**判断链表是否有环，主要有2种思路：**
+ 采用哈希表来记录访问过的结点，如果存在重复访问的节点，则有环
+ 采用快慢指针，快指针的速度是慢指针的2倍，如果快指针最终与慢指针相遇，则有环
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

####[判断一个链表是否有环II LeetCode.142](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
+ 思路1：使用哈希表来记录访问过的节点，一旦链表有环，返回重复访问的该节点  Time: O(N)  Space: O(N)
+ 思路2：使用快慢指针  Time: O(N)  Space: O(1)
```
# 思路1：使用哈希表存储节点，一旦节点出现2次，则说明该节点为入环的第一个节点
# 时间复杂度：O(N)，空间复杂度：O(N)
# 时间：15mins

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        nodeSets = set()
        while head:
            if head in nodeSets:
                return head
            nodeSets.add(head)
            head = head.next
        return None

# 思路2：快慢指针--快指针移动2步，慢指针移动1步；如果有环，则必相遇
# 问题：假设链表到入口长度为a, 环的长度为b，则当快慢指针相遇时，
# 快指针走的长度为慢指针的2倍： f = 2s
# 快指针走的长度为慢指针的长度加上n圈的环：f = s + nb
# 解等式有： s = nb
# 由于从链表到入口的长度为a, 因此 a + nb 即链表的入口(即慢指针走进环 n 圈并在入口停下)
# 因此，可以理解为当快慢指针相遇时，慢指针再走 a 步即可到达入口
# 时间复杂度：O(N)  空间复杂度：O(1)
# 时间：50mins
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        fast, slow = head, head
        while fast:
            if not fast.next:
                return None
            fast = fast.next.next
            slow = slow.next
            if fast == slow:  # 相遇
                # 新指针从链表头出发a个位置，与slow指针相遇
                ptr = head
                while True:
                    # 先判断是否相等，再执行移动，如果链表本身为环的话，则相遇位置就是链表入口
                    if ptr == slow:
                        return slow
                    ptr = ptr.next
                    slow = slow.next
        return None
```

[LC.234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        """
        思路：遍历链表, 然后将链表中的所有值按顺序存储，然后直接判断
        是否与逆序元素相等，即可
        Time: O(N)  Space: O(N)
        """
        res, cur = [], head
        while cur:
            res.append(cur.val)
            cur = cur.next
        return res == res[::-1]

class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        """
        思路二：Step1: 复制一个新的链表  Step2: 将新链表逆序  
        Step3: 将新链表和原链表同时遍历，若不相同则返回False
        Time: O(3N)  Space：O(N)
        """
        # 复制链表
        def copyList(head):
            cur = head
            newList, tail = None, None
            while cur:
                # 如果是第一个结点
                if newList is None:
                    # newList指向头节点
                    newList = ListNode(cur.val)
                    # tail指向尾节点，方便使用尾插法插入
                    tail = newList
                else:
                    # 尾插法插入节点
                    tail.next = ListNode(cur.val)
                    tail = tail.next
                cur = cur.next
            return newList
        
        newList = copyList(head)
        # 链表逆序
        def reversedList(head):
            prev, cur = None, head
            while cur:
                temp = cur.next
                cur.next = prev
                prev = cur
                cur = temp
            return prev
        rev = reversedList(newList)

        def printList(head):
            ptr = head
            while ptr:
                print(ptr.val, end=' —> ')
                ptr = ptr.next

        # 同时遍历2个链表，判断是否完全相同
        while rev and head:
            if rev.val != head.val:
                return False
            rev, head = rev.next, head.next
        return True

# Solution 3: 双指针 + 中点 + 翻转链表
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        """
        思路：Step1：使用双指针找到链表的中点；
        Step2: 对链表后半部分进行逆序
        Step3: 同时从起点和中点开始遍历，判断是否相同
        Time: O(N)   Space: O(1)
        """
        # 链表逆序
        def reversedList(head):
            prev, cur = None, head
            while cur:
                temp = cur.next
                cur.next = prev
                prev = cur
                cur = temp
            return prev

        # 双指针找到中点位置
        slow, fast = head, head
        while fast.next and fast.next.next:
            slow = slow.next
            fast = fast.next.next

        # 对后半部分进行逆序
        rev = reversedList(slow)
        # rev的长度即为遍历的长度
        while head and rev:
            if head.val != rev.val:
                return False
            head, rev = head.next, rev.next
        return True
```


#### 合并2个有序链表
##### 思路1：递归法
[递归思路参考](https://leetcode-cn.com/problems/merge-two-sorted-lists/solution/yi-kan-jiu-hui-yi-xie-jiu-fei-xiang-jie-di-gui-by-/)

##### 递归的规律
+ 递归函数必须要有终止条件，否则会出错；
+ 递归函数先不断调用自身，直到遇到终止条件后进行回溯，最终返回答案。

##### 本题递归解法思路：

+ 终止条件：当两个链表都为空时，表示我们对链表已合并完成。
+ 如何递归：我们判断 l1 和 l2 头结点哪个更小，然后较小结点的 next 指针指向其余结点的合并结果。（调用递归）

##### 思路2：迭代法
+ 创建dummy节点，作为结果链表的开头。
+ 创建指向结果列表的指针cur。该指针一直指向结果链表的最后一个节点；**初始状态中，cur指针指向dummy节点**。随着结果链表的增加而不断向后移动，始终保持指向**结果链表**的最后一个节点。
+ 循环遍历2个有序链表。list1, list2
+ 拼接未遍历完的元素。当其中一个链表遍历完成后，若存在另一链表不为空，则将cur的next指针指向该链表
+ 返回结果链表的头节点--dummy节点的下一个节点(dummy.next)。

```
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        # 递归解法
        # 递归的终止条件--当list1和list2都遍历结束时
        # Time: O(m+n)  Space: O(m+n) 
        if not list1:  # list1为空时，返回list2(若list2也为空，则返回None)
            return list2
        if not list2:
            return list1
        
        if list1.val <= list2.val:
            list1.next = self.mergeTwoLists(list1.next, list2)
            return list1
        else:
            list2.next = self.mergeTwoLists(list1, list2.next)
            return list2

# 迭代法求解 -- 利用dummy节点，最后返回dummy节点的next即可
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        # 迭代法求解，创建dummy节点，作为结果链表的头，同时创建cur指针指向结果链表，
        # 并保证cur指针一直指向结果链表的最后一个节点
        # Time: O(m+n)  Space: O(m+n)
        dummy = ListNode(val=0)
        cur = dummy
        while list1 and list2:
            if list1.val <= list2.val:
                cur.next = list1
                list1 = list1.next
            else:
                cur.next = list2
                list2 = list2.next
            cur = cur.next
        
        cur.next = list1 if list1 else list2

        return dummy.next
```