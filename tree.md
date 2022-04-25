# 定义
二叉树：最多两个child节点   
二叉搜索树：左节点小，有节点大于root    
pre order:root left right
in order: left, root, right   
post order: left, right, root




## 基础：树的递归
### 116. Populating Next Right Pointers in Each Node
#### media

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""

class Solution:
    def connect(self, root: 'Optional[Node]') -> 'Optional[Node]':
        if (not root) or (not root.left and not root.right):
            return root
        root.left.next = root.right
        if root.next:
            root.right.next = root.next.left
        self.connect(root.left)
        self.connect(root.right)
        return root
```

### 98. Validate Binary Search Tree
#### media
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        return self.valid(root, float('-inf'), float('inf'))
        
    def valid(self, root, min_v, max_v):
        if not root:
            return True
        if root.val >= max_v or root.val <= min_v:
            return False
        bool_v = self.valid(root.left, min_v, root.val) and self.valid(root.right, root.val, max_v)
        return bool_v
 ```





## 重要！**重建二叉树**
### 105. Construct Binary Tree from Preorder and Inorder Traversal
#### media
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder or not inorder:
            return None
        rootValue = preorder[0]
        root = TreeNode(rootValue)
        rootIndex = inorder.index(rootValue)
        root.left = self.buildTree(preorder[1:rootIndex + 1], inorder[:rootIndex])
        root.right = self.buildTree(preorder[rootIndex + 1:], inorder[rootIndex + 1:])
        return root
```

## 树的bfs
### 103. Binary Tree Zigzag Level Order Traversal
#### media
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def zigzagLevelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        # bfs
        if not root:
            return []
        queue = [[root, 0]]
        res = []
        while queue != []:
            node, level = queue.pop(0)
            if len(res) < level + 1:
                res.append([])
            res[level].append(node.val)
            if node.left:
                queue.append([node.left, level + 1])
            if node.right:
                queue.append([node.right, level + 1])
        for idx, node_list in enumerate(res):
            if idx % 2 == 1:
                res[idx] = res[idx][::-1]
        return res
```