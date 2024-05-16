```python
class ListNode():
    def __init__(self,val,next = None,order = 0):
        self.val = val
        self.next = next
        
```

### 单链表冒泡排序


```python
def bubbleSort(self, head):
    node_i = head
    tail = None
    
    while node_i:
        # 这里的 node_i 只是起到表示循环次数的作用，毕竟有多少 node while就会执行多少次
        node_j = head
        swapped = 0
        while node_j and node_j.next != tail :
            if node_j.val > node_j.next.val :
                # 关于下面这个语句的执行过程可以见下面的解释
                node_j.val, node_j.next.val = node_j.next.val, node_j.val
                # 这一行代码用来改变 swapped ，swapped 标示交换的次数，若交换的次数为0，则说明链表有序
                swapped += 1
            node_j = node_j.next
        # 把最大的数字移动到最后一个后，对起标记为 tail
        # 并在每次循环后更新 tail，tail及其之后的部分都已经有序
        if swapped == 0：
            return head
        tail = node_j
        node_i = node_i.next
        
    return head
        
# 放在力扣中测试了一下，确实是可以用的，但是被一个大链表排序搞得超时了，提交没有通过。        
```

在Python中，当执行 
node_j.val, node_j.next.val = node_j.next.val, node_j.val
时，Python实际上会先计算等号右边的所有表达式的值，然后再根据从左至右的顺序进行赋值。这个过程可以看作是使用了一个临时元组来暂存这两个值，然后再解包赋值，因此不会出现某个值“消失”的情况。

具体步骤如下：

1. 计算 node_j.next.val，得到值 A。
2. 计算 node_j.val，得到值 B。
3. 创建一个临时元组 (A, B)。
4. 按照从左到右的顺序，从元组中取出值进行赋值：node_j.val 被赋值为 A，node_j.next.val 被赋值为 B。

所以，尽管看起来像是直接交换，但实际上Python保证了两个值都得到了妥善处理，没有丢失的风险。这是Python中一种常见的值交换技巧，利用了元组解包的特性。

### 单链表选择排序


```python
def sectionSort(self, head):
    node_i = head
    # 接下来将 第 i 各位置的结点换成第 i 小的值
    while node_i and node_i.next:
        # 第 i 轮的最小值
        min_node = node_i
        node_j = node_i.next
        while node_j:
            if min_node.val > node_j.val:
                # 在这里，应该让 min_node 指向 node_j 而不是交换二者的值
                min_node = node_j
            node_j = node_j.next
        # 如果不相等就交换值，相等当然就不变了。虽然感觉直接换也没有问题，但是不换可能更稳定
        # 稳定指的是相同的值在排序之后顺序保持不变
        if node_i != min_node:
            min_node.val, node_i.val = node_i.val, min_node.val
        node_i = node_i.next
        
    return head
```

### 单链表插入排序
引入哑结点的操作值得学习，如果不引入哑结点就要在第一次先找到 min_node 放在表头，然后再从表头开始，但是引入哑结点把整个操作统一为插入排序。


```python
def insertionSort(self, head):
    if not head or not head.next:
        return head
    # 哑结点。为了prev能直接启动插入排序
    dummy_head = ListNode(-1)
    dummy_head.next = head
    rear = head
    cur = head.next
    # cur 始终在 listrear 的后面一个，为了决定把他插入在前面列表还是挂在 listrear 后面，需要进行如下操作
    while cur:
        # 若 cur 大于等于 listrear
        if cur.val >= rear.val:
            rear = rear.next
        else:  
            prev = dummy_head
            while cur.val >= prev.next.val:
                prev = prev.next
            rear.next = cur.next
            cur.next = prev.next
            prev.next = cur
            # 此时 cur已经被插入到链表当中，为了保证 cur 始终是 rear 后面的一个，应当执行
        cur = rear.next
    return dummy_head.next

```

### 单链表归并排序
这是一个递归算法，并且实现这个算法要写两个个函数分别处理结点和链表
，直接抄一遍，对其中不理解的地方做一下注释。
归并排序是将链表分割为两个数组，调用自身函数将每个链表排序有序状态，之后再将调用辅助函数两个数组合并为一个有序数组。
递归终止条件就是当前链表只有一个结点或链表为空。

归并排序算法的辅助函数传入的两个参数代表的是左右两个链表


```python
# 对于传入的两个链表，将其排序为一个链表
def merge(self, left, right):
    #哑结点，用来当头结点的前位
    dummy_head = ListNode(-1)
    # 当且位置，其是链表的尾部，用来连接接下来要插入的结点
    cur = dummy_head
    while left and right:
        if left.val <= right.val:
            cur.next = left
            left = left.next
        else:
            cur.next = right
            right = right.next
        cur = cur.next
    #接下来 left 和 right 里只有一个不为空
    #此时直接把这个不空的链表接到尾部即可
    if left:
        cur.next = left
    elif:
        cur.next = right
    # 为什么没有考虑两者都为空的情况呢
    # 从 while 的条件可以看出来，只要有一个空的话就停止 while 循环了
    
    return dummy_head.next

# ok，这是真正的排序算法，它需要调动上面那个函数
def mergeSort(self, head):
    if not head or not head.next:
        return head
    
    slow, fast = head,head.next
    # 熟悉的快慢指针操作
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
    #断开链表
    left_head ,right_head = head, slow.next
    slow.next = None
    
    #归并操作，也就是递归的链表合体操作
    return self.merge(self.mergeSort(left_head),self.mergeSort(right_head))
```

