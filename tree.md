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

### 22. Generate Parentheses
#### media
```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        if n == 0:
            return []
        result = []
        self.helper(n, n, '', result)
        return result
    
    def helper(self, l, r, item, result):
        # 不成立：(())),即item中右括号数量大于左括号，后续不管再放入什么都无法成立
        if l > r:
            return
        if l == 0 and r == 0:
            result.append(item)
            return
        if l > 0:
            self.helper(l - 1, r, item + '(', result)
        if r > 0:
            self.helper(l, r-1, item + ')', result)
```
### 100. Same Tree
#### easy
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if p is None and q is None:
            return True
        return self.helper(p, q)

    def helper(self, p, q):
        if p is None and q is None:
            return True
        if p is not None and q is None:
            return False
        if p is None and q is not None:
            return False
        bool_res = (p.val == q.val) and self.helper(p.left, q.left) and self.helper(p.right, q.right)
        return bool_res
```
### 101. Symmetric Tree
#### easy
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if root is None:
            return True
        return self.helper(root.left, root.right)
    def helper(self, left, right):
        if left == None and right == None:
            return True
        if left is None or right is None or left.val != right.val:
            return False
        
        bool_res = self.helper(left.left, right.right) and self.helper(left.right, right.left)
        return bool_res
```




### 104. Maximum Depth of Binary Tree
#### easy
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:


def maxDepth(self, root: Optional[TreeNode]) -> int:
        if root is None:
            return 0
        
        return self.helper(root)
    
    def helper(self, root):
        if root is None:
            return 0
        depth = max(self.helper(root.left), self.helper(root.right)) + 1
        return depth
```

### 108. Convert Sorted Array to Binary Search Tree
#### easy
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if nums == []:
            return None
        
        root = self.helper(nums)
        return root
    
    def helper(self, nums):
        if len(nums) == 0:
            return None #叶子节点
        mid = len(nums) // 2
        node = TreeNode(nums[mid])
        node.left = self.helper(nums[:mid])
        node.right = self.helper(nums[mid+1:])
        return node
```
### 110. Balanced Binary Tree
#### easy
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if root is None:
            return True
        height = self.helper(root)
        return height >= 0
        
        
    def helper(self, root):
        if root is None:
            return 0
        left_height = self.helper(root.left)
        right_height = self.helper(root.right)
        if left_height == -1 or right_height == -1:
            return -1
        if abs(left_height - right_height) > 1:
            return -1
        else:
            return max(left_height,right_height) + 1
```

### 111. Minimum Depth of Binary Tree
#### easy
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        return self.helper(root)
        
        
    def helper(self, root):
        if root == None:
            return 0
        left_height = self.helper(root.left)
        right_height = self.helper(root.right)
        if root.left and root.right:
            return min(left_height, right_height) + 1
        else:
            return max(left_height, right_height) + 1
        
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

