#### Recent 30 Days
###### 329. Longest Increasing Path in a Matrix
```
def longestIncreasingPath(self, matrix):
	def dfs(r, c):
		if cache[r][c] != 1:
			return cache[r][c]
		for dr, dc in directions:
			nr, nc = dr+r, dc+c
			if 0 <= nr < R and 0 <= nc < C and matrix[nr][nc] > matrix[r][c]:
				cache[r][c] = max(dfs(nr, nc)+1, cache[r][c])
		return cache[r][c]

	R, C = len(matrix), len(matrix[0])
	cache = [[1 for i in range(C)] for j in range(R)]
	directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
	res = 0
	for i in range(R):
		for j in range(C):
			res = max(dfs(i, j), res)
	return res
```

###### 286. Walls and Gates
```
def wallsAndGates(self, rooms):
	R, C = len(rooms), len(rooms[0])
	queue = deque([])
	for i in range(R):
		for j in range(C):
			if rooms[i][j] == 0:
				queue.append((i, j))
	directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
	while queue:
		for _ in range(len(queue)):
			r, c = queue.popleft()
			for dr, dc in directions:
				nr, nc = dr+r, dc+c
				if 0 <= nr < R and 0 <= nc < C and rooms[nr][nc] == 2147483647:
					rooms[nr][nc] = rooms[r][c] + 1
					queue.append((nr, nc))
	return rooms
```

###### 658. Find K Closest Elements
```
def findClosestElements(self, arr, k, x):
	## Initialize pointers to 0 and start of last possible k length subarray
	l, r = 0, len(arr)-k
	while l < r:
		mid = (l+r)//2
		## The left side is further, arr will start at least at mid+1
		if x-arr[mid] > arr[mid+k]-x: 
			l = mid+1
		else:
			r = mid
	return arr[l:l+k]
```

###### 124. Binary Tree Maximum Path Sum
```
def maxPathSum(self, root):
	def dfs(node):
		nonlocal res
		if not node:
			return 0
		l, r = max(dfs(node.left), 0), max(dfs(node.right), 0)
		res = max(res, l+r+node.val)
		return max(l, r) + node.val
	res = float('-inf')
	dfs(root)
	return res
```

###### 875. Koko Eating Bananas
```
def minEatingSpeed(self, piles, h):
	def hour(k):
		res = 0
		for p in piles:
			res += math.ceil(p/k)
		return res
	l, r = 1, max(piles)
	while l <= r:
		mid = (l+r)//2
		if hour(mid) > h:
			l = mid+1
		else:
			r = mid-1
	return l
```

###### 1235. Maximum Profit in Job Scheduling
```
def jobScheduling(self, startTime, endTime, profit):
	## jobs (s, e, profit); Sort by end time
	jobs = [(s, e, p) for s, e, p in zip(startTime, endTime, profit)]
	jobs.sort(key = lambda x:x[1])
	ends = [x[1] for x in jobs]
	
	## dp & define binsearch
	dp = [0 for i in range(len(jobs)+1)]
	
	def binsearch(start):
		l, r = 0, len(profit)-1
		while l <= r:
			mid = (l+r)//2
			if ends[mid] > start:
				r = mid-1
			else:
				l = mid+1
		return r

	## Through jobs: 1. binsearch on end to find latest e <= s
	## 2. dp[i] = max(dp[i-1], profit + dp[prev])
	for i in range(len(jobs)+1):
		s, e, p = jobs[i-1]
		prev = binsearch(s)+1
		if prev >= 0:
			dp[i] = max(dp[i-1], p+dp[prev])
		else:
			dp[i] = max(dp[i-1], p)
	return dp[-1]
```

###### 1790. Check if One String swap Can make Strings Equal
```
def areAlmostEqual(self, s1, s2):
	diff = [(a, b) for a, b in zip(s1, s2) if a != b]
	return len(diff) == 0 or (len(diff) == 2 and diff[0] == diff[1][::-1])
```

