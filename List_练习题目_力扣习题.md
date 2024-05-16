#### 707 构建链表
https://leetcode.cn/problems/design-linked-list/description/

以下是我自己的答案


```python
class Node():
    def __init__(self,val):
        self.val = val
        self.next = None

class MyLinkedList(object):

    def __init__(self):
        self.head = Node(-1)
        self.len = 0

    def get(self, index):
        """
        :type index: int
        :rtype: int
        """
        if index == 0 :
            return self.head.val
        if index > self.len:
            return
        cur = self.head
        count = 0
        while cur and count < index:
            count = count + 1
            cur = cur.next
        return cur.val


    def addAtHead(self, val):
        """
        :type val: int
        :rtype: None
        """
        if self.head.val == -1:
            self.head.val = val
        else:
            node = Node(val)
            node.next = self.head
            self.head = node
            self.len += 1

    def addAtTail(self, val):
        """
        :type val: int
        :rtype: None
        """
        if self.head.val == -1:
            self.head.val = val
        else:    
            cur = self.head
            while cur.next != None:
                cur = cur.next
            cur.next = Node(val)
            self.len += 1


    def addAtIndex(self, index, val):
        """
        :type index: int
        :type val: int
        :rtype: None
        """
        if index > self.len:
            return 
        count = 0
        cur = self.head
        if index == 0 :
            self.addAtHead(val)
        if index >= 1:
            while cur and count <index-1:
                count += 1
                cur = cur.next
            node = Node(val)
            node.next = cur.next
            cur.next = node   
            self.len += 1 
        
        


    def deleteAtIndex(self, index):
        """
        :type index: int
        :rtype: None
        """
        if index > self.len:
            return 
        count = 0
        cur = self.head
        if index == 0 :
            self.head = self.head.next
        if index >= 1:
            while cur and count <index-1:
                count += 1
                cur = cur.next
            cur.next = cur.next.next
            self.len = self.len - 1 
    
    def show(self):
        cur = self.head
        count = 0
        while cur:
            print(self.get(count))
            count += 1
            cur = cur.next
        



# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```


```python
a = MyLinkedList()
```


```python
a.addAtHead(4)
a.show()
```

    4
    


```python
print(a.get(1))
```

    None
    

#### 206  反转链表
https://leetcode.cn/problems/reverse-linked-list/description/


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def reverseList(self, head):
        if head == None:
            return head
        cur = head.next
        reversed_list = ListNode(head.val)
        while cur:
            node = ListNode(cur.val)
            node.next = reversed_list
            reversed_list = node
            cur = cur.next
        return reversed_list
            

        
```

官方题解 java 转 python 版本  

这个版本更好一点的原因是无需重新建立 ListNode 并采用头插法来反转




```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        prev = None
        curr = head
        while curr is not None:
            next_node = curr.next
            curr.next = prev
            prev = curr
            curr = next_node
        return prev
    
# 记住 当前是谁，上一个是谁，下一个是谁    
# 但是这段代码会改变 head
'''    
    prev = None
    cur = head 
    while cur:
        next_node = cur.next
        cur.next = prev
        prev = cur
        cur = cur.next 
'''
```

#### 203  移除链表元素
https://leetcode.cn/problems/remove-linked-list-elements/description/

以下是个人题解


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def removeElements(self, head, val):
        """
        :type head: ListNode
        :type val: int
        :rtype: ListNode
        """
        prev = None
        cur = head 
        while head and head.val == val:
            head = head.next
        while cur:
            if prev and cur.val == val :
                prev.next = cur.next
                cur = cur.next
                continue
            prev = cur
            cur = cur.next
        return head 
        
```

以下是通义千问修改个人题解之前一个错误版本的解法


```python
def removeElements(self, head, val):
    """
    :type head: ListNode
    :type val: int
    :rtype: ListNode
    """
    # 处理头节点可能为val的情况
    while head and head.val == val:
        head = head.next
    
    prev = head
    cur = head
    while cur:
        if cur.val == val:
            prev.next = cur.next
        else:
            prev = cur
        cur = cur.next
    
    return head
```

#### 328 奇偶链表

