#### Knowledge Check
* **Relevant**: [[Recursion]]
* **Basic Idea:**
	* Incrementally construct potential solutions by exploring all choices and backtracking when constraints are violated.
* **Basic Steps**
	* If `curr == success`, declare and terminate the path.
	* If all paths have been explored without success, backtrack to the previous choice.
	* If the path is not a dead-end, continue exploring further.
* **Used When**:
	* Complete exploration of possible solutions is required.
	* Best feasible selection under given constraints is needed.
* **Example**: `N-Queens Problem, Sudoku Solver, Word Search`
* ***How is this different from recursion?**
	* **Exploration and Pruning**: Backtracking prunes paths that cannot lead to a solution
	* **State Restoration (Backtracking Step)**: In regular recursion, there’s often no need to undo previous steps
	* **Constraint-Satisfaction Focus**: Backtracking is especially suited for problems with constraints


#### Practice
###### LeetCode 784. Letter Case Permutation  
#String #Backtrack  
```
def letterCasePermutation(self, s):
	chars = [c for c in s]
	res = []
	def backtrack(i):
		if i == len(s):
			res.append(''.join(chars))
			return
		if chars[i].isnumeric():
			backtrack(i+1)
		else:
			chars[i] = chars[i].lower()
			backtrack(i+1)
			chars[i] = chars[i].upper()
			backtrack(i+1)
	backtrack(0)	
	return res
```
######  LeetCode 39. Combination Sum  
#Array #Backtrack  
```
def combinationSum(self, candidates, target):
	res = []
	cur = []
	def backtrack(i, target):
		if 0 == target:
			res.append(cur.copy())
		if i >= len(candidates) or target < 0:
			return
		cur.append(candidates[i])
		backtrack(i, target - candidates[i])
		cur.pop()
		backtrack(i+1, target)
	backtrack(0, target)
	return res
```
######  LeetCode 77. Combinations  
#Array #Backtrack
```
def combine(self, n, k):
	cur = []
	res = []

	def backtrack(i):
		if len(cur) == k:
			res.append(cur.copy())
			return
		if i > n:
			return
		cur.append(i)
		backtrack(i+1)
		cur.pop()
		backtrack(i+1)

	backtrack(1)
	return res
```