### 单链表快速排序
这也是一个迭代算法，跟归并排序有点像，但是又有那么一些差别。快速排序通过调整基准值 povit 的位置，来递归排序。确实有点难理解，发明的人有点东西。

辅助函数 partition（left，right） 输入的是一个完整链表的头结点和尾结点，并且用left.val  当基准值，将其固定到其顺序位置上，但是对左右两边的顺序不管。但是跟归并排序一样，在递归到最小单元并且处理完低级单元剩下的都是有序单元。因此左右的顺序也是确定的。只不过相同值的结点的顺序也可能改变，因为基准结点在最后在调换，但是如果有其他相同值的结点，可能在调换中改变顺序。


```python
# 传入链表的一段，其中 left 代表头结点  right 代表尾结点
def partition(self, left,right):
    if left == right or left.next == right:
        return left
    # 基准值为头结点的值
    pivot = left.val
    # less代表基准值左边的最后一个， cur 代表遍历列表
    less = left
    cur = left.next
    
    while cur! = right:
        # 当前值小于基准值时
        if cur.val < pivot:
            # less 部分向前移位
            less = less.next 
            # 移位之后的值不知道是多少，但是无所谓，和 cur 交换
            less.val, cur.val = cur.val, less.val
        # 继续遍历
        cur = cur.next
    # 遍历结束时，less 的值小于等于 left
    # 且 less 之前结点的值除了 left都小于 left.val
    # less 之后结点的值都大于等于 left.val
    # 所以 less所处的位置正应该是 left.val 的位置
    less.val, left.val = left.val, less.val
    return less 

def quickSort(self,left,right):
    if left == right or left.next == right:
        return left
    # 将基准值的位置找到，并且左边都是小的，右边都是大的
    pi = self.partition(left, right)
    # 继续排序，在每个迭代的过程中都会把基准值的位置找到
    self.quickSort(left,pi)
    self.quickSort(pi.next.right)
    return left
```

### 单链表计数排序
计数排序算法类似于积分的想法，确定链表元素的范围，然后建立一个跟范围一样大的数组。数组按顺序被标记，然后每个 index 对应的是所有值为 index 的结点构成的链表。最后从前往后合并链表即可。


```python
def countingSort(self,head):
    if not head or not head.next:
        return head
    list_min,list_max = float('inf'),float('-inf')
    cur = head
    while cur:
        if cur.val <list_min:
            list_min = cur.val
        elif cur.val >list_max:
            list_max = cur.val
        cur = cur.next
    
    size = list_max-list_min+1
    counts = [0 for i in range(size)]
    
    cur = head
    while cur:
        # 对值为 cur.val 的
        # counts[cur.val - list_min] 计数加一
        counts[cur.val - list_min] += cur.c1
        cur = cur.next
        
    dummy_head = ListNode(-1)
    cur = dummy_head
    # 相当于是重新创建了结点
    for i in range(size):
        while counts[i]:
            cur.next = ListNode(i+list_min)
            counts[i] -=1
            cur = cur.next
    return dummy_head.next
```

### 单链表桶排序 
桶排序（Bucket Sort）是一种基于分布的排序算法，特别适用于均匀分布的数据。它将数组元素分布到多个桶中，然后分别对每个桶内的元素进行排序，最后合并所有桶中的元素得到排序后的结果。桶排序在最理想情况下可以达到线性时间复杂度 O(n)。

工作原理
1. 创建桶：根据数据的范围创建一定数量的桶（数组或列表）。
2. 分配元素：将输入数据分配到相应的桶中。
3. 桶内排序：对每个桶中的元素进行排序（可以使用其他排序算法，例如插入排序或快速排序）。
4. 合并桶：将所有桶中的元素按序合并成一个有序序列。


