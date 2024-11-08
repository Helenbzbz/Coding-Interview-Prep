#### Knowledge Check
* **Used when**:
	* ***Optimal substructure***: optimal solution can be formed by combining optimal solutions to subproblems
	* ***Greedy choice property*** a locally optimal choice leads to a globally optimal solution
* **General Steps**:
	* Sort or establish a priority rule.
	* Locally optimal choice at each step (e.g., maximum profit, minimum cost, earliest finish time).
	* Until reaching a solution or all options are exhausted.
* **Examples of Problems:** `Activity Selection, Dijkstra's Algorithm, Fractional_Knapsack, Sequencing_with_Deadlines`

#### Practice
###### **LeetCode 455. Assign Cookies**
#Greedy 
```
def findContentChildren(self, g, s):
	g.sort()
	s.sort()
	i, j = 0, 0
	res = 0
	while i < len(g) and j < len(s):
		if g[i] <= s[j]:
			res += 1
			i += 1
			j += 1
		else:
			j += 1
	return res
```
###### **LeetCode 392. Is Subsequence**
#TwoPointers #Greedy #DP 
```
def isSubsequence(self, s, t):
	if s == "":
		return True
	i, j = 0, 0
	while i < len(s) and j < len(t):
		if s[i] == t[j]:
			i += 1
		j += 1
	return i == len(s)
```

###### **LeetCode 55. Jump Game**
#DP #Greedy 
```
def canJump(self, nums):
	pointer = len(nums)-1
	for i in range(len(nums)-1, -1, -1):
		if i + nums[i] >= pointer:
			pointer = i
	return pointer == 0
```
###### **LeetCode 406. Queue Reconstruction**
#Greedy #Array 
```
def reconstructQueue(self, people):
	people.sort(key = lambda x:(-x[0], x[1]))
	output = []
	for p in people:
		output.insert(p[1], p)
	return output
```
###### **LeetCode 871. Minimum Number of Refueling Stops**
#Array #DP #Greedy #Heap 
```
def minRefuelStops(self, target, startFuel, stations):
	dp = [startFuel] + [0]*len(stations)
	for i in range(len(stations)):
		loc, cap = stations[i]
		for j in range(i, -1, -1):
			if dp[j] >= loc:
				dp[j+1] = max(dp[j]+cap, dp[j+1])
	for i, n in enumerate(dp):
		if n >= target:
			return i
	return -1
```
###### **LeetCode 135. Candy**
#Greedy #Array 
```
def candy(self, ratings):
	candies = [1 for i in range(len(ratings))]
	for i in range(len(ratings)-1):
		if ratings[i+1] > ratings[i]:
			candies[i+1] = candies[i] + 1
	res = candies[-1]
	for i in range(len(ratings)-2, -1, -1):
		if ratings[i] > ratings[i+1]:
			candies[i] = max(candies[i], candies[i+1] + 1)
		res += candies[i]
	return res
```
###### **LeetCode 621. Task Scheduler**
#Array #Hash #Greedy #Heap 
```
def leastInterval(self, tasks, n):
	if n == 0:
		return len(tasks)
		
	task_freq = Counter(tasks)
	heap = [(-freq, task) for task, freq in task_freq.items()]
	heapq.heapify(heap)
	
	queue = deque()
	t = 0
	
	while queue or heap:
		t += 1
		if heap:
			freq, task = heapq.heappop(heap)
			if freq < -1:
				queue.append((t+n, freq+1, task))
				
		while queue and queue[0][0] == t:
			_, freq, task = queue.popleft()
			heapq.heappush(heap, (freq, task))

	return t
```
###### **LeetCode 763. Partition Labels**
```
def partitionLabels(self, s):
	last = {c:i for i, c in enumerate(s)}
	res = []
	track, start = 0, 0
	for i in range(s):
		track = max(track, last[s[i]])
		if i == track:
			res.append((i - start + 1))
			start = i + 1
	return res
```



