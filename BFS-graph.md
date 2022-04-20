## bfs&dfs: https://www.youtube.com/user/tpof314/search?query=bfs
### 429. N-ary Tree Level Order Traversal
#### media
https://www.youtube.com/watch?v=rdOULhA5-Q4
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""
class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        if not root:
            return []
        queue = [[root, 0]]
        res = dict()
        res_l = []
        while len(queue) > 0:
            node, level = queue.pop(0)
            cur_l = res.get(level, list())
            cur_l.append(node.val)
            res[level] = cur_l
            for next_node in node.children:
                queue.append([next_node, level + 1])
        res = list(sorted(res.items(), key = lambda x:x[0]))
        for _, vals in res:
            res_l.append(vals)
        return res_l
```



### 863. All Nodes Distance K in Binary Tree
#### media
https://www.youtube.com/watch?v=pHdl1QOtf1g&t=1448s   
包含：dfs树转图，bfs遍历图
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def __init__(self):
        self.graph = dict()
        
    def distanceK(self, root: TreeNode, target: TreeNode, k: int) -> List[int]:
        if not root.left and not root.right:
            if k == 0:
                return [root.val]
            else:
                return []
        # dfs build graph
        self.build_graph(root, None)
        # bfs 
        res = []
        seen = set()
        queue = [[target, 0]]
        while len(queue) > 0:
            node, level = queue.pop(0)
            if level > k:
                break
            if node in seen:
                continue
            seen.add(node)
            if level == k:
                res.append(node.val)
            else:
                level += 1
                for c_node in self.graph.get(node):
                    queue.append([c_node, level])
        return res
        
    
    
    def build_graph(self, node, parent):
        if not node:
            return
        if parent:
            node_list = self.graph.get(node, list())
            node_list.append(parent)
            self.graph[node] = node_list

        if node.left:
            node_list = self.graph.get(node, list())
            node_list.append(node.left)
            self.graph[node] = node_list
            self.build_graph(node.left, node)
        if node.right:
            node_list = self.graph.get(node, list())
            node_list.append(node.right)
            self.graph[node] = node_list
            self.build_graph(node.right, node)
        
            
```
### 127. Word Ladder
#### hard
https://www.youtube.com/watch?v=0fzUFMpGLMU&t=389s
```python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        if endWord not in wordList:
            return 0
        wordset = set(wordList)
        queue = [[beginWord, 0]]
        while len(queue) > 0:
            word, level = queue.pop(0)
            if word == endWord:
                return level + 1
            level += 1
            gen_word = self.change_one(word)
            for word_g in gen_word:
                if word_g in wordset and word_g != word:
                    wordset.remove(word_g)
                    queue.append([word_g, level])
        return 0

    def change_one(self, word):
        for idx, char in enumerate(list(word)):
            for i in range(ord('a'), ord('z') + 1):
                word_new = word[:idx] + chr(i) + word[idx+1:]
                yield word_new
                
```

### 778. Swim in Rising Water
#### hard
https://www.youtube.com/watch?v=amvrKlMLuGY    
dijkstra 算法
```python
class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        N = len(grid)
        seen = set()
        minH = [[grid[0][0], 0, 0]]
        directions = [[0, 1], [0, -1], [1, 0], [-1, 0]]
        seen.add((0, 0))
        while minH:
            t, r, c = heapq.heappop(minH)
            seen.add((r, c))
            if r == N - 1 and c == N - 1:
                return t
            for dr, dc in directions:
                nr, nc = dr + r, dc + c
                if (nr < 0) or (nc < 0) or (nr >= N) or ((nr, nc) in seen) or (nc >= N):
                    continue
                seen.add((nr, nc))
                heapq.heappush(minH, [max(t, grid[nr][nc]), nr, nc])
```
### 847 
#### hard
https://www.youtube.com/watch?v=Vo3OEN2xgwk  
seen：记录所有组合   

```python
class Solution:
    def shortestPathLength(self, graph: List[List[int]]) -> int:
        n = len(graph)
        queue = [[i, 0, 1 << i] for i in range(n)]
        seen = [set() for i in range(n)] # 记录路径到达每一个node时所经过的点，初始化只经过自己
        while queue:
            node, level, state = queue.pop(0)
            if state == (1 << n) - 1: # 到达当前节点，其路径已遍历所有节点
                return level
            if state in seen[node]:
                continue
            seen[node].add(state)
            print(seen)
            for nei in graph[node]:
                queue.append([nei, level + 1, state | 1 << nei])
```
