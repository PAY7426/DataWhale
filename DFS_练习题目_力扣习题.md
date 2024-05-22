#### 494 目标和
https://leetcode.cn/problems/target-sum/

下面这部分代码用的是深度优先搜索。从起始位置开始，sum从0开始。分别加减当前数字，直到遍历完所有数字的所有符号。但是这个会超时，因为遍历太多了。


```python
class Solution:
    def findTargetSumWays(self, nums, target):
        # 首先取出数组长度
        size = len(nums)
        # 接着定义深度优先搜索算法
        # i 是下标，cur_sum 是从数组从下标 0 加到 i 的各种情况
        def dfs(i, cur_sum):
            #处理遍历到最后一个数字的情况，此时已经赋予了数组中每个数字一个符号
            # ，与 target 相等则计数 1
            if i == size:
                if cur_sum == target:
                    return 1
                # 否则为0
                else:
                    return 0
            # 如果后面还有数字，考虑 cur_sum 减去和加上 nums[i] 的影响
            # 赋予 nums[i] 正负号，并分别统计复合条件的情况
            ans = dfs(i + 1, cur_sum - nums[i]) + dfs(i + 1, cur_sum + nums[i])
            return ans
        
        return dfs(0, 0)
```

既然遍历太多了，那么可以考虑用空间换时间。对于已经遍历过的可以存储下来，再下次需要的时候直接传入即可。


```python
class Solution:
    def findTargetSumWays(self, nums, target):
        
        # 首先明确 dfs 搜索的是什么
        # 每一次深度搜索都会确定一个数组中 从开始到 i 这些元素划分出的正负集合
        # 并且附带上一个该正负集合目前累积出的和 cur_sum
        # 他返回的是通过在给定前 0-i 个元素的正负集合划分计算出的当前和 cur_sum 时
        # 后面的所有元素的正负划分集合中能够得到 target 的组合数目
        # 自然的，当 i == size 时，会判断这个 cur_sum是否是我们要的target
        # 如果是，那么当前确定的正负集合就需要计数
        # 但是在整个搜索过程中，我们都不用存储这个正负集合
        # 因为判断正负集合是否有效，取决于 i == size 时 cur_sum == target 是否为真
        size = len(nums)
        # 对当前位置，以及 cur_sum 的组合数目要统计一下，方便后面选取
        table = dict()
        
        def dfs(i, cur_sum):
            if i == size:
                if cur_sum == target:
                    return 1
                else:
                    return 0
            # 相比之前的代码块，改变在于存储  
            # 这里 (i,cur_sum) 是 key 值
            # 如果已经计算过当且的值了，之前取用就行
            if (i, cur_sum) in table:
                return table[(i, cur_sum)]
            
            # 否则要继续计算当前的值
            # 并将其存储在字典中
            cnt = dfs(i + 1, cur_sum - nums[i]) + dfs(i + 1, cur_sum + nums[i])
            table[(i, cur_sum)] = cnt
            return cnt

        return dfs(0, 0)
```

#### 200 岛屿数量
https://leetcode.cn/problems/number-of-islands/description/

这里的代码是自己想的方法，但是会超出时间。

实际上把访问过的 '1' 置为 '0' 即可，并在遍历 grid 时只dfs 非0 的结点 [i,j]就行了。


```python
class Solution(object):
    def numIslands(self, grid):
        """
        :type grid: List[List[str]]
        :rtype: int
        """
        count = 0
        # table 用来存储结点是否被访问
        table = []
        def dfs(i,j):
            # 如果结点已访问，或者结点为无效，结点为'0'，退出函数
            if [i,j] in table:
                return
            elif i < 0 or i >=n or j<0 or j>=m or grid[i][j] == '0':  
                return
            # 否则将结点添加入访问过的列表
            table.append([i,j])
            # 接着向上下左右遍历
            dfs(i+1,j)
            dfs(i-1,j)
            dfs(i,j+1)
            dfs(i,j-1)

     
        n = len(grid)
        m = len(grid[0])
        
        for i in range(n):
            for j in range(m):
                # 先将海洋遍历一下
                if grid[i][j] == '0' :
                    table.append([i,j])
                    
        for i in range(n):
            for j in range(m):
                # 如果当且结点已经访问过，则继续遍历
                if [i,j] in table:
                    continue
                else:
                    dfs(i,j)
                    count += 1
        return count      
        
```


