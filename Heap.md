#### Knowledge Check
* **Overview** 
	* Specialized tree-based structure satisfies the heap property
	* Min-heap: parent is always <= children
	* Max-Heap: max-heap, parent is always >= children
* **Basic Operations**
	* *Insertion:* $O(log n)$
	* *Extract-Min/ Extract-Max:* $O(logn)$
	* *Peak:* $O(1)$
	* *Building a Heap*: $O(n)$
* **Common Applications of Heaps:**
	* Priority Queues
	* Heap Sort
	* Dijkstra Algorithm (find shortest path)
	
#### Practice
###### **LeetCode 1046. Last Stone Weight**
#Array #Heap
```
def lastStoneWeight(self, stones):
	if not stones:
		return 0
		
	stone_list = [-s for s in stones]
	heapq.heapify(stone_list)
	
	while len(stone_list) >= 2:
		s1 = heapq.heappop(stone_list)
		s2 = heapq.heappop(stone_list)
		if s1 != s2:
			stones = heapq.heappush(stone_list, -abs(s1-s2))
	return -stone_list[0] if stone_list else 0
```
###### **LeetCode 703. Kth Largest Element in a Stream**
#Tree #Heap 
```
class KthLargest:
	def __init__(self, k: int, nums: List[int]):  
		self.k = k
		self.nums = nums
		heapq.heapify(self.nums)
		while len(self.nums) > self.k:
			heapq.heappop(self.nums)
	
	def add(self, val: int) -> int:
		heapq.heappush(self.nums, val)
		if len(self.nums) > self.k:
			heapq.heappop(self.nums)
		return self.nums[0]
		
```
###### **LeetCode 215. Kth Largest Element in an Array**
#Array #Heap #Quickselect 

* Best solution with Quick Select: [[Search#**LeetCode 215. Kth Largest Element in an Array**]]
* But here we use heap 

```
def findKthLargest(self, nums, k):
	heapq.heapify(nums)
	while len(nums) > k:
		heapq.heappop(nums)
	return nums[0]
```

###### **LeetCode 347. Top K Frequent Elements**
#Heap #Quickselect 
```
def topKFrequent(self, nums, k):
	counts = {}
	for n in nums:
		counts[n] = counts.get(n, 0) + 1
	heap = [(freq, n) for n, freq in counts.items()]
	heapq.heapify(heap)
	while len(heap) > k:
		heapq.heappop(heap)
	return [n[1] for n in heap]
```
###### **LeetCode 218. The Skyline Problem**
#Array #Heap 
```
def getSkyline(self, buildings):
	events = []
	res = []
	heights = []
	active_heights = defaultdict(int)
	heapq.heapify(heights)

	## Get Events
	for l, r, h in buildings:
		events.append((l, h, 's'))
		events.append((r, h, 'e'))

	## Sort Events
	events.sort(key = lambda x:(x[0], x[1] if x[2] == 'e' else -x[1]))

	## Process events
	active_heights[0] = 1
	prev_max = 0
	
	for idx, height, t in events:
		if t == 's':
			active_heights[height] += 1
			heapq.heappush(heights, -height)
		else:
			active_heights[height] -= 1
			if active_heights[height] == 0:
				del active_heights[height]

		while heights and active_heights[-heights[0]] == 0:
			heapq.heappop(heights)
			
		cur_max = -heights[0] if heights else 0
		if prev_max != cur_max:
			res.append([idx, cur_max])
			prev_max = cur_max
			
	return res
```

###### **LeetCode 295. Find Median from Data Stream**
#Sorting #Heap 
```
class MedianFinder:
	def __init__(self):
		self.smalls = [] # Max-heap, -num
		self.bigs = [] 
	
	def addNum(self, num):
		heapq.heappush(self.smalls, -num)
		heapq.heappush(self.bigs, -heapq.heappop(self.smalls))
		while len(self.smalls) < len(self.bigs):
			heapq.heappush(self.smalls, -heapq.heappop(self.bigs))
	
	def findMedian(self):
		if len(self.smalls) == len(self.bigs):
			return (-self.smalls[0] + self.bigs[0])/2
		elif len(self.smalls) > len(self.bigs):
			return -self.smalls[0]	
		else:
			return self.bigs[0]
```
###### **LeetCode 23. Merge k Sorted Lists**
#LinkedList #Heap 
```
def mergeKLists(self, lists):
	heap = [(n.val, idx, n) for idx, n in enumerate(lists) if n]
	heapq.heapify(heap)
	
	pseudo_head = ListNode()
	curr = pseudo_head
	while heap:
		_, i, n = heapq.heappop(heap)
		curr.next = n
		if n.next:
			heapq.heappush(heap, (n.next.val, i, n.next))
		curr = curr.next
	return pseudo_head.next
```
###### **LeetCode 767. Reorganize String**
#Hash #String #Greedy #Heap 
```
def reorganizeString(self, s: str) -> str:
	s_count = {}
	for char in s:
		s_count[char] = s_count.get(char, 0)+1
	heap = [(-freq, char) for char, freq in s_count.items()]
	heapq.heapify(heap)
	
	res = ""
	while len(heap) >= 2:
		f1, c1 = heapq.heappop(heap)
		f2, c2 = heapq.heappop(heap)
		res += (c1+c2)
		if f1 + 1 < 0:
			heapq.heappush(heap, (f1 + 1, c1))
		if f2 + 1 < 0:
			heapq.heappush(heap, (f2 + 1, c2))
			
	if heap:
		f, c = heapq.heappop(heap)
		if f != -1 or (res and c == res[-1]):
			return ""
		else:
			res += c
	return res
```


