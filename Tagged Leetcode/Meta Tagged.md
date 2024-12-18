###### 1650 Lowest Common Ancestor of a Binary Tree III
#Tree 
* Find the depth of nodes `p` and `q`
* Bring the deeper to same level as the other
* Traverse together until they meet
```
class Solution:
	def lowestCommonAncestor(self, p: 'Node', q: 'Node') -> 'Node':
		def find_depth(n):
			level = 0
			while n.parent:
				level += 1
				n = n.parent
			return level
	
		pd, qd = find_depth(p), find_depth(q)
		while pd < qd:
			qd -= 1
			q = q.parent
		while qd < pd:
			pd -= 1
			p = p.parent
	
		while p!= q:
			p, q = p.parent, q.parent
		return p
```

###### 236. Lowest Common Ancestor of a Binary Tree
#Tree 
```
class Solution:
	def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
		if not root:
			return None
		if root == p or root == q:
			return root
		l = self.lowestCommonAncestor(root.left, p, q)
		r = self.lowestCommonAncestor(root.right, p, q)
		if l and r:
			return root
		return l or r
```

###### 50. Pow(x, n)
* Recursion, and can be optimized if followed by a odd/even power handle
```
class Solution:
	def myPow(self, x: float, n: int) -> float:
		if n == 0:
			return 1
		if n < 0:
			return 1/self.myPow(x, -n)
		if n % 2 == 0:
			multi = self.myPow(x, n//2)
			res = 1
		else:
			multi = self.myPow(x, (n-1)//2)
			res = x
		return res * multi * multi
```
###### 426. Convert BST to Sorted Doubly Linked List
```
class Solution:
	def treeToDoublyList(self, root: 'Optional[Node]') -> 'Optional[Node]':
		if not root:
			return root
		head, prev = None, None
		
		def dfs(node):
			nonlocal head, prev
			if not node:
				return
			dfs(node.left)
			if prev:
				// this is in middle, we link prev and node together
				prev.right, node.left = node, prev
			else:
				// no prev, leftest
				head = node
			prev = node
			dfs(node.right)

		dfs(root)
		head.left, prev.right = prev, head
		return head
```