###### 556. Next Greater Element III
```
def nextGreaterElement(self, n):
	n = str(n)
	n = [char for char in n]
	## Find i that n[i] < n[i+1]
	i = len(n)-2
	while i >= 0 and n[i] >= n[i+1]:
		 i -= 1
	if i < 0: return -1
	## Find j that j > i and n[j] first > n[i]
	j = len(n)-1
	while n[j] <= n[i]:
		j -= 1
	## Swap j and i
	n[i], n[j] = n[j], n[i]
	i += 1
	## Reverse things from i + 1 to end
	k = len(n)-1
	while k > i:
		n[i], n[k] = n[k], n[i]
		i += 1
		k -= 1
	res = int(''.join(n))
	return -1 if res > 2**31-1 else res
```

###### 826. Most Profit Assigning Work
```
def maxProfitAssignment(self, difficulty, profit, worker):
	jobs = [(-p, d) for p, d in zip(profit, difficulty)]
	jobs.sort()
	jobs = deque(jobs)
	worker.sort(reverse = True)	
	res = 0
	for w in worker:
		while jobs and jobs[0][1] > w:
		jobs.popleft()
		if jobs:
			res -= jobs[0][0]
		else:
			break
	return res
```

###### 987. Vertical Order Traversal of BT
```
def verticalTraversal(self, root):
	res = defualtdict(list)
	stack = deque([(root, 0, 0)])
	while stack:
		n, col, row = stack.popleft()
		res[col].append((row, n.val))
		if n.left:
			stack.append((n.left, col-1, row+1))
		if n.right:
			stack.append((n.right, col+1, row+1))
	res = [res[key] for key in sorted(res.keys())]
	return [n[1] for n in for sorted(col)] for col in res]
```

###### 1347. Minimum Number of Steps to Make Two Strings Anagram
```
def minSteps(self, s, t):
	char_count = {}
	for c in s:
		char_count[c] = char_count.get(c, 0)+1
	for c in t:
		char_count[c] = char_count.get(c, 0)-1
	### Positive change will hedge negative missings
	return sum([max(v, 0) for v in char_count.values()])
```

###### 1268. Search Suggestions System
```
class Node:
	def __init__(self):
		self.children = {}
		self.word = False

class Trie:
	def __init__(self):
		self.head = Node()
		
	def add(word):
		curr = self.head
		for c in word:
			if c not in curr.children:
				curr.children[c] = Node()
			curr = curr.children[c]
		curr.word = True
	
def suggestedProducts(self, products, searchWord):
	products.sort()
	
	T = Trie()
	for p in products:
		T.add(p)
		
	curr = T.head
	res = []
	
	def dfs(node, prefix):
		if len(curr_res) == 3:
			return
		if node.word:
			curr_res.append(prefix)
		for k in sorted(node.children.keys()):
			dfs(node.children[k], prefix+k)

	prefix = ''
	for c in searchWord:
		curr_res = []
		if c in curr.children:
			curr = curr.children[c]
			prefix += c
			dfs(curr, prefix)
		else:
			break
		res.append(curr_res)
		
	while len(res) < len(searchWord):
		res.append([])
	return res
```

#### 3 Months
###### 859 Buddy Strings
```
def buddyStrings(self, s, goal):
	if len(s) != len(goal):
		return False
	if s == goal:
		return len(set(s)) < len(s)
	unmatched = [(a, b) for a, b in zip(s, goal) if a != b]
	return len(unmatched) == 2 and unmatched[0][::-1] == unmatched[1]
```
###### 184. Single-Threaded CPU
```
def getOrder(self, tasks):
	tasks = [(t[1], i, t[0]) for i, t in enumerate(tasks)]
	tasks.sort(key = lambda x:x[2])
	tasks = deque(tasks)
	time = 1
	available = []
	order = []
	while tasks or available:
		while tasks and tasks[0][2] <= time:
			heapq.heappush(available, tasks.popleft())
		if available:
			p, i, e = heapq.heappop(available)
			time += p
			order.append(i)
		else:
			time = tasks[0][2]
	return order
```

