#### Knowledge Check
* **Types of Graphs**
	* Directed, Undirected, Weighted
	* Cyclic, Acyclic

* **Graph Representation**
	1. *Matrix*: `grid[r][c]` 0: free, 1:blocked
	2. *Adjacency List*: collection of lists
		```
		class GraphNode:
			def __init__(self, val):
				self.val = val
				self.neighbors = []
		```
	3. *Adjacency Dict:* 
		* `adj_dict[v1][v2] == 1` means edge exists from `v1` to `v2`
		* otherwise, `adj_dict[v1][v2] == 0`
	4. *Adjacency Matrix*: 
		```
		adjList = defaultdict(list)
		in_degree = [0 for i in range(len(edges))]
		for a, b in edges:
			adjList[a].append(b)
			in_degree[b] += 1
		```
 
* **Depth-First Search (DFS)**
	* ***Basic idea:***
		* start from `(0, 0)`
		* *RETURN WHEN*
			* if out of bound: `r < 0 or r == R or c < 0 or c == C)`
			* or in `visited`
			* or blocked: `grid[r][c] == 1`
		* add visited
		*  traverse to 4 directions by `dfs()`
		* remove visited
	* ***Code:***
	```
	def dfs(r, c):
		if r < 0 or c < 0 or r >= R or c >= R \\
		or (r, c) in visited or grid[r][c] == 1:
			return something
		if certain condition met: 
			do something
		visited.add((r, c))
		dfs(r+1, c)
		dfs(r-1, c)
		dfs(r, c+1)
		dfs(r, c-1)
		visited.remove((r, c))
		return something
	```

* **Breadth-First Search (BFS)**
	* ***Basic ideas***
		* Initialize R, C, visited, queue `[(0, 0)]`
		* While `queue`:
			* `for (r, c) in queue`:
				* `r, c = queue.popleft()`
				* if at destination, return something
				* `visited.add((r, c))`
				* for 4 directions (we could also combine this with `adj_dict`, `for nei in adj_dict`), if not out of bound or not in visited or not blocked, add to the visited
			* `length += 1`
	* ***Code***
	```
	def bfs():
		R, C = int, int
		visited = set()
		queue = deque([(0, 0)])
		length = 0
		
		while queue:
			for _ in range(len(queue)):
				r, c = queue.popleft()
				if somecondition met:
					return length
				visited.add((r, c))
				neighbors = [[0, 1], [0, -1], [-1, 0], [1, 0]]
				for dr, dc in neighbors:
					new_r = r + dr
					new_c = c + dc
					if new_r < 0 or new_r >= R or new_c < 0 or new_c >= C or (r, c) in visited or blocked:
						continue
					queue.append((new_r, new_c))
			length += 1
	```

* **Common Algorithms**
	* **Dijkstra (DFS)**
		Find the shortest distance from source node to all other nodes in weighted graph with non-negative weights 
	```
	def dijkstra(graph, start):
		dist = {n: float('inf') for n in graph}
		dist[start] = 0

		heap = [(0, start)]
		heapq.heapify(heap)

		while heap:
			cur_dist, cur_node = heapq.heappop(heap)
			if cur_dist > dist[cur_node]:
				continue
			for n, w in graph[cur_node]:
				d = cur_dist + w
				if d < dist[nei]:
					dist[nei] = d
					heapq.heappush(heap, (d, nei))
		return dist
	```
	
	* **Bellman-Ford (BFS)**
		Find the shortest path from src to all other nodes (worked even with negative weight edges) + negative weights detection
		```
		def bellman_ford(edges, V, start):
			dist = {i: float('int') for i in range(V)}
			dist[start] = 0
			
			for i in range(V-1):
				for u, v, w in edges:
					if dist[u] != float('inf') and dist[u] + w < dist[v]:
						dist[v] = dist[u] + w

			for u, v, w in edges:
				if dist[u] != float('inf') and dist[u] + w < dist[v]: 
					return dist, True ## Negative Weight Cycle exists
					
			return dist, False ## Negative Weight Cycle do not exist
		```

* **Floyd-Warshall (BFS)**
		Find shortest paths between all pairs of nodes in a weighted graph, require no negative weigh cycles
	```
		def floyd_warshall(graph):
			dist = [[float('inf')] * V for _ in range(V)]
			for v in range(V):
				for v in range(V):
					dist[u][v] = graph[u][v]
			for k in range(V):
				for i in range(V):
					for j in range(V):
						dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
			return dist
	```

* **Topological Sort**
	* ***Kahn's Algorithm (BFS)***
		```
		def kahn(edges, n):
			adj_dict = defaultdict(list)
			indegree = [0] * n
			for u, v in edges:
				adj_dict[u].append(v)
				adj_dict[v].append(u)
				indegree[v] += 1
				
			queue = deque([i for i in range(n) if indegree[i] == 0])
			order = []
			while queue:
				n = queue.popleft()
				order.append(n)

				for nei in adj_dict[n]:
					indegree[nei] -= 1
					if indegree[nei] == 0:
						queue.append(nei)
						
			return order if len(order) == n else []
		```
	* ***DFS-based Approach***
		```
		def topo_dfs(egdes, n):
			visited = [False] * n
			adj_dict = defaultdict(list)
			for u, v in edges:
				adj_dict[u].append(v)
				adj_dict[v].append(u)
			stack = []

			def dfs(i):
				visited[i] = True
				for nei in adj_dict[i]:
					if not visited[nei]:
						dfs(nei)
				stack.append(i)

			for i in range(n):
				if not visited[i]:
					dfs(i)
					
			return stack[::-1]
		```

