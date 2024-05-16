```python
class ListNode():
    def __init__(self,val,next = None):
        self.val = val
        self.next = next 
        
class LinkedList():
    def __init__(self):
        self.head = None
        
    def create(self,data):
        self.head = ListNode(data[0])  #初始的空节点，以0为默认值
        cur = self.head
        for i in range(len(data)-1):
            node = ListNode(data[i+1])
            cur.next = node
            cur = cur.next
            
    def length(self):
        count = 0
        cur = self.head
        while cur:
            count +=1
            cur = cur.next
        return count
    
    def find(self,val):
        cur = self.head
        while cur:
            if cur.val == val:
                return cur
            cur = cur.next
        return None
    
    def insertFront(self,val):  #在头结点之前插入结点
        node = ListNode(val)
        node.next = self.head
        self.head = node
        
    def insertRear(self, val):
        node = ListNode(val)
        cur = self.head
        while cur:
            if cur.next != None:
                cur = cur.next
        cur.next = node
        
    def insertInside(self,index,val):
        count = 0
        cur = self.head
        while count != index-1 and cur:
            count = count+1
            cur = cur.next
        if not cur:
            return "Error"
        node = ListNode(val)
        node.next = cur.next
        cur.next = node
         
    def change(self,index,val):
        count = 0
        cur = self.head
        while count != index and cur:
            count = count+1
            cur = cur.next
        if not cur:
            return "Error"
        cur.val = val
        
    def removeFront(self):   #要注意 self 是LinkedList 结构，但 self.head 是 ListNode 结构
        if self.head != None:
            self.head = self.head.next
            
    def removeRear(self):
        if not self.head or not self.head.next:
            return 'Error'

        cur = self.head
        while cur.next.next:  # 这一句很重要
            cur = cur.next
        cur.next = None
        
    
        
    def removeInside(self,index):
        count = 0
        cur = self.head
        while cur and cur < index-1:
            count += 1
            cur = cur.next
        if not cur:
            return "Error"
        cur.next = cur.next.next
    
```


```python

```
