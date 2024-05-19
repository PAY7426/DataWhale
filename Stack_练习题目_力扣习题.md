### 20 有效的括号
https://leetcode.cn/problems/valid-parentheses/description/

注意在判断最后一个元素是否和当前括号匹配时要先判断 len(stack) != 0 是否为真。因为当列表为空的时候，返回最后一个元素会报错，Python 会抛出 IndexError 异常，因为列表中不存在索引为 -1 的元素。


```python
class Solution:
    def isValid(self, s):
        if len(s)%2 == 1:
            return False
        stack = []
        for ch in s:
            if ch == '[' or  ch == '(' or  ch == '{' :
                stack.append(ch)
            elif ch == "]":
                # 当前列表非空，判断最后一个元素是否和当且括号匹配
                if len(stack) != 0 and stack[-1] == "[":
                    stack.pop()
                else:
                    return False
            elif ch == ")":
                if len(stack) != 0  and stack[-1] == "(":
                    stack.pop()
                else:
                    return False
            elif ch == "}":
                if len(stack) != 0  and stack[-1] == "{":
                    stack.pop()
                else:
                    return False
        if len(stack) != 0 :
            return False
        else :
            return True
```

#### 227 基本计算器
https://leetcode.cn/problems/basic-calculator-ii/description/


```python
class Solution(object):
    def calculate(self, s):
        """
        :type s: str
        :rtype: int
        """
        size = len(s)
        stack = []
        op = '+'
        index = 0
        while index <size:
            if s[index] == ' ':
                index += 1
                continue 
            if s[index].isdigit():
                # 取出第 index 位的数字的 int 版本
                num = ord(s[index])-ord('0')
                # 如果不是个位数
                while index+1< size and s[index+1].isdigit():
                    num = 10*num + ord(s[index+1])-ord('0')
                    index += 1
                if op == '+':
                    stack.append(num)
                elif op == '-':
                    stack.append(-num)
                elif op == '*':
                    num = stack.pop() * num
                    stack.append(num)
                elif op == '/':
                    # 当操作符为 / 时会碰到问题
                    # 例如 14 - 3/2
                    # 此时 stack 顶元素是 -3 
                    # 但是 -3/2 =-2，而我们知道应该先计算 3/2=1
                    # 再计算14-1 ，所以这里要对栈顶元素为负数情况单独处理
                    a = stack.pop()
                    if a <0:
                        num = - ( (-a)/num )
                    else:
                        num = a / num
                    stack.append(num)
            elif s[index] in '+-*/':
                op = s[index]
            index += 1
        total = 0
        while len(stack)!=0:
            total = total+stack.pop()
        return total

```

#### 155 最小栈
https://leetcode.cn/problems/min-stack/  

以下是自己实现的代码。刚开始的时候在 MinStack 中设置了属性 self.top 但是这个属性和 top 函数冲突了。所以要改个名字。


```python
class ListNode:
    def __init__(self, value):
        self.value = value
        self.next = None

class MinStack:
    def __init__(self):
        self.head = None
        self.min = None
        
    def is_empty(self):
        return self.head == None
    
    # 获取最小值
    def getMin(self):
        return self.min

    # 入栈
    def push(self,value):
        if self.is_empty():
            self.min = value
        else:
            if value <self.min:
                self.min = value
        cur = ListNode(value)
        cur.next = self.head
        self.head = cur
        return None
        
    # 出栈
    def pop(self):
        if self.is_empty():
            return None
        else:
            self.head = self.head.next
            cur = self.head
            if self.is_empty():
                self.min = None
            else:
                min_stack = cur.value
                while cur:
                    if min_stack >cur.value:
                        min_stack = cur.value
                    cur = cur.next
                self.min = min_stack
            return None
        
    # 获取栈顶元素
    def top(self):
        if self.is_empty():
            return None
        else:
            return self.head.value
```

下面这个代码更巧妙，他在生成栈的时候同时维护了一个最小值栈。这个最小值栈存储了栈的前n个元素中的最小值。


```python
class MinStack(object):

    def __init__(self):
        self.stack=[]
        # minstack 中的每一个元素，都是 stack 中第一个到对应位置的所有元素中的最小值
        # minstack[i] = min { stack[j] : 0 < =j < i  }
        self.minstack=[float("inf")]

    def push(self, val):
        """
        :type val: int
        :rtype: None
        """
        self.stack.append(val)
        # 比较一下当前元素与之前的最小值
        # 将二者的最小值 push 进最小值栈中
        self.minstack.append(min(self.minstack[-1],val))


    def pop(self):
        """
        :rtype: None
        """
        self.stack.pop()
        self.minstack.pop()


    def top(self):
        """
        :rtype: int
        """
        return self.stack[-1]


    def getMin(self):
        """
        :rtype: int
        """
        return self.minstack[-1]



```

#### 20 有效的括号
https://leetcode.cn/problems/valid-parentheses/description/