* **Prim's Algorithm**
	Minimum Spanning Tree
	* Start with arbitrary node as starting point
	* Initialize visited to track
	* Use heap to select edge with minimum weight to connect vertex in MST to vertex outside
	```
	def prim(graph, n):
		start = 0
		visited = set()
		heap = [(0, start)]
		mst_cost = 0
		mst_edges = []

		while heap and len(visited) < n:
			w, u = heapq.heappop(heap)
			if u in visited:
				continue
			visited.add(u)
			mst_cost += w
			mst_edges.append((u, w))

			for u, w in graph[u]:
				if v not in visited:
					heapq.heappush(heap, (w, v))
			return mst_cost, mst_edges
	```

#### Practice
###### **LeetCode 733. Flood Fill**
#DFS #BFS 
```
def floodFill(self, image, sr, sc, color):
	## BFS
	m, n = len(image), len(image[0])
	queue = deque([(sr, sc)])
	visited = set()
	origcolor = image[sr][sc]
	
	while queue:
		r, c = queue.popleft()
		image[r][c] = color
		visited.add((r, c))

		directions = [(1, 0), (-1, 0), (0, -1), (0, 1)]
		for dr, dc in directions:
			newr, newc = r + dr, c + dc
			if newr < 0 or newr >= m or newc < 0 or newc >= n or (newr, newc) in visited or image[newr][newc] != origcolor:
				continue
			queue.append((newr, newc))
	return image

	## DFS
	def dfs(r, c, origcolor):
		if r < 0 or r >= m or c < 0 or c >= n or image[r][c] == color or image[r][c] != origcolor:
			return 
		image[r][c] = color
		dfs(r+1, c, origcolor)
		dfs(r-1, c, origcolor)
		dfs(r, c+1, origcolor)
		dfs(r, c-1, origcolor)

	m, n = len(image), len(image[0])
	dfs(sr, sc, image[sr][sc])
	return image
```

###### **LeetCode 997. Find the Town Judge**
#Hash 
```
def findJudge(self, n, trust):
	indegree = [0 for i in range(n)]
	outdegree = [0 for i in range(n)]
	for a, b in trust:
		indegree[b-1] += 1
		outdegree[a-1] -= 1
	for i in range(n):
		if indegree[i] == n-1 and outdegree[i] == 0:
			return i+1
	return -1
```

###### **LeetCode 133. Clone Graph**
#Hash #DFS #BFS 
```
def cloneGraph(self, node):
	save = {}
	def dfs(n):
		if not n:
			return n
		if n in save:
			return save[n]
		clone = Node(n.val, [])
		save[n] = clone
		for nei in n.neighbors:
			clone.neighbors.append(dfs(nei))
		return clone
	return dfs(node)
```

###### **LeetCode 207. Course Schedule**
#DFS #BFS 
```
def canFinish(self, numCourses, prerequisites):
	adj_dict = {i: [] for i in range(numCourses)}
	indegree = [0 for i in range(numCourses)]
	for a, b in prerequisites:
		adj_dict[a].append(b)
		indegree[b] += 1 

	queue = deque([i for i in range(numCourses) if indegree[i] == 0])
	visited = set()
	while queue:
		i = queue.popleft()
		visited.add(i)
		for nei in adj_dict[i]:
			if nei not in visited:
				indegree[nei] -= 1
				if indegree[nei] == 0:
					queue.append(nei)
	return len(visited) == numCourses
```

###### **LeetCode 332. Reconstruct Itinerary**
#DFS #Graph
```
def findItinerary(self, tickets):
	adj_dict = defualtdict(list)
	for a, b in tickets:
		adj_dict[a].append(b)
	for a, b in adj_dict.items():
		b.sort(reverse = True)

	res = []
	def dfs(i):
		for nei in adj_dict[i]:
			next_i = adj_dict[i].pop()
			dfs(next_i)
		res.append(i)

	dfs('JFK')
	return res[::-1]
```

###### **LeetCode 269. Alien Dictionary**
#Array #Graph #DFS #BFS 
```
def alienOrder(self, words):
	## Adj_Dict
	adj_dict = defaultdict(list)
	indegree = {char: 0 for w in words for char in w}

	for i in range(len(words)-1):
		w1, w2 = words[i], words[i+1]
		min_length = min(len(w1), len(w2))
		
		if len(w2) < len(w1) and w2[:min_length] == w1[:min_length]:
			return ""
		
		for j in range(min_length):
			if w1[j] != w2[j]:
				if w2[j] not in adj_dict[w1[j]]:
					adj_dict[w1[j]].append(w2[j])
					indegree[w2[j]] += 1
				break

	queue = deque([w for w in indegree if indegree[w] == 0])
	order = ""
	while queue:
		i = queue.popleft()
		order += i
		for nei in adj_dict[i]:
			indegree[nei] -= 1
			if indegree[nei] == 0:
				queue.append(nei)
				
	return order if len(order) == len(indegree) else ""
```

###### **LeetCode 210.  Course Schedule II**
#DFS #BFS 
```
def findOrder(self, numCourses, prerequisites):
	adj_dict = {i: [] for i in range(numCourses)}
	indegree = [0 for i in range(numCourses)]

	for a, b in prerequisites:
		adj_dict[b].append(a)
		indegree[a] += 1

	queue = deque([i for i in range(numCourses) if indegree[i] == 0])
	order = []
	while queue:
		i = queue.popleft()
		order.append(i)
		for nei in adj_dict[i]:
			indegree[nei] -= 1
			if indegree[nei] == 0:
				queue.append(nei)
	return order if len(order) == numCourses else []
```