```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    
    # 将链表节点值 val 添加到对应桶 buckets[index] 中
    def insertion(self, buckets, index, val):
        # 若当前桶空，则建立初始结点
        if not buckets[index]:
            buckets[index] = ListNode(val)
            return
        # 对于非空的桶，先生成一个结点
        node = ListNode(val)
        # 结点指向桶当前的头结点，其是就是把 node 放在桶最前面
        node.next = buckets[index]
        # 这时候桶的头结点就是新的结点 node
        buckets[index] = node
        
    # 归并环节
    # 以下就是归并排序重写了一下
    # 桶排序中会用到
    def merge(self, left, right):
        dummy_head = ListNode(-1)
        cur = dummy_head
        while left and right:
            if left.val <= right.val:
                cur.next = left
                left = left.next
            else:
                cur.next = right
                right = right.next
            cur = cur.next
            
        if left:
            cur.next = left
        elif right:
            cur.next = right
            
        return dummy_head.next

    def mergeSort(self, head: ListNode):
        # 分割环节
        if not head or not head.next:
            return head
        
        # 快慢指针找到中心链节点
        slow, fast = head, head.next
        while fast and fast.next:
            slow = slow.next 
            fast = fast.next.next 
            
        # 断开左右链节点
        left_head, right_head = head, slow.next 
        slow.next = None
        
        # 归并操作
        return self.merge(self.mergeSort(left_head), self.mergeSort(right_head))        
    
    def bucketSort(self, head, bucket_size=5):
        if not head:
            return head
        
        # 找出链表中最大值 list_max 和最小值 list_min
        list_min, list_max = float('inf'), float('-inf')
        cur = head
        while cur:
            if cur.val < list_min:
                list_min = cur.val
            if cur.val > list_max:
                list_max = cur.val
            cur = cur.next
            
        # 计算桶的个数，并定义桶
        # 除以桶的大小，取整再加一
        bucket_count = (list_max - list_min) // bucket_size + 1
        # 其是自己没想过可以用数组来构成一个桶堆
        buckets = [[] for _ in range(bucket_count)]
        
        # 将链表节点值依次添加到对应桶中
        cur = head
        while cur:
            # cur 的值在第几个区间中就放在第几个桶里面
            # 先求指标
            index = (cur.val - list_min) // bucket_size
            # 再插入桶中
            self.insertion(buckets, index, cur.val)
            cur = cur.next
        # 哑结点
        dummy_head = ListNode(-1)
        cur = dummy_head
        # 将元素依次出桶，并拼接成有序链表
        # 把每个桶都处理一遍
        for bucket_head in buckets:
            # 对于当且所在桶，将其排序得到
            bucket_cur = self.mergeSort(bucket_head)
            # 若桶非空，则元素出桶
            while bucket_cur:
                # 连接桶内元素
                cur.next = bucket_cur
                # 当前指针向前滚动
                cur = cur.next
                # 元素出桶，直接指向下一个元素
                bucket_cur = bucket_cur.next
                
        return dummy_head.next
    
    def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        return self.bucketSort(head)
```

### 单链表基数排序
基数排序是一种非比较性的排序算法，它根据键值的每位数字来进行分配排序。基数排序适用于排序的数的位数较少的情况，如果每个数字的位数都相同或者数字的位数相对较小，则基数排序是一种非常高效的排序算法。

一般情况下,元素 data[i] 的关键字 data[i].key由 $d$ 位数字(或字符)组成,即 $k^{d}k^{d-1} \cdots k^2k^0$, 每一个数字表示关键字的一位,其中 $k^{d}$ 为最高位. $k^1$ 是最低位,每一位的值都在范围  $0 \leq k^{i} < r$  内, 其中 $r$ 称为基数(radix)。例如,对于二进制数 $r$ 为 $2$,对于十进制数 $r$ 为 $10$。

甚至可以对不同的位置，有不同的基数。只要每个位上面的元素都是统一有序的（都是递增或都是递减），那么所有数据都是有序的。

基本思想：
基数排序的基本思想是将待排序的元素按照其个位、十位、百位等每一位的数值来进行排序，从低位到高位依次进行排序，直到最高位。这样，当所有位都被考虑完毕后，数组就被排好序了。

算法步骤：
1. 确定排序的轮数：首先确定待排序数的最高位数，决定了需要进行多少轮排序。例如，如果待排序数的最大值有 k 位，则需要进行 k 轮排序。

2. 按位排序：从个位开始到最高位，依次对待排序的数进行排序。每一轮排序都根据当前位的值进行分配，然后按照分配的顺序重新排列待排序数组。

3. 重组排序后的数组：完成所有位的排序后，将排序后的元素依次放入数组中，得到最终的有序序列。


```python
class Solution:
    def radixSort(self, head: ListNode):       
        # 计算位数最长的位数
        # 用来确定要循环多少次
        size = 0
        cur = head
        while cur:
            val_len = len(str(cur.val))
            if val_len > size:
                size = val_len
            cur = cur.next
            
        # 如果位数不同，基数也不同
        # radix 可以设置成一个数组 radix[]
        # 对不同的位数 i 设置不同的基数 radix[i]
        radix
        # 从个位到高位遍历位数
        for i in range(size):
            # 生成 10 个列表
            # 如果是 k 进制，就是 k 个列表
            # 用来进行分配 第 i 位 为 k 的元素
            buckets = [[] for _ in range(radix)]
            cur = head
            while cur:
                # 以每个节点对应位数上的数字为索引，将节点值放入到对应桶中
                # i 时对应的位数是第 i+1 位
                # 比如 i=0 时，取出的是个位
                buckets[cur.val // (10 ** i) % 10].append(cur.val)
                cur = cur.next
            
            # 生成新的链表
            dummy_head = ListNode(-1)
            cur = dummy_head
            for bucket in buckets:
                for num in bucket:
                    cur.next = ListNode(num)
                    cur = cur.next
            head = dummy_head.next
            
        return head
    
    def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        return self.radixSort(head)

```
