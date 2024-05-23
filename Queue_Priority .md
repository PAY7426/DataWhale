### 二叉堆实现优先队列
下列代码中没有__init__()函数，则该函数默认为空。


```python
class Heapq:
    # 堆调整方法：调整为大顶堆
    # 这里的数组采用的是 0 开头的索引
    # 1 开头  0 开头
    #  i       i-1 = j
    # 2i       2i-1 = (2j+2)-1 = 2j+1
    # 所以左子树结点是 index*2 + 1
    def heapAdjust(self, nums: [int], index: int, end: int):
        # index: 当前根结点
        # end: 最后一个结点的索引
        # 先获取左子树 left 和右子树 right
        left = index * 2 + 1
        right = left + 1
        while left <= end:
            # 当前节点为非叶子结点
            max_index = index
            if nums[left] > nums[max_index]:
                max_index = left
            if right <= end and nums[right] > nums[max_index]:
                max_index = right
            if index == max_index:
                # 如果不用交换，则说明已经交换结束
                # 从 index 下面开始的树 都是有序堆结构
                # 不需要再调整了，因此 break
                break
            # 调整结构，交换顺序
            nums[index], nums[max_index] = nums[max_index], nums[index]
            # 继续调整子树
            index = max_index
            left = index * 2 + 1
            right = left + 1
    
    # 将数组构建为二叉堆
    # 从最后一层非叶子结点向上调整
    # 就像上浮的气球一样
    def heapify(self, nums: [int]):
        size = len(nums)
        # (size - 2) // 2 是最后一个非叶节点，叶节点不用调整
        # 从 (size-2)//2 到 -1 遍历索引，步长为 -1 
        for i in range((size - 2) // 2, -1, -1):
            # 调用调整堆函数
            self.heapAdjust(nums, i, size - 1)
    
    # 入队操作
    def heappush(self, nums: list, value):
        nums.append(value)
        size = len(nums)
        i = size - 1
        # 寻找插入位置
        while (i - 1) // 2 >= 0:
            cur_root = (i - 1) // 2
            # value 小于当前根节点，则插入到当前位置
            if nums[cur_root] > value:
                break
            # 继续向上查找
            nums[i] = nums[cur_root]
            i = cur_root
        # 找到插入位置或者到达根位置，将其插入
        nums[i] = value
                
    # 出队操作
    def heappop(self, nums: list) -> int:
        size = len(nums)
        nums[0], nums[-1] = nums[-1], nums[0]
        # 得到最大值（堆顶元素）然后调整堆
        top = nums.pop()
        # 因为最后一个元素和堆顶元素交换后要出队
        # 所以数组长度减一，调用堆调整方法的时候 end 的索引为为 size -2 
        if size > 0:
            self.heapAdjust(nums, 0, size - 2)
            
        return top
    
    # 升序堆排序
    def heapSort(self, nums: [int]):
        self.heapify(nums)
        size = len(nums)
        for i in range(size):
            # 把堆顶最大的元素移到最后
            nums[0], nums[size - i - 1] = nums[size - i - 1], nums[0]
            # 对前面的部分进行整理，确保目前最大的元素在堆顶
            # 实际上就是把原来堆顶的两个子树中的最大值提上来
            # 并且把放在堆顶的最小值沉下去
            # 所以只需整理就可以了，不需要将数组变为堆那样复杂的操作
            self.heapAdjust(nums, 0, size - i - 2)
        return nums

```


```python
# 创建一个 Heapq 类的实例
heap_instance = Heapq()

# 假设我们有一些数据
data = [19, 7, 11, 8, 16, 10, 21]

# 使用 heapify 方法将列表构建成一个堆
heap_instance.heapify(data)

# 现在 data 已经是一个堆结构了，可以进行其他操作
print("构建堆后的数据:", data)

# 向堆中插入一个元素
heap_instance.heappush(data, 25)
print("插入元素后的数据:", data)

# 从堆中弹出最大元素
max_value = heap_instance.heappop(data)
print("弹出的最大值:", max_value)
print("弹出后的数据:", data)

# 对数据进行堆排序
sorted_data = heap_instance.heapSort(data)
print("堆排序后的数据:", sorted_data)
```

    构建堆后的数据: [21, 16, 19, 8, 7, 10, 11]
    插入元素后的数据: [25, 21, 19, 16, 7, 10, 11, 8]
    弹出的最大值: 25
    弹出后的数据: [21, 16, 19, 8, 7, 10, 11]
    堆排序后的数据: [7, 8, 10, 11, 16, 19, 21]
    

#### 利用 python 自带的 heapq 实现


```python
import heapq

class PriorityQueue:
    def __init__(self):
        self.queue = []
        self.index = 0

    def push(self, item, priority):
        heapq.heappush(self.queue, (-priority, self.index, item))
        self.index += 1

    def pop(self):
        return heapq.heappop(self.queue)[-1]

```

`heapq.heappush(self.queue, (-priority, self.index, item))` 这一行代码的工作机制涉及到Python标准库中的`heapq`模块，它是用来处理堆数据结构的工具。这里主要目的是向一个优先级队列（基于堆实现）中插入一个新元素，并维持队列的优先级特性。下面详细解释这一行代码的运行机制：

1. **`heapq.heappush`**: 这是`heapq`模块提供的一个函数，用于向堆（在这个场景下是优先级队列）中添加一个元素，并自动调整堆结构以保持堆的性质。堆的性质是父节点的值（在这个上下文中是优先级的负值）总是小于或等于其子节点的值，从而确保最小值（在这里是最高优先级）总是在堆顶。

2. **`self.queue`**: 这是类`PriorityQueue`中的一个实例变量，用于存储堆中的元素。它是一个列表，用来表示堆结构。当调用`heappush`时，实际上是向这个列表中添加元素，并且`heapq`会确保这个列表满足堆的性质。

3. **`(-priority, self.index, item)`**: 这个元组是要插入堆中的元素。它包含三个部分：
   - `(-priority)`: 将优先级取负值的原因是为了将最大优先级的元素变为最小堆中的最小值，从而确保高优先级的元素总是位于堆顶。因为`heapq`模块默认实现最小堆，这样处理可以方便地将最大优先级的元素放在堆顶。
   - `self.index`: 这是一个递增的计数器，用于在优先级相同时决定元素的顺序。当两个元素具有相同的优先级时，先加入队列的元素（具有较小的索引值）会被认为优先级更高，这样可以保持元素的先进先出顺序。
   - `item`: 实际存储在队列中的数据，可以是任何类型，代表需要管理的具体对象或任务。

综上所述，当执行这行代码时，它会将一个包含优先级、索引和实际数据的元组插入到`self.queue`中，并通过`heappush`函数自动调整列表结构以保持堆的性质，确保新插入的元素处于正确的位置，使堆顶始终是最高优先级的元素。


```python

```
