#### Knowledge Overview
* FIFO: First In first out
* **Types of Queues**
	* *Enqueue (Add)*: $O(1)$
	* *Dequeue (Remove):* $O(1)$
	* *Peak/ Front:* $O(1)$
	* *Is Empty:* $O(1)$
	* *Size:* $O(1)$
* **Basic Operations**
	* *Standard Queue*: Basic FIFO Queues
	* *Circular Queue*: Connects the end to front
	* *Deque (Double Ended Queue*): Allows insertion and deletion from both ends
	* *Priority Queue*: Removed based on priority rather than insertion order

#### Practice
###### **LeetCode 933. Number of Recent Calls**
#Queue 
```
class RecentCounter:
	def __init__(self):
		self.requests = deque()

	def ping(self, t: int) -> int:
		self.requests.append(t)
		while self.requests[0]+3000 < t:
			self.requests.popleft()
		return len(self.requests)
```

###### **LeetCode 232. Implement Queue Using Stacks**
#Queue #Stack 
[[Stack#**LeetCode 225. Implement Stack using Queues**]]

* Use two stacks, `stack_in` and `stack_out`
* Push operations go to `stack_in` first
* Pop operations check `stack_out`, if it's empty, transfer `stack_in` to `stack_out` and then pop FIFO

```
class MyQueue:
	def __init__(self):
		self.stack_in = []
		self.stack_out = []
	
	def push(self, x: int) -> None:
		self.stack_in.append(x)
	
	def pop(self) -> int:
		if not self.stack_out:
			while self.stack_in:
				self.stack_out.append(self.stack_in.pop())
		return self.stack_out.pop()

	def peek(self) -> int:
		if not self.stack_out:
			while self.stack_in:
				self.stack_out.append(self.stack_in.pop())
		return self.stack_out[-1]

	def empty(self) -> bool:
		return not self.stack_in and not self.stack_out
```

###### **LeetCode 279. Perfect Squares**
#Math #DP #BFS
```
def numSquares(self, n):
	squares = []
	i = 1
	while i*i <= n:
		squares.append(i*i)
		i += 1

	stack = deque([(n, 0)])
	visited = set()
	
	while stack:
		remainder, steps = stack.popleft()
		for s in squares:
			next_rem = remainder - s
			if next_rem == 0:
				return steps + 1
			if next_rem > 0 and next_rem not in visited:
				stack.append((next_rem, steps + 1))
				visited.add(next_rem)
	return n
```

###### **LeetCode 346. Moving Average from Data Stream**
#Array #Queue 
```
class MovingAverage:
	def __init__(self, size: int):
		self.values = deque([])
		self.size = size
		self.total = 0
		self.count = 0
	
	def next(self, val: int) -> float:
		self.values.append(val)
		self.total += val
		self.count += 1
		while len(self.values) > self.size:
			self.total -= self.values.popleft()
			self.count -= 1
		return self.total/self.count
```

###### **LeetCode 862. Shortest Subarray with Sum at Least K**
#Array #BinSearch #Queue #PrefixSum 
* I know it's sort of like a queue question but a more space efficiency/ computational efficiency way should be using sliding winders and 2 pointers
```
def shortestSubarray(self, nums, k):
	prefix = [0 for i in range(len(nums)+1)]
	for i in range(len(nums)):
		prefix[i+1] = prefix[i] + nums[i]
		
	queue = deque()
	shortest = float('inf')
	for i in range(len(prefix)):
		while queue and prefix[i]-prefix[queue[0]] >= k:
			shortest = min(shortest, i-queue.popleft())
		while queue and prefix[i] <= prefix[queue[-1]]:
			queue.pop()
		queue.append(i)
	return shortest if shortest != float('inf') else -1
```

###### **LeetCode 146. LRU Cache**
#Hash #LinkedList 
```
class LRUCache:
	def __init__(self, capacity: int):
		self.keys = deque([])
		self.cap = capacity
		self.store = {}
	
	def get(self, key: int) -> int:
		if key in self.store:
			self.keys.remove(key)
			self.keys.append(key)
		return self.store.get(key, -1)
	
	def put(self, key: int, value: int) -> None:
		if key in self.store:
			self.keys.remove(key)
		elif len(self.keys) >= self.cap:
			old_k = self.keys.popleft()
			del self.store[old_k]
		self.keys.append(key)
		self.store[key] = value
```

###### **LeetCode 622. Circular Queue**
#Queue #Stack 
```
class Node:
	def __init__(self, value = None, next = None):
		self.value = value
		self.next = next
	
class MyCircularQueue:
	def __init__(self, k: int):  
		self.cap = k
		self.head, self.tail = None, None
		self.count = 0
		
	def enQueue(self, value: int) -> bool:  
		if self.count == self.cap: 
			return False
		if self.count == 0:
			self.head = Node(value)
			self.tail = self.head
		else:
			new = Node(value)
			self.tail.next = new
			self.tail = new
		self.count += 1
		return True
			
	def deQueue(self) -> bool:  
		if self.count == 0:
			return False
		self.head = self.head.next
		self.count -= 1
		return True
	
	def Front(self) -> int: 
		if self.count == 0:
			return -1
		return self.head.value
	
	def Rear(self) -> int:
		if self.count == 0:
			return -1
		return self.tail.value 
	
	def isEmpty(self) -> bool:
		return self.count == 0
	
	def isFull(self) -> bool:
		return self.count == self.cap
```

###### **Meta 1. Queue Removals**
```
def findPositions(arr, x):
	queue = deque([(val, idx+1) for idx, val in enumerate(arr)])
	res = []

	for _ in range(x):
		poped = []
		for _ in range(min(x, len(queue))):
			poped.append(queue.popleft())

		max_element = max(poped, key = lambda x:x[0])
		res.append(max_element[1])

		for element in poped:
			if element != max_element:
				queue.append((min(element[0]-1, 0), element[1]))
	return res
```
