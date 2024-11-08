#### Knowledge Check
* **Basic Idea (Backtracking)**: Incrementally construct choices to the solutions
	[[Backtrack]]
![[Pasted image 20241026144850.png]]

* *Basic Steps*
	* If `curr == success`, declare and terminate
	* if all paths explored, backtrack to previous
	* If not dead-end, keep exploring
	
* **Used When**:
	* Complete Exploration is needed
	* Best Feasible Select
	
* **Example**: `Path_in_Binary_Matrix, Check_Knight_Tour_Configuration, Word_Search`

#### Classic Examples
1. **Subsets**
	```
	def subset(nums):
		res = []
		cur = []
		
		def recursive(cur, i):
			if i == len(nums):
				res.append(cur.copy())
				return
			cur.append(nums[i])
			recursive(cur, i+1) --> Add i & move to i+1
			cur.pop()
			recursive(cur, i+1) --> Do not add i & move to i+1

		recursive(curr, 0)
		return res
	```

2. **Permutations**
```
def permutations(nums):
	res = []
	
	def backtrack(start):
		if start == len(nums):
			res.append(nums.copy())
			return
			
		for i in range(start, len(nums)):
			res[start], res[i] = res[i], res[start]
			backtrack(start + 1)
			res[start], res[i] = res[i], res[start]

	backtrack(0)
	return res
```

3. **Combinations**
```
def combinations(n, k):
	res = []
	curr = []
	def backtrack(i):
		if len(curr) == k:
			res.append(curr.copy())
			return
		if i > n:
			return
		curr.append(i)
		bcktrack(i+1)
		curr.pop()
		backtrack(i+1)
		
	backtrack(1)
	return res
```


#### Practice
###### **LeetCode 206. Reverse Linked List & LeetCode 21. Merge Linked List** 
#LinkedList #Recursion 
* Both could be done in recursive but not the most optimal/ ideal case ...
* I will stick with my usual approach

###### **LeetCode 46. Permutations**
#Array #Backtrack 
```
def permute(self, nums):
	res = []
	
	def backtrack(start):
		if start == len(nums):
			res.append(nums.copy())
			return
		for i in range(start, len(nums)):
			nums[start], nums[i] = nums[i], nums[start]
			backtrack(start + 1)
			nums[start], nums[i] = nums[i], nums[start]

	backtrack(0)
	return res
```

###### **LeetCode 78. Subsets**
#Array #Backtrack 
```
def subsets(self, num):
	res, curr = [], []
	
	def backtrack(i):
		if i == len(nums):
			res.append(curr.copy())
			return
		curr.append(nums[i])
		backtrack(i+1)
		curr.pop()
		backtrack(i+1)

	backtrack(0)
	return res
```

###### **LeetCode 37. Sudoku Solver**
#Array #HashTable #Backtrack #Matrix
```
def solveSudoku(self, board):
	rows = {i: set() for i in range(9)}
	cols = {i: set() for i in range(9)}
	box = {i: set() for i in range(9)}
	unfill = []

	for r in range(9):
		for c in range(9):
			if board[r][c] != '.':
				rows[r].add(board[r][c])
				cols[c].add(board[r][c])
				box[r//3*3+c//3].add(board[r][c])
			else:
				unfill.append((r,c))

	def fill_number(i):
		if i == len(unfill):
			return True

		r, c = unfill[i]
		for num in range(1, 10):
			n = str(num)
			if n in rows[r] or n in cols[c] or n in box[r//3*3+c//3]:
				continue
			board[r][c] = n
			rows[r].add(n)
			cols[c].add(n)
			
			box[r//3*3+c//3].add(n)
			
			if fill_number(i+1):
				return True
				
			board[r][c] = '.'
			rows[r].remove(n)
			cols[c].remove(n)
			box[r//3*3+c//3].remove(n)

	fill_number(0)
```

###### **LeetCode 51. N-Queens**
#Array #Backtrack 
```
def solveNQueens(self, n):
	board = [['.' for i in range(n)] for j in range(n)]
	cols = {i: False for i in range(n)}
	diag = {i: False for i in range(-n, n+1)}
	anti = {i: False for i in range(0, 2*n+1)}
	res = []
	
	def backtrack(r):
		if r == n:
			res.append([''.join(row) for row in board])
		for c in range(n):
			if cols[c] or diag[r-c] or anti[r+c]:
				continue
			board[r][c] = 'Q'
			cols[c] = True
			diag[r-c] = True
			anti[r+c] = True
			backtrack(r+1)
			board[r][c] = '.'
			cols[c] = False
			diag[r-c] = False
			anti[r+c] = False

	backtrack(0)
	return res
```

###### **LeetCode 241. Different Ways to Add Parentheses**
#Math #String #DP #Recursion 
```
def diffWaysToCompute(self, expression):
	res = []

	# 1. Base Case Empty
	if len(expression) == 0: 
		return res
	# 2. Base Case Num
	if len(expression) == 1 or (len(expression) == 2 and expression[0].isdigit()): 
		return [int(expression)]

	# 3. For each Operator, we break down and enumerate l, r
	for i, n in enumerate(expression):
		if n.isdigit():
			continue
		l = self.diffWaysToCompute(expression[:i])
		r = self.diffWaysToCompute(expression[i+1:])
		for i in l:
			for j in r:
				if n == '+':
					res.append(i + j)
				elif n == '-':
					res.append(i - j)
				else:
					res.append(i * j)
					
	return res
```

###### **LeetCode 98. Validate BST**
#DFS #Tree 
```
def isValidBST(self, root):
	def recursive(node, low = -math.inf, high = math.inf):
		if not node: 
			return True
		if node.val <= low or node.val >= high:
			return False
		return recursive(node.right, node.val, high) and recursive(node.left, low, node.val)
	return recursive(root)
```