###### 208 Implement Trie
```
class Node:
	def __init__(self):
		self.children = {}
		self.word = False
		
class Trie:
	def __init__(self):
		self.head = Node()
	
	def insert(self, word: str) -> None:
		curr = self.head
		for c in word:
			if c not in curr.children:
				curr.children[c] = Node()
			curr = curr.children[c]
		curr.word = True
	
	def search(self, word: str) -> bool:
		curr = self.head
		for c in word:
			if c not in curr.children:
				return False
			curr = curr.children[c]
		return curr.word
	
	def startsWith(self, prefix: str) -> bool:
		curr = self.head
		for c in prefix:
			if c not in curr.children:
				return False
			curr = curr.children[c]
		return True
```

###### 296. Best Meeting Point
```
def minTotalDistance(self, grid):
	R, C = len(grid), len(grid[0])
	rows, cols = defaultdict(int), defaultdict(int)
	h = 0
	for i in range(R):
		for j in range(C):
			if grid[i][j] == 1:
				rows[i] += 1
				cols[j] += 1
				h += 1
	target = math.ceil(h/2)
	def median(counts):
		curr = 0
		for k in sorted(counts.keys()):
			curr += counts[k]
			if curr >= target:
				return k
	rm, cm = median(rows), median(cols)
	return sum([abs(k-rm)*rows[k] for k in rows.keys()]) + sum([abs(k-cm)*cols[k] for k in cols.keys()])
```

###### 317. Shortest Distance from All Buildings
```
def shortestDistance(self, grid):
	R, C = len(grid), len(grid[0])
	dist = [[0 for j in range(C)] for i in range(R)]
	building = sum(c==1 for r in grid for c in r)
	build_count = [[0 for j in range(C)] for i in range(R)]
	
	directions = [(1, 0), (-1, 0), (0, -1), (0, 1)]
	for i in range(R):
		for j in range(C):
			if grid[i][j] == 1:
				queue = deque([(i, j, 1)])
				visited = set()
				while queue:
					r, c, d = queue.popleft()
					for dr, dc in directions:
						nr, nc = dr+r, dc+c
						if 0 <= nr < R and 0 <= nc < C and (nr, nc) not in visited and grid[nr][nc] == 0:
							visited.add((nr, nc))
							queue.append((nr, nc, d+1))
							dist[nr][nc] += d
							build_count[nr][nc] += 1
	res = float('inf')
	for i in range(R):
		for j in range(C):
			if dist[i][j] > 0 and build_count[i][j] == building:
				res = min(res, dist[i][j])
	return res
```

###### 542. 01 Matrix
```
def updateMatrix(self, mat):
	R, C = len(mat), len(mat[0])
	queue = deque([(r, c) for r in range(R) for c in range(C) if mat[r][c] == 0])
	directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
	visited = set()
	while queue:
		 r, c = queue.popleft()
		 for dr, dc in directions:
			 nr, nc = r+dr, c+dc
			 if 0 <= nr < R and 0 <= nc < C and (nr, nc) not in visited and mat[nr][nc] == 1:
				 visited.add((nr, nc))
				 mat[nr][nc] = mat[r][c] + 1
				 queue.append((nr, nc))
	return mat
```

###### 1779. Find Nearest Point Has the Same X or Y Coordinate
```
def nearestValidPoint(self, x, y, points):
	dist, idx = float('inf'), -1
	for i, p in enumerate(points):
		if x == p[0]:
			if abs(y-p[1]) < dist:
				dist = abs(y-p[1])
				idx = i
		if p[1] == y:
			if abs(x-p[0]) < dist:
				dist = abs(x-p[0])
				idx = i
		if dist == 0:
			return idx
	return idx
```

###### 210. Course Schedule II
```
def findOrder(self, numCourses, prerequisites):
	adj = {i:[] for i in range(numCourses)}
	in_degree = [0 for i in range(numCourses)]

	for a, b in prerequisites:
		adj[b].append(a)
		in_degree[a]+=1

	queue = deque([i for i in range(numCourses) if in_degree[i] == 0])
	order = []
	while queue:
		i = queue.popleft()
		order.append(i)
		for nei in adj[i]:
			in_degree[nei] -= 1
			if in_degree[nei] == 0:
				queue.append(nei)
	return order if len(order) == numCourses else []
```