https://leetcode.cn/problems/odd-even-linked-list/description/


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def oddEvenList(self, head):
        if not head or not head.next:
            return head
        head_odd = head 
        tail_odd = head.next.next
        if tail_odd == None:
            return head
        head_even = head.next
        if tail_odd.next == None:
            head_odd.next = tail_odd
            tail_odd.next = head_even
            head_even.next = None
            return head_odd
        else: 
            tail_even = tail_odd.next
        cur = tail_even.next
        count = 0
        head_odd.next = tail_odd
        head_even.next = tail_even
    
        while cur:
            count += 1
            if count % 2 == 1:
                tail_odd.next = cur
                tail_odd = cur
            else:
                tail_even.next = cur
                tail_even = cur
            cur = cur.next
        tail_even.next = None
        tail_odd.next = head_even
        return head_odd

```

官方题解


```python
class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        if not head:
            return head
        
        evenHead = head.next
        odd, even = head, evenHead
        while even and even.next:
            odd.next = even.next
            odd = odd.next
            even.next = odd.next
            even = even.next
        odd.next = evenHead
        return head


```

自己尝试写一下


```python
def oddEvenList(self, head):
    if not head:
        return head
    head_odd = head
    head_even = head.next
    
    odd = head_odd
    even = head_even
    while even and even.next
#   while odd and even: 是完全错误的写法，它忽略了 even.next为空时执行下方语句中的
#       even = odd.next 会出问题，因为此时 odd = None ，无法赋值 odd.next 给 even
        odd.next = even.next
        odd = even.next
        even.next = odd.next
        even = odd.next
    odd.next = head_even
    return head_odd
        
        
```

#### 234 回文链表
https://leetcode.cn/problems/palindrome-linked-list/description/

个人解法套用了之前的反转链表的函数，但是官方题解中的反转函数会改变链表自身，所以用了我自己写的反转链表函数，但是这样导致运行时间过长了。


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def reverseList(self, head):
        if head == None:
            return head
        cur = head.next
        reversed_list = ListNode(head.val)
        while cur:
            node = ListNode(cur.val)
            node.next = reversed_list
            reversed_list = node
            cur = cur.next
        return reversed_list

    def isPalindrome(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        if not head.next:
            return head
        rev = self.reverseList(head)
        revv = rev
        cur = head
        while cur and cur.val == revv.val:
            cur = cur.next
            revv = revv.next
        return True if cur == None else False

```

以下是通义千问给的建议，slow 和 fast 两个指针一个快一个慢，能够找到链表中间结点。它主要快在这里，时间和O（n）同阶，但是大概节省一半。


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def reverseList(self, head):
        prev = None
        curr = head
        while curr is not None:
            next_node = curr.next
            curr.next = prev
            prev = curr
            curr = next_node
        return prev

    def isPalindrome(self, head):
        #这一步定义了 slow 和 fast 两个指针，一个走两格一个走一格
        #这就导致当结点个数为 奇数时，fast = None， slow = center（中间节点）
        #        当结点个数为 偶数时，fast = Rear（最后一个结点） ， slow = 第（结点数/2） 个结点 
        #注意 while 的判断条件 fast或者fast为空
        
        slow = head 
        fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            
        # 从 slow 开始的链表就是 head 的后半部分链表
        # 只需比较 slow 链表反转过后 和 head 链表 的前面部分是否相等即可
        # 即使 反转链表会改变 slow 本身也不用在意
        
        second_half_head = self.reverseList(slow)
        
        # 但是个人觉得还是不要改变的好
        # 以下是比较的部分，这一部分就没有什么好说的了，用自己写的判断方法和他写的判断方法都可以
        first_half_iter = head
        second_half_iter = second_half_head
        while second_half_iter:
            if first_half_iter.val != second_half_iter.val:
                return False
            first_half_iter = first_half_iter.next
            second_half_iter = second_half_iter.next
        
        return True
```

#### 138 随机链表的复制
https://leetcode.cn/problems/copy-list-with-random-pointer/description/

以下是自己写的代码，虽然一如既往的耗费时间


```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x, next=None, random=None):
        self.val = int(x)
        self.next = next
        self.random = random