```python
grid = [["1","1","1","1","0"],["1","1","0","1","0"],["1","1","0","0","0"],["0","0","0","0","0"]]
solution = Solution()
solution.numIslands(grid)
```

    [[0, 4], [1, 2], [1, 4], [2, 2], [2, 3], [2, 4], [3, 0], [3, 1], [3, 2], [3, 3], [3, 4], [0, 0], [1, 0], [2, 0], [2, 1], [1, 1], [0, 1], [0, 2], [0, 3], [1, 3]]
    




    1




```python
class Solution:
    # 定义深度搜索函数
    # 传入 grid 和 当前所处的地点
    def dfs(self, grid, i, j):
        n = len(grid)
        m = len(grid[0])
        # 当前是  '0' 或 地点 [i,j] 无效
        if i < 0 or i >= n or j < 0 or j >= m or grid[i][j] == '0':
            return 0
        # 如果是 '1' 那么访问过后就将其置为 '0'
        grid[i][j] = '0'
        self.dfs(grid, i + 1, j)
        self.dfs(grid, i, j + 1)
        self.dfs(grid, i - 1, j)
        self.dfs(grid, i, j - 1)

    def numIslands(self, grid) :
        count = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                # 如过当前结点是未访问过的 '1' 
                # 进行深度搜索，将当前岛屿访问完毕并且都置为 '0'
                if grid[i][j] == '1':
                    self.dfs(grid, i, j)
                    count += 1
        return count

```

#### 133 克隆图
https://leetcode.cn/problems/clone-graph/submissions/533451564/


```python

# Definition for a Node.
"""
class Node(object):
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""
class Solution:
    def cloneGraph(self, node):
        # 如果结点为空，直接返回结点即可
        if not node:
            return node
        # 否则建立访问字典
        visitedDict = dict()
        
        # 定义深度搜索建立结点的代码
        def dfs(node):
            # 如果结点已经被建立过，直接返回建立过的结点
            if node in visitedDict:
                return visitedDict[node]
            
            # 如果结点尚未建立，开始建立结点
            # 首先克隆结点，按照当前结点的值建立结点，并且附带一个列表用来保存边
            clone_node = Node(node.val, [])
            # 访问字典更新
            visitedDict[node] = clone_node
            
            # 接下来对原结点的所有的邻接结点建立结点
            for neighbor in node.neighbors:
                clone_node.neighbors.append(dfs(neighbor))
            return clone_node

        return dfs(node)

```

顺便 贴一个 用栈实现的代码


```python
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

class Solution(object):
    def cloneGraph(self, node):
        """
        :type node: Node
        :rtype: Node
        """
        # 如果当前结点为空，直接返回当前结点
        if not node:
            return None
        # 复制当前结点
        # 并且把原邻接列表直接引用今来
        newnode = Node(node.val, node.neighbors)
        # 建立栈用来保存新建结点，并且对其完善
        stack = [newnode]
        # 用来保存已复制的结点，键值对为
        # { oldnode: newnode }
        visited = {node: newnode}
        while stack:
            # 当前栈不空
            # 弹出新建结点
            n = stack.pop()
            # 根据旧邻接链表建立新邻接列表
            newneignbors = []

            for neighbor in n.neighbors:
                # 如果未建立过新结点
                if neighbor not in visited:
                    # 建立新结点，结点值为旧结点的值，新结点的邻接链表为旧结点的旧邻接列表
                    newneignbor = Node(neighbor.val, neighbor.neighbors)
                    # 对于当前处理的栈顶的结点 , 将新建立的结点加入邻接列表
                    newneignbors.append(newneignbor)
                    # 将新建的结点加入栈，留待后续处理
                    # 因为栈中加入的都是新建立的结点
                    # 因为图是无向连接图，因此一定会把所有结点都加入进来
                    # 因此当所有结点都加入到栈中时，就是反过来更新邻接列表的时候
                    stack.append(newneignbor)
                    visited[neighbor] = newneignbor
                    
                else:
                # 如果新结点已经建立过，直接添加到新邻接列表中即可
                # 当所有结点都被建立的时候，根据旧的邻接链表，直接建立新的结点就可以了
                    newneignbors.append(visited[neighbor])
                    
            # 最后把邻接列表更新 为新建立的邻接列表
            # 重点就是当栈非空的时候，邻接列表就要更新
            # 如果所有结点都是新创建的结点，那么邻接链表就得是新建立的邻接列表
            n.neighbors = newneignbors
        return newnode
```

