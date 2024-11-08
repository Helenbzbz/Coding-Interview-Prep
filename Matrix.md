#### Knowledge Check
* **Overview**
	* Traversing a 2D grid, modifying elements, or performing calculations based on neighboring cells
	* **Algorithms**
		* Iteration
		* DFS/BFS
		* Dynamic Programming (DP)
	* **Combined with:**
		* Directional Vectors `[(0,1), (1,0), (0,-1), (-1,0)]`
		* Boundaries Check
		* Visited Tracking

#### Practice
###### LeetCode 54. Spiral Matrix  
#Matrix #Simulation  
```
def spiralOrder(self, matrix):
	res = []
	R, C = len(matrix), len(matrix[0])
	r, c = 0, 0
	while r < R and c < C:
		for i in range(c, C):
			res.append(matrix[r][i])
		r += 1
		for i in range(r, R):
			res.append(matrix[i][C-1])
		C -= 1

		if r >= R or c >= C:
			break
		for i in range(C-1, c-1, -1):
			res.append(matrix[R-1][i])
		R -= 1
		for i in range(R-1, r-1, -1):
			res.append(matrix[i][c])
		c += 1
	return res
```
###### LeetCode 48. Rotate Image  
#Matrix  
```
def rotate(self, matrix):
	r, c = 0, 0
	R, C = len(matrix)-1, len(matrix[0])-1
	while r < R and c < C:
		for i in range(R-r):
			top_left = matrix[r][c+i]
			matrix[r][c+i] = matrix[R-i][c]
			matrix[R-i][c] = matrix[R][C-i]
			matrix[R][C-i] = matrix[r+i][C]
			matrix[r+i][C] = top_left
		r += 1
		c += 1
		C -= 1
		R -= 1
	return matrix
```
###### LeetCode 73. Set Matrix Zeroes  
#Matrix  
```
def setZeroes(self, matrix):
	first = False
	R, C = len(matrix), len(matrix[0])

	for c in range(C):
		if matrix[0][c] == 0:
			first = True
			break
	
	for r in range(1, R):
		for c in range(C):
			if matrix[r][c] == 0:
				matrix[0][c] = 0
				matrix[r][0] = 0
					
	for r in range(1, R):
		for c in range(1, C):
			if matrix[0][c] == 0 or matrix[r][0] == 0:
				matrix[r][c] = 0

	if matrix[0][0] == 0:
		for r in range(R):
			matrix[r][0] = 0

	if first:
		for c in range(C):
			matrix[0][c] = 0
```
###### LeetCode 240. Search a 2D Matrix II  
#Matrix #BinarySearch  
- Start from bottom left
	- If too big --> `row -= 1`
	- if too small --> `col += 1`
- When out of bound, still not found, `return False`

```
def searchMatrix(self, matrix, target):
	R, C = len(matrix), len(matrix[0])
	i, j = R-1, 0
	while j < C and i >= 0:
		if matrix[i][j] == target:
			return True
		elif matrix[i][j] > target:
			i -= 1
		else:
			j += 1
	return False
```
###### LeetCode 200. Number of Islands  
#Matrix #DepthFirstSearch #BreadthFirstSearch
```
def numIslands(self, grid):
	R, C = len(grid), len(grid[0])
	visited = set()
	def dfs(r, c):
		if r < 0 or r >= R or c < 0 or c >= C or (r, c) in visited or grid[r][c] == '0':
			return
		visited.add((r, c))
		dfs(r+1, c)
		dfs(r-1, c)
		dfs(r, c+1)
		dfs(r, c-1)

	count = 0
	for r in range(R):
		for c in range(C):
			if grid[r][c] == '1' and (r, c) not in visited:
				dfs(r, c)
				count += 1
	return count
```