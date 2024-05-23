#### 622 设计循环队列
https://leetcode.cn/problems/design-circular-queue/description/

自己按照列表实现的想法码一下即可。


```python
class MyCircularQueue(object):

    def __init__(self, k):
        """
        :type k: int
        """
        self.que = [0 for i in range(k+1)]
        self.front = 0
        self.rear = 0
        self.size = k

    def enQueue(self, value):
        """
        :type value: int
        :rtype: bool
        """
        if self.isFull():
            return False
        else:
            self.rear = (self.rear+1) % (self.size +1)
            self.que[self.rear] = value
            return True

    def deQueue(self):
        """
        :rtype: bool
        """
        if self.isEmpty():
            return False
        self.front = (self.front+1)%(self.size+1)
        return True


    def Front(self):
        """
        :rtype: int
        """
        if self.isEmpty():
            return -1
        return self.que[(self.front+1) % (self.size + 1)]

    def Rear(self):
        """
        :rtype: int
        """
        if self.isEmpty():
            return -1
        return self.que[self.rear]


    def isEmpty(self):
        """
        :rtype: bool
        """
        if self.rear == self.front:
            return True
        else:
            return False 
        


    def isFull(self):
        """
        :rtype: bool
        """
        if (self.rear+1)% (self.size+1) == self.front:
            return True
        else:
            return False 



# Your MyCircularQueue object will be instantiated and called as such:
# obj = MyCircularQueue(k)
# param_1 = obj.enQueue(value)
# param_2 = obj.deQueue()
# param_3 = obj.Front()
# param_4 = obj.Rear()
# param_5 = obj.isEmpty()
# param_6 = obj.isFull()
```

#### 滑动平均值


```python
class MovingAverage(object):

    def __init__(self, size):
        """
        Initialize your data structure here.
        :type size: int
        """
        self.size = size
        self.que = [0 for i in range(size+1)]
        self.front = 0
        self.rear = 0
        self.total = 0
        self.num = 0
    def next(self, val):
        """
        :type val: int
        :rtype: float
        """

        # 重点在于对 满队列 和 非满队列 的处理，
        # 满队列时，分母是size
        # 非满队列时，分母是当前元素的数量，需要定义一个 num 并且在非满的时候更新
        if self.isFull():
            a = self.Front()
            self.deQueue()
            self.enQueue(val)
            self.total = self.total + val - a
            return float(self.total)/float(self.size)
        else:
            self.enQueue(val)
            self.total = self.total +val
            self.num += 1
            return float(self.total)/float(self.num)

    def enQueue(self, value):
        """
        :type value: int
        :rtype: bool
        """
        if self.isFull():
            return False
        else:
            self.rear = (self.rear+1) % (self.size +1)
            self.que[self.rear] = value
            return True

    def deQueue(self):
        """
        :rtype: bool
        """
        if self.isEmpty():
            return False
        self.front = (self.front+1)%(self.size+1)
        return True


    def Front(self):
        """
        :rtype: int
        """
        if self.isEmpty():
            return 0
        return self.que[(self.front+1) % (self.size + 1)]

    def Rear(self):
        """
        :rtype: int
        """
        if self.isEmpty():
            return 0
        return self.que[self.rear]


    def isEmpty(self):
        """
        :rtype: bool
        """
        if self.rear == self.front:
            return True
        else:
            return False 
        


    def isFull(self):
        """
        :rtype: bool
        """
        if (self.rear+1)% (self.size+1) == self.front:
            return True
        else:
            return False 




# Your MovingAverage object will be instantiated and called as such:
# obj = MovingAverage(size)
# param_1 = obj.next(val)
```

#### 239 滑动窗口最大值
https://leetcode.cn/problems/sliding-window-maximum/description/


```python
class Solution:
    def maxSlidingWindow(self, nums, k):
        size = len(nums)
        q = [(-nums[i], i) for i in range(k)]
        # 对这一行的注释见下
        heapq.heapify(q)
        # 堆顶元素最大，所以添加到 res 数组中
        res = [-q[0][0]]
        # 对 k 到 size -1 的数据开始遍历
        for i in range(k, size):
            # 先把数据进队
            heapq.heappush(q, (-nums[i], i))
            # 判断当前最大数据的索引是否在范围内
            # 不在就出队
            # 此时最后一个进队的元素是 i 所以窗口索引的范围是 i-k+1 到 i
            # while 执行的标准是 q[0][1] <= i-k
            while q[0][1] <= i - k:
                heapq.heappop(q)
            res.append(-q[0][0])
        return res

```

在Python的`heapq`模块中，`heapify`函数用于将一个列表转换成堆结构，它默认按照列表中每个元素的首个元素进行比较来决定优先级。也就是说，当执行`heapify(q)`时，它会认为列表`q`中的每个元素是一个可以比较的序列（通常是元组或列表），并且以每个元素的第一个元素作为堆排序的优先级依据。

