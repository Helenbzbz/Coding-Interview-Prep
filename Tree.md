#### Knowledge Check
* **Overview**
	* ***Binary Trees***
		* *Root Nodes*: node without parents
		* *Leaf Nodes*: node without children (BTS always has leaf nodes)
		* *Heights*: from this node to furthest decedents
		* *Depth*: From this node to root node
	* ***Binary Search Tree***
		* Left Child Value < Root Value < Right Child Value
		* ***Search*: $O(h)$**
		```
		def search(root, target):
			if not root:
				return False
			if target > root.val:
				return search(root.right, target)
			elif target < root.val:
				return search(root.left, target)
			else:
				return True
		```
		* ***Insert(root, 6)*: $O(logn)$**
		```
		def insert(root, val):
			if not root:
				return Node(val)
			if val > root.val:
				root.right = insert(root.right, val)
			elif val < root.val:
				root.left = insert(root.left, val)
			return root
		```
		* ***remove(root, 6)*:**
			* *Base Case*: 0 or 1 child, remove directly and shift the child up
			* *2 Children:* Find smallest on the right, replace this to be the current value and remove this from the right side
		```
		def findmin(root):
			curr = root
			while curr and curr.left:
				curr = curr.left
			return curr
			
		def remove(root, val):
			if not root:
				return None
			if val > root.val:
				root.right = remove(root.right, val)
			elif val < root.val:
				root.right = remove(root.left, val)
			else:
				if not root.left:
					return root.right
				elif not root.right:
					return root.left
				else:
					minnode = findmin(root)
					root.val = minnode.val
					root.right = remove(root.right, minnode.val)
			return root
		```

* **Operations**
	* **BFS**
		* *Basic Steps*
			* Initialize: Search from root
			* Adjacent Nodes: Level by level
			* Stopping: When desired found/ all nodes explored
		* *Used When:*
			* Tree-structures
			* Not a wide tree
			* Level by level 
			* Solution near root
		* *Examples*: `Minimum_Depth, Bottom_Up_Level_Order_Traversal, Level_Order_Traversal`
	* **DFS**
		* *Types*
			* Preorder: root, left, right
			* In-order: left, root, right
			* Post-order: left, right, root
		* *Used When*:
			* Tree-structures
			* Balanced/ Low-Branching
			* Hierarchical
			* Solution near leaves
			* Travel along leaves paths + all possible paths
		* *Examples:* `Path_Sum, Valid_BST, Serialize_and_Deserialize, Maximum_Path_Sum, Reconstruct_from_Preorder_Inorder, Invert_BST, Kth_Smallest_Element_in_BST, Lowest_Common_Ancestors, Maximum_Depth_of_Binary_Tree, Same_Tree, Subtree_of_Another_Tree, Valid_BST`
	
* **More Advanced Topics**
	* [[Trie]]
	* [[Union-Find]]

#### Practice
###### **LeetCode 104. Maximum Depth of Binary Tree**
#Tree #DFS #BFS 
```
def maxDepth(self, root):
	if not root:
		return 0
	return max(self.maxDepth(root.left), self.maxDepth(root.right))+1
```
###### **LeetCode 100. Same Tree**
#Tree #DFS #BFS 
```
def isSameTree(self, p, q):
	if not p and not q:
		return True
	if not p or not q or p.val != q.val:
		return False
	return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```
###### **LeetCode 102. Binary Tree Level Order Traversal**
#Tree #BFS 
```
def levelOrder(self, root):
	if not root:
		return []
	queue = deque([root])
	res = []
	
	while queue:
		cur_level = []
		for _ in range(len(queue)):
			cur_node = queue.popleft()
			cur_level.append(cur_node.val)
			if cur_node.left:
				queue.append(cur_node.left)
			if cur_node.right:
				queue.append(cur_node.right)
		res.append(cur_level)
	return res
```

###### **LeetCode 199. Binary Tree Right Side View**
#Tree #DFS #BFS 
```
def rightSideView(self, root):
	if not root:
		return []
	queue = deque([root])
	res = []
	while stack:
		for _ in range(len(queue)):
			cur_node = queue.popleft()
			if cur_node.left:
				queue.append(cur_node.left)
			if cur_node.right:
				queue.append(cur_node.right)
		res.append(cur_node.val)
	return res
```

###### **LeetCode 297. Serialize and Deserialize Binary Tree**
#DFS #BFS #Tree 
```
class Codec:
	def serialize(self, root):
		if root is None:
			return '.,'
		return  str(root.val) + ',' + self.serialize(root.left) + self.serialize(root.right)

	def deserialize(self, data):
		def reconstruct(l):
			if l[0] == '.':
				l.pop(0)
				return None
			root = TreeNode(int(nodes.pop(0)))
			root.left = reconstruct(l)
			root.right = reconstruct(l)
			return root
		data = data.split(',')
		return reconstruct(data)
```

###### **LeetCode 124. Binary Tree Maximum Path Sum**
#DP #Tree #DFS 
```
def maxPathSum(self, root):
	maximum = float('-inf')
	
	def dfs(node):
		nonlocal maximum
		if not node:
			return 0
		left_val = dfs(node.left)
		right_val = dfs(node.right)

		maximum = max(maximum, left_val + node.val + right_val)
		return max(node.val + left_val, node.val + right_val, 0)
		
	dfs(root)
	return maximum
```

###### **LeetCode 543. Diameter of Binary Tree**
#Tree #DFS 
```
def diameterOfBinaryTree(self, root):
	res = 0
	def dfs(node):
		nonlocal res
		if not node:
			return 0
		l, r = dfs(node.left), dfs(node.right)
		res = max(res, l + r)
		return max(l, r) + 1
	dfs(root)
	return res
```

###### **LeetCode 98. Validate Binary Search Tree**
#Tree #DFS #BFS 
```
def isValidBST(self, root):
	def dfs(node, low = float('-inf'), high = float('inf')):
		if not node:
			return True
		if node.val <= low or node.val >= high:
			return False
		return dfs(node.left, low, node.val) and dfs(node.right, node.val, high)
	return dfs(root)
```


###### **Meta 2. Nodes in a Subtree**
#Tree #DFS 
```
class Node:
	def __init__(self, val):
		self.val = val
		self.children = []

def countOfNodes(root, queries, s):
	counts = defaultdict(lambda: defaultdict(int))
	def dfs(node):
		char = s[node.val-1] 
		counts[node.val][char] += 1
		for c in node.children:
			c_count = dfs(c)
			for k, count in c_count.items():
				counts[node.val][k] += count
		return counts[node.val]
		
	dfs(root)
	res = []
	for u, c in queries:
		res.append(counts[u][c])
	return res
```