#### 695 岛屿的最大面积
https://leetcode.cn/problems/max-area-of-island/description/

直接仿照之前的题解即可


```python
class Solution:
    # 定义深度搜索函数
    # 传入 grid 和 当前所处的地点
    def dfs(self, grid, i, j,n,m):
        # 当前是  '0' 或 地点 [i,j] 无效
        if i < 0 or i >= n or j < 0 or j >= m or grid[i][j] == 0:
            return 0
        # 如果是 '1' 那么访问过后就将其置为 '0'
        grid[i][j] = 0
        # 向周围深度搜索岛屿面积，返回上下左右的面价并加上中间的 1
        return 1 + self.dfs(grid, i + 1, j,n,m) +self.dfs(grid, i, j + 1,n,m) +self.dfs(grid, i - 1, j,n,m)+self.dfs(grid, i, j - 1,n,m)
    
    def maxAreaOfIsland(self, grid) :
        count = 0
        # 面积初始值为 0 
        area = 0
        n = len(grid)
        m = len(grid[0])
        for i in range(n):
            for j in range(m):
                # 如过当前结点是未访问过的 1
                # 进行深度搜索，将当前岛屿访问完毕并且都置为 0
                if grid[i][j] == 1:
                    a = self.dfs(grid, i, j, n, m)
                    # 当前最大面积和计算出的岛屿面积中的 max
                    area = max(area,a)
        # 返回这个面积
        return area
grid = [[1,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
solution = Solution()
solution.numIslands(grid)
```




    6



#### 841 钥匙和房间
https://leetcode.cn/problems/keys-and-rooms/


```python
class Solution(object):
    def canVisitAllRooms(self, rooms):
        """
        :type rooms: List[List[int]]
        :rtype: bool
        """
        visited  = [0 for i in range(len(rooms))]
        
        # 设置深度搜索函数
    
        def DFS(rooms,visited,room):
            # 如果当前房间已经被访问过了
            # 则钥匙已经被取走
            # 直接返回
            if visited[room] == 1:
                return
            else:
            # 如果没有访问过，则标记已访问
                visited[room] = 1
                print(f'{room} is visited.')
                # 取走钥匙，按照钥匙可以开的房间
                # 依次访问
                for i in rooms[room]:
                    DFS(rooms,visited,i)
        DFS(rooms,visited,0)
        return 0 not in visited
solution = Solution()
rooms =[[1,3],[3,0,1],[2],[0,4],[5],[2]]
solution.canVisitAllRooms(rooms)
```

    0 is visited.
    1 is visited.
    3 is visited.
    4 is visited.
    5 is visited.
    2 is visited.
    




    True



#### 130 被围绕的区域
https://leetcode.cn/problems/surrounded-regions/description/

注意到题目解释中提到：任何边界上的 O 都不会被填充为 X。 我们可以想到，所有的不被包围的 O 都直接或间接与边界上的 O 相连。我们可以利用这个性质判断 O 是否在边界上，具体地说：

1. 对于每一个边界上的 O，我们以它为起点，标记所有与它直接或间接相连的字母 O；
2. 最后我们遍历这个矩阵，对于每一个字母：
3. 如果该字母被标记过，则该字母为没有被字母 X 包围的字母 O，我们将其还原为字母 O；
4. 如果该字母没有被标记过，则该字母为被字母 X 包围的字母 O，我们将其修改为字母 X。

自己的想法是找出不跟边界接触的岛屿，但是倒在了标记这一块上面。结果题目直接反向操作，把跟边界接触或间接解除的地方标记即可。剩下的没被标记的 O 肯定是被 X 包围了。现在自己按照题解的思路写一下代码。