在你的代码示例中，`q`列表中的元素是形如`(-nums[i], i)`的元组。这里，`-nums[i]`是作为优先级的关键字（因为堆默认是最小堆，取负值是为了模拟最大堆行为），而`i`是用来辅助判断元素是否过期（是否在当前窗口内）的索引。`heapify(q)`在执行时，自然就会以每个元组的第一个元素，即`-nums[i]`，作为判断堆中元素优先级的标准。

总结来说，`heapq`模块在处理序列元素时，默认以每个元素的第0号索引（即第一个元素）作为比较的基准，来构建和调整堆结构，而无需额外指定。在本例中，正是利用这一点，通过负值来巧妙地实现了最大值的堆排序。


```python

```

#### 347 前 K 个高频元素
https://leetcode.cn/problems/top-k-frequent-elements/description/


```python
import heapq
class Solution(object):
    def topKFrequent(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        freq = dict()
        for num in nums:
            if num in freq:
                freq[num] += 1
            else:
                freq[num] = 1
        freq_list = [ [-v,k] for k,v in freq.items() ]
        heapq.heapify(freq_list)
        #print(freq_list)
        result = []
        for i in range(k):
            val, num = heapq.heappop(freq_list)  # 弹出堆顶元素并解包
            result.append(num)  # 将弹出的元素添加到结果列表中
        #print(result)
        return result
```


```python
nums = [1,1,1,2,2,3]
k = 2
sol = Solution()
sol.topKFrequent(nums,k)
```




    [1, 2]



有一个借助别的代码包的作弊方法


```python
from collections import Counter

class Solution:
    def topKFrequent(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        # 使用collections.Counter计算每个元素的频率
        count = Counter(nums)
        
        # 根据频率对元素进行排序，然后取前k个
        return [item for item, freq in count.most_common(k)]

# 测试用例
nums = [1,1,1,2,2,3]
k = 2
solution = Solution()
print(solution.topKFrequent(nums, k))  # 应该输出 [1,2]
```

    [1, 2]
    

`return [item for item, freq in count.most_common(k)] ` 

这行代码是一个列表推导式（List Comprehension），它是一种简洁的创建列表的方法。在这里，它用于从一个迭代过程中直接构造一个新列表。具体来说，这行代码的作用是从`count.most_common(k)`产生的迭代器中提取元素。

详细解析如下：

- `count.most_common(k)`: 这部分调用了`Counter`对象`count`的`most_common`方法，该方法返回一个列表，其中包含元素及其频率的元组，按频率降序排序。`k`参数限制了返回的元素数量，只返回前`k`个出现频率最高的元素。

- `item, freq in count.most_common(k)`: 这部分是一个循环，遍历`count.most_common(k)`返回的列表。在每次迭代中，`item`和`freq`分别接收元组的两个元素，即元素本身和其频率。

- `return [item for ...]`: 列表推导式的主体部分，对于`count.most_common(k)`产生的每个`(item, freq)`元组，只保留`item`（即元素本身），并将这些`item`收集起来形成一个新的列表。

综上所述，这行代码的含义是：从频率最高的`k`个元素中，提取每个元素（忽略它们的频率），并将这些元素放入一个新的列表中作为返回值。

#### 215 数组中的第K个最大元素
https://leetcode.cn/problems/kth-largest-element-in-an-array/description/

根据上题的代码修改一下即可


```python
import heapq
class Solution(object):
    def frequencySort(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        freq = dict()
        for num in nums:
            if num in freq:
                freq[num] += 1
            else:
                freq[num] = 1
        freq_list = [ [-k,v] for k,v in freq.items() ]
        heapq.heapify(freq_list)
        # 做好排序后开始更新 result
        result = []
        # 把堆中的所有 数值val 和 频率rep 拿出来按照大小*频率的顺序排列进列表中
        for i in range(len(freq_list)):
            val, rep = heapq.heappop(freq_list)  # 弹出堆顶元素并解包
            for i in range(rep):
                result.append(-val)  # 将弹出的元素添加到结果列表中
        # 最后取出列表中的第k个元素即可
        return result[k-1]
    
nums = [-1,-1]
k = 2
sol = Solution()
sol.topKFrequent(nums,k)
```




    -1



#### 451 根据字符出现频率排序
https://leetcode.cn/problems/sort-characters-by-frequency/description/

按照上题的代码修改一下即可。


```python
import heapq
class Solution(object):
    def frequencySort(self, s):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        nums = s
        freq = dict()
        for num in nums:
            if num in freq:
                freq[num] += 1
            else:
                freq[num] = 1
        freq_list = [ [-v,k] for k,v in freq.items() ]
        heapq.heapify(freq_list)
        # 按频率排序之后
        # 再挨个连接
        result = ''
        for i in range(len(freq_list)):
            val, num = heapq.heappop(freq_list)
            # 频率有多少 就连接多少
            for i in range(-val):
                result = result + num
        #print(result)
        return result
```


```python
sol = Solution()
sol.frequencySort("Aabb")

```




    'bbAa'




```python

```
