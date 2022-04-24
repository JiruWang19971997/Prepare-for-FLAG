## 两数之和，简单模板
 递归写法
- 确定dfs递归的路径记录矩阵：combinations
- 记录矩阵在本地dfs执行结束后，要还原：combinations.pop()
- 每次递归需更新原始数值或matrix
迭代写法
- 定义visited矩阵，记录执行过的路径
- 使用stack
### 39. Combination Sum
#### media
```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        if len(candidates) == 0:
            return [[]]
        results = []
        self.dfs(candidates, target, 0, [], results)
        return results
    
    def dfs(self, candidates, target, idx, combinations, results):
        if target < 0:
            return
        if target == 0:
            return results.append(list(combinations))
        for i in range(idx, len(candidates)):
            combinations.append(candidates[i])
            self.dfs(candidates, target - candidates[i], i, combinations, results)
            combinations.pop()
```



## 图，寻找路径
### 79. Word Search
#### media
```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        if len(board) == 0 or len(board[0]) == 0:
            return []
        for i in range(len(board)):
            for j in range(len(board[0])):
                if self.dfs(board, i, j, word, 0):
                    return True
        return False
    
    def dfs(self, board, i, j, word, index):
        if index == len(word):
            return True
        if i < 0 or i >= len(board) or j < 0 or j >= len(board[0]) or board[i][j] != word[index]:
            return False
        
        board[i][j] = 'used'
        found = self.dfs(board, i  + 1, j, word, index + 1) or self.dfs(board, i  - 1, j, word, index + 1)\
        or self.dfs(board, i, j + 1, word, index + 1) or self.dfs(board, i, j - 1, word, index + 1)
        board[i][j] = word[index]
        return found
```

### 212. Word Search II
#### hard
构建trie树（指针，字典）   
遍历trie树
```python
class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        if len(board) == 0 or len(board[0]) == 0 or len(words) == 0:
            return []
        # build trie
        trie = dict()
        for word in words:
            t = trie
            for c in word:
                if c not in t.keys():
                    t[c] = dict()
                t = t[c]
            t['#'] = None #结束点
        
        # dfs
        res = set()
        for i in range(len(board)):
            for j in range(len(board[0])):
                if board[i][j] not in trie.keys():
                    continue
                self.dfs(board, trie, i, j, '', res)
        return res
        
    def dfs(self, board, trie, i, j, word, res):
        # 超出范围
        
        if i < 0 or i >= len(board) or j < 0 or j >= len(board[0]):
            return
        
        c = board[i][j]
        
        # 不是这一层的节点
        if c not in trie:
            return
        
        if '#' in trie[c]:
            res.add(word + c)
            if len(trie[c]) == 1:
                return
        board[i][j] = '##'
        self.dfs(board, trie[c], i + 1, j, word + c, res)
        self.dfs(board, trie[c], i - 1, j, word + c, res)
        self.dfs(board, trie[c], i, j + 1, word + c, res)
        self.dfs(board, trie[c], i, j - 1, word + c, res)
        board[i][j] = c
```

## 图，小岛问题
### 419. Battleships in a Board
## media
```python
class Solution:
    def countBattleships(self, board: List[List[str]]) -> int:
        if len(board) == 0 or len(board[0]) == 0:
            return 0
        count = 0
        for i in range(len(board)):
            for j in range(len(board[0])):
                if board[i][j] == '.':
                    continue
                count += 1
                self.dfs(board, i, j)
        return count
        
    def dfs(self, board, i, j):
        if i < 0 or i >= len(board) or j < 0 or j >= len(board[0]):
            return
        if board[i][j] == '.':
            return
        board[i][j] = '.'
        self.dfs(board, i + 1, j)
        self.dfs(board, i - 1, j)
        self.dfs(board, i, j + 1)
        self.dfs(board, i, j - 1)
```
## 图论，搜索
### 210. Course Schedule II
#### media
bfs
```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        graph = {i:[] for i in range(numCourses)}
        in_degree = [0 for i in range(numCourses)]
        for course_id, pre_id in prerequisites:
            graph[pre_id].append(course_id)
            in_degree[course_id] += 1
        queue = []
        for i in range(numCourses):
            if in_degree[i] == 0:
                queue.append(i)
        res = []
        while len(queue) > 0:
            course_id = queue.pop(0)
            res.append(course_id)
            for idx in graph[course_id]:
                in_degree[idx] -= 1
                if in_degree[idx] == 0:
                    queue.append(idx)
        if len(res) < numCourses:
            return []
        if len(res) == numCourses:
            return res
```
dfs
```python
# dfs
        pre_list = [[] for i in range(numCourses)]
        for course_id, pre_id in prerequisites:
            pre_list[course_id].append(pre_id)
        res = []
        visited = [-1 for i in range(numCourses)]
        # 倒序法，遍历每一个点，确认这个点是否可以通过pre course走到，如果可以走到，再遍历下一个点是否可以走到
        for course_idx in range(numCourses):
            if not self.dfs(pre_list, visited, course_idx, res):
                return []
        return res
            
    def dfs(self, pre_list, visited, course_idx, res):
        if visited[course_idx] == 1:
            return True
        if visited[course_idx] == 0:
            return False
        visited[course_idx] = 0
        for idx in pre_list[course_idx]:
            if not self.dfs(pre_list, visited, idx, res):
                return False
        # 如果先修课节点全部修过，visited == 1
        visited[course_idx] = 1
        res.append(course_idx)
        return True
```





