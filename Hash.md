#### Knowledge Check
1. **Hash**
	* **Sets**, a sets of values
	* **Maps**, a set of values point to other values
	* **Insert:** TreeMap $O(logn)$, HashMap $O(1)$
	* **Remove:** TreeMap $O(logn)$, HashMap $O(1)$
	* **Search:** TreeMap $O(logn)$, HashMap $O(1)$
	* **In Order**: TreeMap $O(n)$, HashMap $O(nlogn)$
		Hash Map do not keep any order, therefore, we need to take out all the keys and sort to find the values
	
2. **Hash Implementation**
	* `put('key', 'value')` -> an array; `key` add to gather integer and mod by size of array
	* **Problem: Collision of Index**
		* Minimize Collision: Half Full, resize, double by rehashing
		* Solve Collision: Linked List/ Open Addressing (if occupied, go to next)

```
class Pair:
	def __init__(self, key, val):
		self.key = key
		self.val = val
		
class HashMap:
	def __init__(self):
		self.size = 0
		self.capacity = 2
		self.map = [None, None]
		
	def hash(self, key):
		index = 0
		for c in key:
			index += ord(c)
		return index % self.capacity
		
	def rehash(self):
		self.capacity *= 2
		newMap = []
		for i in range(self.capacity):
			newMap.append(None)
		oldMap = self.map
		self.map = newMap
		self.size = 0
		for pair in oldMap:
			if pair:
				self.put(pair.key, pair.val)
				
	def get(self, key):
		index = self.hash(key)
		while self.map[index] != None:
			if self.map[index].key == key:
				return self.map[index].val
			index += 1
			index = index % self.capacity
		return None
		
	def put(self, key, val):
		index = self.hash(key)
		while True:
			if self.map[index] == None:
				self.map[index] = Pair(key, val)
				self.size += 1
				if self.size >= self.capacity //2:
					self.rehash()
				return
			elif self.map[index].key == key:
				self.map[index].val = val
				return
		index += 1
		index = index % self.capacity
```
* **Example**: `Contains_Duplicates, Find_Prefix_of_Strs, Valid_Anagram, Group_Anagrams, Two_Sum`

#### Practice 

###### **LeetCode 136.  Single Number**
#Array #Bit

```
def singleNumber(nums):
	seen = set()
	total_1, total_2 = 0, 0
	for n in nums:
		if n not in seen:
			total_1 += n
			seen.add(n)
		total_2 += n
	return total_1*2 - total_2
```

###### **LeetCode 217. Contain Duplicates**
#Array #HashTable  #Sorting

```
def containsDuplicate(nums):
	seen = set()
	for n in nums:
		if n in seen:
			return True
		seen.add(n)
	return False
```

###### **LeetCode 347. Top K Frequent Elements**
#Array #HashTable #Sorting #Heap

- **QuickSelect:** 
	- Count Frequency
	- Store Unique Elements
	- ***Partition Function***
		- All elements < selected pivot move to left
		- All elements > selected pivot move to right
		- Return store index of the selected pivot variable
	- ***Quick Select***
		- Similar to quick Sort
		- Recursively focus on either left or right until pivot is at $kth$ 

```
def topKFrequent(self, nums, k):

	def partition(l, r, pivot):
		pivot_freq = count[unique[pivot]]
		# 1. Move to end
		unique[pivot], unique[r] = unique[r], unique[pivot]
		# 2. Move all less to left
		store_idx = l
		for i in range(l, r):
			if count[unique[i]] < pivot_freq:
				unique[store_idx], unique[i] = unique[i], unique[store_idx]
				store_idx += 1
		unique[r], unique[store_idx] = unique[store_idx], unique[r]
		return store_idx

	def quickselect(l, r, k):
		if l == r:
			return
		pivot = random.randint(l, r)
		pivot = partition(l, r, pivot)
		if k == pivot:
			return 
		elif k < pivot:
			quickselect(l, pivot-1, k)
		else:
			quickselect(pivot+1, r, k)
		
	# 1. Count Freq and Store Unique
	from collections import Counter
	count = Counter(nums)
	unique = list(count.keys())
	
	# 2. Start QuickSelect
	n = len(unique)
	quickselect(0, n-1, n-k)		
	return unique[n-k:]
```

###### **LeetCode 49. Group Anagrams** [[String#Practice]]

###### **LeetCode 128. Max Points on a Line** 
#Hash #Math #Array 

```
def maxPoints(self, points):
	def linear(p1, p2):
		x1, x2, y1, y2 = p1[0], p2[0], p1[1], p2[1]
		if x1 == x2:
			return (float('inf'), x1)
		if y1 == y2:
			return (0, y1)
		slope = (y2-y1)/(x2-x1)
		intercept = y1 - slope*x1
		return (slope, intercept)
		
	if len(points) <= 1:
		return len(points)
		
	store = defaultdict(set)
	res = 0
	for i in range(len(points)-1):
		for j in range(i, len(points)):
			p1, p2 = points[i], points[j]
			idx = linear(p1, p2)
			store[idx].add(i)
			store[idx].add(j)
			res = max(res, len(store[idx]))
	return res
```

**LeetCode 30. Substring with Concatenation of All Words** [[String#Practice]]

**LeetCode 560. Subarray Sum Equals K**
#Array #HashTable #PrefixSum 

```
def subarraySum(nums, k):
	sum_idx = defaultdict(int)
	res = 0
	cur_sum = 0
	sum_idx[0] = 1
	for i, n in enumerate(nums):
		cur_sum += n
		if cur_sum - k in sum_idx:
			res += sum_idx[cur_sum-k]
		sum_idx[cur_sum] += 1
	return res
```