###### 297. Serialize and Deserialize Binary Tree
```
class Codec:
	def serialize(self, root):
		if not root:
			return '.,'
		return str(root.val) + ',' + self.serialize(root.left) + self.serialize(root.right)

	def deserialize(self, data):
		def dfs():
			nonlocal i
			i += 1
			if values[i-1] == '.':
				return None
			n = TreeNode(values[i-1])
			n.left = dfs()
			n.right = dfs()
			return n

		values = data.split(',')		
		i = 0
		return dfs()
```

###### 772. Basic Calculator III
```
def calculate(self, s: str):
	l1, o1, l2, o2 = 0, 1, 1, 1
	stack = []
	i = 0
	while i < len(s):
		c = s[i]
		## num
		if c.isdigit():
			n = 0
			while i < len(s) and s[i].isdigit():
				n = n*10+int(s[i])
				i += 1
			i -= 1
			if o2 == 1:
				l2 *= n
			else:
				l2 = int(l2/n)
		## (
		elif c == '(':
			stack.append((l1, o1, l2, o2))
			l1, o1, l2, o2 = 0, 1, 1, 1
		## )
		elif c == ')':
			l1 += o1*l2
			pl1, po1, pl2, po2 = stack.pop()
			if po2 == 1:
				l2 = pl2*l1
			else:
				l2 = int(pl2/l1)
			l1, o1 = pl1, po1
		## +
		elif c == '+':
			l1 += o1*l2
			o1, l2, o2 = 1, 1, 1
		## -
		elif c == '-':
			l1 += o1*l2
			o1, l2, o2 = -1, 1, 1
		## *
		elif c == '*':
			o2 = 1
		## /
		elif c == '/':
			o2 = -1
		i += 1
	return l1+ o1*l2
```

###### Making A Large Island
```
def largestIsland(self, grid):
	## DFS to find size for each island
	R, C = len(grid), len(grid[0])
	def dfs(r, c, id):
		directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
		size = 1
		grid[r][c] = id
		for dr, dc in directions:
			nr, nc = dr + r, dc + c
			if 0 <= nr < R and 0 <= nc < C and grid[nr][nc] == 1:
				size += dfs(nr, nc, id)
		return size

	sizes, id = {}, 2
	for i in range(R):
		for j in range(C):
			if grid[i][j] == 1:
				sizes[id] = dfs(i, j, id)
				id += 1
	maxsize = max(sizes.values()) if sizes else 0
	
	## Find Largest Island, DFS to see if neighbor around this empty
	for i in range(R):
		for j in range(C):
			if grid[i][j] == 0:
				add_size = 1
				seen = set()
				for dr, dc in directions:
					nr, nc = dr + r, dc + c
					if 0 <= nr < R and 0 <= nc < C and grid[nr][nc] > 1 and grid[nr][nc] not in seen:
						add_size += sizes[grid[nr][nc]]
						seen.add(grid[nr][nc])
				maxsize = max(maxsize, add_size)
	return maxsize
```

###### 2049. Count Nodes With the Highest Score
```
def countHighestScoreNodes(self, parents):
	## Adj_list
	n = len(parents)
	adj_dict = {i:[] for i in range(n)}
	for i, p in enumerate(parents):
		if p == -1:
			tree[i].append(i)
		else:
			tree[p].append(i)

	tree_sizes = [0 for i in range(n)]
	## DFS to find size 
	def dfs(i):
		size = 1
		for c in adj_dict[i]:
			size += dfs(c)
		tree_sizes[i] = size
		return size

	dfs(0)

	## FIND MAX
	max_score = 0
	cnt = 0
	for i in range(n):
		score = 1
		for c in adj_dict[i]:
			score += tree_sizes[c]
		parent_size = n - tree_sizes[i]
		if parent_size > 0:
			score *= parent_size
		if score > max_score:
			cnt = 1
			max_score = score
		elif score == max_score:
			cnt += 1
	return cnt
```