## 应用题
抽象当前状态到下一状态得每一步,以及每个状态
### 365. Water and Jug Problem
#### media
用迭代写dfs
- 普通写法
```python
class Solution:
    def canMeasureWater(self, jug1Capacity: int, jug2Capacity: int, targetCapacity: int) -> bool:
        # dfs, iteration
        stack = [(0, 0)]
        seen = set()
        while len(stack) > 0:
            jug1, jug2 = stack.pop()
            if jug1 + jug2 == targetCapacity or jug1 == targetCapacity or jug2 == targetCapacity:
                return True
            if (jug1, jug2) in seen:
                continue
            if (jug1, jug2) in seen:
                continue
            seen.add((jug1, jug2))
            stack.append((jug1Capacity, jug2)) # jug1倒满
            stack.append((0, jug2)) # jug1倒光
            stack.append((max(jug1 - (jug2Capacity - jug2),0), min(jug2Capacity,jug2 + jug1))) # jug1倒入jug2
            stack.append((jug1, jug2Capacity))
            stack.append((jug1, 0))
            stack.append((min(jug1Capacity,jug1 + jug2), max(0, jug2 - (jug1Capacity - jug1)))) #jug2倒入jug1
        return False
```
- follow up
```python
class Solution:
    def canMeasureWater(self, x: int, y: int, z: int) -> bool:
        if x == z or y == z:
            return True
        if x + y < z:
            return False
        if (x == 0 or y == 0) and z > 0:
            return False
        
        # dfs, iteration
        stack = [0]
        seen = set()
        x, y = min(x, y), max(x, y)
        while len(stack) > 0:
            state = stack.pop()
            seen.add(state)
            if state == z or state + x == z:
                return True
            
            # x加满，倒入y,x被清空
            s = state + x
            if s not in seen and s <= y:
                stack.append(s)
            
            # y倒入x，x被加满
            s = state - x
            if s not in seen and s > 0:
                stack.append(s)
            
            # x清空，y倒入x，y加满，y倒入x，x清空
            s = y - (x - state)
            if s not in seen and s > 0 and s <= y:
                stack.append(s)
            
            # x加满，x倒入y，清空y，x倒入y
            s = x - (y - state)
            if s not in seen and s > 0 and s <= y:
                stack.append(s)
        return False
```











### 329. Longest Increasing Path in a Matrix
#### hard
https://www.youtube.com/watch?v=wCc_nd-GiEc&t=447s
```python
class Solution:
    def __init__(self):
        self.cache = dict()
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        n = len(matrix)
        m = len(matrix[0])
        max_len =  0
        for i in range(n):
            for j in range(m):
                print(i, j)
                ll = self.dfs(matrix, i, j, -1)
                max_len = max(ll, max_len)
        return max_len
    
    def dfs(self, matrix, i, j, pre):
        n = len(matrix)
        m = len(matrix[0])
        if i < 0 or i >= n or j < 0 or j >= m or matrix[i][j] <= pre:
            return 0
        if (i, j) in self.cache:
            return self.cache.get((i, j))
        cur = matrix[i][j]
        l1 = self.dfs(matrix, i + 1, j, cur)
        l2 = self.dfs(matrix, i, j + 1, cur)
        l3 = self.dfs(matrix, i - 1, j, cur)
        l4 = self.dfs(matrix, i, j - 1, cur)
        res = max(l1, l2, l3, l4) + 1
        self.cache[(i, j)] = res
        return res
# dfs迭代, 超时, 很迷
#     def dfs(self, matrix, i, j):
#         l_x = len(matrix)
#         l_y = len(matrix[0])
#         directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
#         stack = [[1, (i, j)]]
#         step_long = 0
#         while len(stack) > 0:
#             step, axis = stack.pop()
#             step_long = max(step_long, step)
#             for x, y in directions:
#                 n_i = axis[0] + x
#                 n_j = axis[1] + y
                
#                 if n_i < 0 or n_i >= l_x or n_j < 0 or n_j >= l_y or matrix[n_i][n_j] <= matrix[axis[0]][axis[1]]:
#                     continue
#                 stack.append([step + 1, (n_i, n_j)])
#         return step_long
```
### 417. Pacific Atlantic Water Flow
#### media
注意for循环的m和n
```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        n = len(heights)
        m = len(heights[0])
        self.visited_p = [[False] * m for i in range(n)]
        self.visited_a = [[False] * m for i in range(n)]
        for i in range(m):
            self.dfs(heights, 0, i, - 1, p_or_a = 'p')
            self.dfs(heights, n - 1, i, - 1, p_or_a = 'a')
        for j in range(n):
            self.dfs(heights, j, 0, - 1, p_or_a = 'p')
            self.dfs(heights, j, m - 1, - 1, p_or_a = 'a')
        res = []
        for i in range(n):
            for j in range(m):
                if self.visited_p[i][j] and self.visited_a[i][j]:
                    res.append([i, j])
        return res
        
    def dfs(self, heights, i, j, pre, p_or_a = 'p'):
        if i < 0 or i >= len(heights) or j < 0 or j >= len(heights[0]) or heights[i][j] < pre:
            return
        if p_or_a == 'p':
            if self.visited_p[i][j] == True:
                return
        if p_or_a == 'a':
            if self.visited_a[i][j] == True:
                return
        
        if p_or_a == 'p':
            self.visited_p[i][j] = True
        if p_or_a == 'a':
            self.visited_a[i][j] = True
        cur = heights[i][j]
        
        self.dfs(heights, i + 1, j, cur, p_or_a)
        self.dfs(heights, i - 1, j, cur, p_or_a)
        self.dfs(heights, i, j + 1, cur, p_or_a)
        self.dfs(heights, i, j - 1, cur, p_or_a)
```