笔记里有，自己尝试重写一下


```python
class Solution:
    def isValid(self, s):
        if len(s)%2 == 1:
            return False
        stack = []
        for ch in s:
            if ch == '[' or  ch == '(' or  ch == '{' :
                stack.append(ch)
            elif ch == "]":
                if len(stack) != 0 and stack[-1] == "[":
                    stack.pop()
                else:
                    return False
            elif ch == ")":
                if len(stack) != 0  and stack[-1] == "(":
                    stack.pop()
                else:
                    return False
            elif ch == "}":
                if len(stack) != 0  and stack[-1] == "{":
                    stack.pop()
                else:
                    return False
        if len(stack) != 0 :
            return False
        else :
            return True
```

#### 150 逆波兰表达式
https://leetcode.cn/problems/evaluate-reverse-polish-notation/description/


```python
class Solution(object):
    def evalRPN(self, tokens):
        """
        :type tokens: List[str]
        :rtype: int
        """
        # 就是一个栈操作，当遇到运算符号的时候，把栈顶两个 数字 取出来操作一下
        # 到最后还剩下的 数字 就是最终结果
        stack = []
        for i in tokens:
            # 先判断是否是运算符号
            if i in "+*/-":
                if i == '+':
                    a = stack.pop()
                    b = stack.pop()
                    stack.append(b + a)
                elif i == '-':
                    a = stack.pop()
                    b = stack.pop()
                    stack.append(b - a)
                elif i == '*':
                    a = stack.pop()
                    b = stack.pop()
                    stack.append(b * a)
                elif i == '/':
                    a = stack.pop()
                    b = stack.pop()
                    # 除法这里
                    if a*b<0:
                        stack.append(-int(-b/a))
                    else:
                        stack.append( int(b/a))
            # 如果不是运算符号，就是数字字符
            # 将字符转换为数字
            else:
                # 可以直接像下面一样吧字符转换为数字
                # stack.append(int(i))
                #' 也可以像下面这样重新计算一下数字
                num = 0
                if i[0] == '-':

                    for j in range(len(i)-1):
                        a = ord( i[j+1] ) - ord('0')
                        # 这里算数字的公式要记住 前面的乘10向前移位，再加上个位数
                        num = num * 10 + a
                    stack.append(-num)
                else:
                    for j in range(len(i)):
                        a = ord(i[j]) - ord('0')
                        num = num * 10 + a
                    stack.append(num)  
                #''' 
        return stack.pop()                 
```

#### 946 验证栈序列
https://leetcode.cn/problems/validate-stack-sequences/description/

考虑借用一个辅助栈 stackstackstack ，模拟 压入 / 弹出操作的排列。根据是否模拟成功，即可得到结果。

1. 入栈操作： 按照压栈序列的顺序执行。
2. 出栈操作： 每次入栈后，循环判断 “栈顶元素 === 弹出序列的当前元素” 是否成立，将符合弹出序列顺序的栈顶元素全部弹出。
由于题目规定 栈的所有数字均不相等 ，因此在循环入栈中，每个元素出栈的位置的可能性是唯一的（若有重复数字，则具有多个可出栈的位置）。因而，在遇到 “栈顶元素 === 弹出序列的当前元素” 就应立即执行出栈。



```python
class Solution(object):
    def validateStackSequences(self, pushed, popped):
        """
        :type pushed: List[int]
        :type popped: List[int]
        :rtype: bool
        """
        stack = []
        i = 0
        # 让 pushed 的元素模拟一个栈
        # 当碰到 popped 链表头时候就说明当且元素出栈了
        # 然后 stack.pop 并且 popped 删除链表头，用 i=i+1 模拟
        for ele in pushed:
            stack.append(ele)
            while stack and stack[-1] == popped[i]:
                stack.pop()
                i = i+1
        return len(stack) == 0

```

#### 394 字符串解码
https://leetcode.cn/problems/decode-string/description/


```python
class Solution(object):
    def decodeString(self, s):
        """
        :type s: str
        :rtype: str
        """
        # 新建一个栈用来存储 倍数 和 字符串信息
        # 新建倍数为0，字符串为空
        stack = []
        times = 0
        now_str = ''
        for i in range(len(s)):
            # 当前为数字的时候，继续累积
            if s[i].isdigit():
                a = ord(s[i])-ord('0')
                times = times * 10 + a
            # 当前为左括号时，上一段处理已结束
            # 将倍数和当前字符串压入栈并初始化
            elif s[i] == '[':
                stack.append(times)
                stack.append(now_str)
                now_str = ''
                times = 0
            # 当前为有括号时，开始处理
            elif s[i] == ']':
                # 首先保存上一次处理过程中的字符串
                past_str = stack.pop()
                # 千不该万不该，这里还用了times
                # 这里用 times 之后不重置的话，在之前的模块中就会在当前的times上累积
                # times = stack.pop()
                # 直接新建一个变量即可
                # 然后取出倍数，和 当前字符串的倍乘 连在一起
                num = stack.pop()
                now_str = past_str + now_str * num
            # 开始累积当前的字符串，直到碰到数字或右括号
            else:
                now_str = now_str + s[i]
        return now_str

```


