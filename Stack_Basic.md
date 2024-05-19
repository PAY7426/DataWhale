### 堆栈的顺序存储实现代码


```python
class Stack:
    def __init__(self, size = 100):
        self.stack = []
        self.size = size
        self.top = -1
    
    def is_empty(self):
        return self.top == -1
    
    def is_full(self):
        return self.top +1 == self.size
            # 不能直接写 self.top == size
            # 一方面从 0 到 size -1 就有 size 个元素了
            # 另一方面 size 不是传入参数，而是 self 的属性
     
    # 入栈
    def push(self,val):
        if self.is_full():
            raise Exception('Stack is full')
        else: 
            self.stack.append(val)
            self.top += 1
            
    # 出栈，将栈顶元素返回并删除栈顶元素，top = top -1
    def pop(self):
        if not self.is_empty:
            val = self.stack.pop()
            self.top - = 1
            return val
        else: 
            raise Exception("Stack is empty.")
            
    # 获取栈顶元素，不需要删除
    def peek(self):
        if self.is_empty():
            raise Exception("Stack is empty")
        else:
            return self.stack[self.top]
               
        
```

### 堆栈的链式存储实现代码


```python
# 借助链表结点实现栈
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None

class Stack:
    def __init__(self):
        self.top = None
        
    def is_empty(self):
        return self.top == None
    
    # 入栈
    def push(self,value):
        cur = Node(value)
        cur.next = self.top
        self.top = cur
        
    # 出栈
    def pop(self):
        if self.is_empty():
            raise Exception("Stack is empty")
        else:
            cur = self.top
            self.top = self.top.next
            val = cur.value
            del cur
            return val
        
    # 获取栈顶元素
    def peek(self):
        if self.is_empty():
            raise Exception("Stack is empty")
        else:
            return self.top.value
```


```python

```