```python
class Solution(object):
    def solve(self, board):
        """
        :type board: List[List[str]]
        :rtype: None Do not return anything, modify board in-place instead.
        """
        n = len(board)
        m = len(board[0])
        def DFS(i,j):
            # 位置有效且 当且位置是 "O"     
            # 标记为 'A' 并且向外遍历
            if 0<= i <n and 0<=j<m and board[i][j]== 'O':
                board[i][j] = 'A'
                DFS(i+1,j)
                DFS(i-1,j)
                DFS(i,j+1)
                DFS(i,j-1)
                return
            else:
                return
        # 遍历边界上的
        for i in range(n):
            if board[i,0] == "O":
                DFS(i,0)
            if board[i,m] == "O":
                DFS(i,m)
        for j in range(m):
            if board[0,j] == "O":
                DFS(0,j)
            if board[n,j] == "O":
                DFS(n,j)
        for i in range(n):
            for j in range(m):
                if board[i][j] == "A":
                    board[i][j] = "O"
                else:
                    board[i][j] = "X"
        return board       
            
        
```

但是题解的答案明显更简洁，所以还是贴一下。


```python
class Solution:
    def solve(self, board):
        # 首先检查是否是空节点
        # 在自己写的代码里没有注意到
        if not board:
            return
        # 然后分别取出宽度和长度
        n, m = len(board), len(board[0])
        # 定义深度优先搜索，标记和当前 'O' 直接或间接相连的 'O' 为 'A'
        def dfs(x, y):
            # 当位置索引无效或当前位置为不为 'O' 时
            # 直接返回
            if not 0 <= x < n or not 0 <= y < m or board[x][y] != 'O':
                return
            # 其余情况，标记当前位置为 'A'
            board[x][y] = "A"
            # 遍历附近的位置
            dfs(x + 1, y)
            dfs(x - 1, y)
            dfs(x, y + 1)
            dfs(x, y - 1)
        
        # 对第一行和最后一行标记
        for i in range(n):
            dfs(i, 0)
            dfs(i, m - 1)
        # 对第一列和最后一列标记
        for i in range(m - 1):
            dfs(0, i)
            dfs(n - 1, i)
        
        # 对标记地点改为 'O'
        # 对其余的 'O' 改为 'X'
        for i in range(n):
            for j in range(m):
                if board[i][j] == "A":
                    board[i][j] = "O"
                elif board[i][j] == "O":
                    board[i][j] = "X"


```

#### 1254 统计封闭岛屿的数目
https://leetcode.cn/problems/number-of-closed-islands/

这里选择用 200 岛屿数量 和 130 被包围的区域 的代码结合一下。先把跟边界相连的岛屿标记掉，然后数出当前岛屿的数量。


```python
class Solution:
    def closedIsland(self, grid):
        # 首先检查是否是空节点
        # 在自己写的代码里没有注意到
        if not grid:
            return
        # 然后分别取出宽度和长度
        n, m = len(grid), len(grid[0])
        def dfs(x, y):
            # 当位置索引无效或当前位置为不为 'O' 时
            # 直接返回
            if not 0 <= x < n or not 0 <= y < m or grid[x][y] != 0:
                return
            # 当前位置为 0 时，标记为 1，并在周围遍历
            grid[x][y] = 1
            # 遍历附近的位置
            dfs(x + 1, y)
            dfs(x - 1, y)
            dfs(x, y + 1)
            dfs(x, y - 1)
        
        # 先对边界处的非封闭岛屿进行标记
        for i in range(n):
            dfs(i, 0)
            dfs(i, m - 1)
        for i in range(m - 1):
            dfs(0, i)
            dfs(n - 1, i)
    
        num = 0
        # 对标记地点改为 'O'
        # 对其余的 'O' 改为 'X'
        for i in range(n):
            for j in range(m):
                if grid[i][j] == 0:
                    dfs(i, j)
                    num += 1
        return num


```


```python
grid = [[0,0,1,0,0],[0,1,0,1,0],[0,1,1,1,0]]
solution = Solution()
solution.closedIsland(grid)
```




    1



#### 417 太平洋大西洋水流问题
https://leetcode.cn/problems/pacific-atlantic-water-flow/description/

