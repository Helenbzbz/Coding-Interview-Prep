#### Knowledge Check
* The **Union-Find** (or **Disjoint Set Union, DSU**) is a data structure that helps manage a partition of a set into disjoint (non-overlapping) subsets
* **Operations**:
	- ***Find***: 
		- Determine the "parent" of a set containing a particular element 
		- **Path compression** to optimize future lookups.
	- ***Union***: 
		- Merge two subsets into a single subset
		- Optimized using **union by rank/size** to keep the structure balanced
- **Implementation**
```
class UnionFind:
	def __init__(self, n):
		self.rank = [1] * n
		self.parent = [i for i in range(n)]

	def find(self, x):
		if self.parent[x] != x:
			self.parent[x] = self.find(self.parent[x])
		return self.parent[x]

	def merge(self, x, y):
		px, py = self.find(x), self.find(y)
		if px == py:
			return
		rx, ry = self.rank[px], self.rank[py]
		if rx > ry:
			self.parent[py] = px
		elif rx < ry:
			self.parent[px] = py
		else:
			self.parent[py] = px
			self.rank[px] += 1
```


#### Practice
###### LeetCode 323. Number of Connected Components in an Undirected Graph  
#Graph #UnionFind
```
class UnionFind:
	def __init__(self, n):
		self.parents = [i for i in range(n)]
		self.ranks = [1] * n
		
	def find(self, x):
		if self.parents[x] != x:
			self.parents[x] = self.find(self.parents[x])
		return self.parents[x]

	def merge(self, x, y):
		px, py = self.find(x), self.find(y)
		if px == py:
			return
		rx, ry = self.ranks[x], self.ranks[y]
		if rx < ry:
			self.parents[px] = py
		elif rx > ry:
			self.parents[py] = px
		else:
			self.parents[py] = px
			self.ranks[px] += 1

class Solution:
	def countComponents(self, n, edges):
		U = UnionFind(n)
		for a, b in edges:
			U.merge(a, b)
		return len(set([U.find(i) for i in range(n)]))
```
###### LeetCode 721. Accounts Merge  
#Graph #UnionFind #String
```
class UnionFind:
    def __init__(self):
        self.parent = {}

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            self.parent[rootY] = rootX

class Solution:
    def accountsMerge(self, accounts):
        uf = UnionFind()
        
        email_to_name = {}
        for account in accounts:
            name = account[0]
            first_email = account[1]
            for email in account[1:]:
                if email not in uf.parent:
                    uf.parent[email] = email 
                uf.union(first_email, email)
                email_to_name[email] = name

        from collections import defaultdict
        root_to_emails = defaultdict(list)
        for email in uf.parent:
            root = uf.find(email)
            root_to_emails[root].append(email)

        res = []
        for root, emails in root_to_emails.items():
            res.append([email_to_name[root]] + sorted(emails))

        return res
```
###### LeetCode 684. Redundant Connection  
#Graph #UnionFind
```
class UnionFind:
    def __init__(self, n):
        self.parent = [i for i in range(n + 1)]
        self.rank = [1] * (n + 1)

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # Path compression
        return self.parent[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)

        if rootX == rootY:
            return False  # x and y are already connected

        # Union by rank
        if self.rank[rootX] > self.rank[rootY]:
            self.parent[rootY] = rootX
        elif self.rank[rootX] < self.rank[rootY]:
            self.parent[rootX] = rootY
        else:
            self.parent[rootY] = rootX
            self.rank[rootX] += 1

        return True

class Solution:
    def findRedundantConnection(self, edges):
        n = len(edges)
        uf = UnionFind(n)

        for u, v in edges:
            if not uf.union(u, v):  # If union returns False, cycle detected
                return [u, v]
```

###### LeetCode 685. Redundant Connection II  
#Graph #UnionFind 
```
class UnionFind:
    def __init__(self, n):
        self.parent = [i for i in range(n + 1)]
        self.rank = [1] * (n + 1)

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # Path compression
        return self.parent[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX == rootY:
            return False  # x and y are already connected (cycle detected)

        # Union by rank
        if self.rank[rootX] > self.rank[rootY]:
            self.parent[rootY] = rootX
        elif self.rank[rootX] < self.rank[rootY]:
            self.parent[rootX] = rootY
        else:
            self.parent[rootY] = rootX
            self.rank[rootX] += 1

        return True

class Solution:
    def findRedundantDirectedConnection(self, edges):
        n = len(edges)
        parent = {}
        uf = UnionFind(n)

        # Step 1: Identify a node with two parents, if any
        candidate1 = candidate2 = None
        for u, v in edges:
            if v in parent:
                candidate1 = [parent[v], v]
                candidate2 = [u, v]     
            else:
                parent[v] = u

        # Step 2: Use Union-Find to check for cycles
        for u, v in edges:
            if [u, v] == candidate2:
                continue  

            if not uf.union(u, v):
                # Cycle detected
                if not candidate1:
                    return [u, v]
                return candidate1
                
        return candidate2
```