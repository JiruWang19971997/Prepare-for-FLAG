# 定义
二叉树：最多两个child节点




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