```python
class Solution(object):
    def decodeString(self, s):
        """
        :type s: str
        :rtype: str
        """
        stack = []
        current_string = ""
        current_num = 0

        for char in s:
            if char.isdigit():
                current_num = current_num * 10 + int(char)
            elif char == '[':
                # Push the current string and the number onto the stack
                stack.append(current_string)
                stack.append(current_num)
                # Reset current string and current num
                current_string = ""
                current_num = 0
            elif char == ']':
                # Pop the number and the previous string from the stack
                num = stack.pop()
                previous_string = stack.pop()
                # Repeat the current string 'num' times and concatenate with the previous string
                current_string = previous_string + current_string * num
            else:
                # Simply add the current character to the current string
                current_string += char
        
        return current_string

```

#### 496 下一个更大元素
https://leetcode.cn/problems/next-greater-element-i/description/


```python
class Solution:
    def nextGreaterElement(self, nums1, nums2) :
        # 存储结果
        res = []
        # 用来遍历数组 nums2
        stack = []
        # 建立映射 nums2 的元素映射到 下一个更大的元素
        num_map = dict()
        # 开始遍历
        for num in nums2:
            # 当栈非空并且当前元素大于栈顶元素时
            # 说明找到了 栈顶元素的下一个更大元素
            while stack and num > stack[-1]:
                # 接着要建立映射并且弹出栈顶元素
                num_map[stack[-1]] = num
                stack.pop()
            # 否则将当前数字加入栈中
            stack.append(num)
        for num in nums1:
            # get(key,ele) 返回键为key对应的值，如果key不存在则返回ele 
            res.append(num_map.get(num, -1))
        return res

```

#### 316 去除重复字母



```python
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:
        # 统计每个字符出现的次数
        count = [0] * 26
        for ch in s:
            count[ord(ch) - ord('a')] += 1
        
        # 记录结果字符串中已经包含的字符
        seen = [False] * 26
        
        # 用栈来构建结果字符串
        stack = []
        
        for ch in s:
            # 更新字符的计数
            count[ord(ch) - ord('a')] -= 1
            
            # 如果当前字符已经在结果中，跳过
            if seen[ord(ch) - ord('a')]:
                continue
            
            # 当栈不为空且当前字符小于栈顶字符且栈顶字符后面还有机会加入结果时，
            # 弹出栈顶字符，因为它可以被之后的更小字符替换以优化字典序
            while stack and ch < stack[-1] and count[ord(stack[-1]) - ord('a')] > 0:
                seen[ord(stack.pop()) - ord('a')] = False
            
            # 将当前字符加入栈中，并标记为已添加
            stack.append(ch)
            seen[ord(ch) - ord('a')] = True
        
        # 构建最终的字符串
        return ''.join(stack)
```

#### 496 下一个更大的元素
https://leetcode.cn/problems/next-greater-element-i/


```python
class Solution:
    def nextGreaterElement(self, nums1, nums2) :
        # 存储结果
        res = []
        # 用来遍历数组 nums2
        stack = []
        # 建立映射 nums2 的元素映射到 下一个更大的元素
        num_map = dict()
        # 开始遍历
        for num in nums2:
            # 当栈非空并且当前元素大于栈顶元素时
            # 说明找到了 栈顶元素的下一个更大元素
            while stack and num > stack[-1]:
                # 接着要建立映射并且弹出栈顶元素
                num_map[stack[-1]] = num
                stack.pop()
            # 否则将当前数字加入栈中
            stack.append(num)
        for num in nums1:
            # get(key,ele) 返回键为key对应的值，如果key不存在则返回ele 
            res.append(num_map.get(num, -1))
        return res

```

#### 739 每日温度
https://leetcode.cn/problems/daily-temperatures/description/


```python
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        # 获取温度列表的长度
        n = len(T)
        
        # 初始化一个栈，用于存放待比较的温度索引
        stack = []
        
        # 初始化结果列表，长度与温度列表相同，初始值全为0
        ans = [0 for _ in range(n)]
        
        # 遍历温度列表
        for i in range(n):
            # 当栈不为空且当前温度大于栈顶索引对应的温度时
            while stack and T[i] > T[stack[-1]]:
                # 弹出栈顶索引
                index = stack.pop()
                
                # 计算并记录距离下一个更高温度的天数，即当前位置索引i与弹出索引的差值
                ans[index] = (i-index)
            
            # 将当前温度的索引压入栈中，等待与之后的温度比较
            stack.append(i)
        
        # 返回结果列表，其中的每个值代表了对应位置温度需要等待几天才会遇到更高的温度
        return ans
```