"""

class Solution(object):
    def copyRandomList(self, head):
        """
        :type head: Node
        :rtype: Node
        """
        # 对简单情况的处理
        if not head:
            return None
        if not head.next:
            node = Node(head.val, head.next)
            # 如果这里直接创建 node = Node(head.val, head.next, head.random)
            # 会造成 node.random = head.random 指向原链表的错误情况
            if head.random == None:
                node.random = None
            else:
                node.random = node
            return node
        # 先进行浅层拷贝
        cur = head.next
        copy = Node(head.val)
        cur_copy = copy
        while cur:
            cur_copy.next = Node(cur.val)
            cur_copy = cur_copy.next
            cur = cur.next
        # 再进行深层拷贝
        cur = head 
        cur_copy = copy
        while cur:
            ptr_copy = copy
            random = cur.random
            ptr = head
            if random == None:
                cur_copy.random = None
                cur = cur.next
                cur_copy = cur_copy.next
                continue
            while ptr:
                if random.val == ptr.val and random.next == ptr.next:
                    cur_copy.random = ptr_copy
                    break
                ptr = ptr.next
                ptr_copy = ptr_copy.next
            cur = cur.next
            cur_copy = cur_copy.next
        return copy

```

下面是根据题解的C++代码写出的python递归算法，可以说是很经典的递归算法应用了，没有想到有点可惜。


```python
class Solution:
    def __init__(self):
        self.cached_node = {}

    def copyRandomList(self, head):
        if head is None:
            return None
        if head not in self.cached_node:
            node_new = Node(head.val)
            self.cached_node[head] = node_new
            node_new.next = self.copyRandomList(head.next)
            node_new.random = self.copyRandomList(head.random)
        return self.cached_node[head]
```

#### 141 环形链表
https://leetcode.cn/problems/linked-list-cycle/description/

采用 Floyd 判圈法

这种方法类似于在操场跑道跑步。两个人从同一位置同时出发，如果跑道有环（环形跑道），那么快的一方总能追上慢的一方。

基于上边的想法，Floyd 用两个指针，一个慢指针（龟）每次前进一步，快指针（兔）指针每次前进两步（两步或多步效果是等价的）。如果两个指针在链表头节点以外的某一节点相遇（即相等）了，那么说明链表有环，否则，如果（快指针）到达了某个没有后继指针的节点时，那么说明没环。


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        if not head or not head.next:
            # 最开始这里写成了
            # return head
            # 导致在 只有一个头结点 时总是返回 head，并且非空
            # 但是非空的头结点会被判定成Ture
            # 被惯性思维坑到了，习惯性地返回了 head
            return False
        
        slow = head
        fast = head.next
        while fast and fast.next:
            if fast == slow:
                return True
            slow = slow.next
            fast = fast.next.next

        return False
```

看了一下耗时最短的实现方法，比较厉害，有些计算在里面。没有新建数组保存已遍历结点的操作，只有不断地在链表中走。先展示代码然后解释一下。


```python
class Solution(object):
    def detectCycle(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        slow = head
        fast = head
        # 首先还是快慢指针一直向前，要么到头，要么快慢指针相遇
        while fast != None and fast.next != None:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                break
        # 如果快指针到头了，那么说明链表无环
        if fast == None or fast.next == None:
            return None
        # 如果快慢指针相遇，接下来开始找环的起点。
        # 新建一个指针从头开始，和 slow 指针一起，一步一步地走
        p = head
        while p != slow:
            p = p.next
            slow = slow.next
        return p
```

定义变量
1. $L$  从链表头结点到环起点的距离。
2. $C$  环的长度。
3. $X$  从环的起点到 slow 和 fast 相遇的点的距离。/ slow 在环中走的距离
当快慢指针相遇时，慢指针走的距离为 $S$，快指针走的距离为 $2S$。则快指针比慢指针多走了环的整数倍，即 $2S = S+nC$，$S = nC$。则 $L+X = nC$。

接下来新建一个指针 p 从 head 开始和 slow 一起向前推进。则当 p 走过距离 $L$ 到环的起点的时候，slow 在环内行进的距离正好为 $X+L = nC$ 是环的整数倍，于是 $slow$ 正好也回到起点。于是当 p == slow 时，p 所处的位置就是环的起点。

#### 19 删除链表的倒数第n个结点
https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/


```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        #本来这里写的是
        # dummy_head = ListNode(-1)
        # dummy_head.next = head
        # 但是看了执行最快的写法
        # 他直接在生成哑结点的时候就把 next 指向 head了
        # 算是一种小 trick
        dummy_head = ListNode(next = head)
        slow = dummy_head
        fast = dummy_head
        # 保证 fast 始终在 slow 前面第 n 个位置上
        while n:
            fast = fast.next
            n -= 1
        # 当 fast 在倒数第 1 个结点的时候
        #    slow 在倒数第 n+1 个结点上
        while fast.next:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next
        return dummy_head.next
        
```