如果周边高度小于当前高度，则水流可以流出。如果水流到了上边界或者左边界，则说明能流到 Pacific ，要是水流到了右边界或者下边界，则说明能流到 Atlantic。但是在递归函数中保存和传递最初的位置是否能流入 Pacific，Atlantic 的信息非常麻烦。所以采用反向思维，直接从 Pacific, Atlantic 的边界向岛内标记当前位置是否能到 Pacific, Atlantic。 这就方便多了。

##### 反向思维！！！


```python
class Solution:
    def pacificAtlantic(self, heights):
        m, n = len(heights), len(heights[0])
        
        # 定义回溯的代码
        def search(starts):
            # 设定一个集合
            visited = set()
            
            
            # 定义深度优先搜索
            def dfs(x, y):
                # 如果位置被访问了，则返回
                # 注意这里创建的是元组 (x,y) 
                if (x, y) in visited:
                    return
                # 否则加入访问集合
                visited.add((x, y))
                # 对于周围的格子，如果海拔不低于海平面且比当前位置海拔高
                # 则接着标记
                for nx, ny in ((x, y + 1), (x, y - 1), (x - 1, y), (x + 1, y)):
                    if 0 <= nx < m and 0 <= ny < n and heights[nx][ny] >= heights[x][y]:
                        dfs(nx, ny)
                        
            # 对 starts 列表中的每个点，开始回溯            
            for x, y in starts:
                dfs(x, y)
            return visited

        pacific = [(0, i) for i in range(n)] + [(i, 0) for i in range(1, m)]
        atlantic = [(m - 1, i) for i in range(n)] + [(i, n - 1) for i in range(m - 1)]
        return list(map(list, search(pacific) & search(atlantic)))

```

关于最后一行的解释

在这段代码中，`list(map(list, search(pacific) & search(atlantic)))` 的作用是将两个集合(`search(pacific)` 和 `search(atlantic)`)的交集转换为列表的列表格式。具体步骤如下：

1. **集合交集 (`search(pacific) & search(atlantic)`)**:
   - `search(pacific)` 和 `search(atlantic)` 分别对太平洋和大西洋边缘出发点执行深度优先搜索(DFS)，标记可以到达各自海洋的所有网格位置。这两个函数返回的是所有可达网格位置的集合。
   - 使用 `&` 操作符计算两个集合的交集，即找到所有既能从太平洋又能从大西洋到达的网格位置。

2. **`map(list, ...)`**:
   - 由于交集得到的是一个包含元组的集合，如 `((x1, y1), (x2, y2), ...)`, 而题目要求返回的是二维列表，如 `[[x1, y1], [x2, y2], ...]`。
   - `map()` 函数接收两个参数：一个函数 `f` 和一个可迭代对象 `iterable`，并返回一个将 `f` 应用于 `iterable` 中每一项元素后得到的新迭代器。
   - 在这里，`list` 作为函数 `f`，被应用到交集中的每一个元组上，将其转换为列表。

3. **最外层的 `list(...)`**:
   - 最后，由于 `map()` 返回的是一个迭代器，为了获得实际的列表数据，需要将其转换为列表。因此，最外层的 `list()` 将 `map` 迭代器转换为具体的列表。

综上所述，这一行代码的目的是找出所有同时能流向太平洋和大西洋的网格坐标，并以题目要求的二维列表形式返回这些坐标。

#### 1020 飞地的数量
https://leetcode.cn/problems/number-of-enclaves/description/


```python
# 结合前面的思路，把跟边界相连的地方删除
# 再输出 1 的个数即可
class Solution(object):
    def numEnclaves(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        n = len(grid)
        m = len(grid[0])
        def dfs(x,y):
            if not 0<=x<n or not 0<=y<m or grid[x][y] != 1:
                return
            grid[x][y] = 0
            dfs(x+1,y)
            dfs(x-1,y)
            dfs(x,y+1)
            dfs(x,y-1)

        for i in range(n):
            dfs(i,0)
            dfs(i,m-1)
        for j in range(m):
            dfs(0,j)
            dfs(n-1,j)

        count = 0

        for i in range(n):
            for j in range(m):
                if grid[i][j] == 1:
                    count += 1
        return count
                
```
