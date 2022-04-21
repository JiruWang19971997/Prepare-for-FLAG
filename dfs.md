
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
