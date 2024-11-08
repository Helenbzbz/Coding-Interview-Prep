#### Knowledge Overview
* **BinSearch**
	* Used when: 
		1. target val is sorted 
		2. partially sorted segments
	* *General Steps*
		* `Start & End` index
		* Compute the `mid`
		* Compare `target & mid`
		* `End = mid - 1` or `Start = mid + 1`
	* *PseudoCode*
	```
	def BinSearch(arr, target):
		l, r = 0, len(arr)-1
		while l <= r:
			mid = (l+r)//2
			if arr[mid] == target:
				return mid
			elif arr[mid] < target:
				l = mid + 1
			else:
				r = mid - 1
		return -1
	```

* **Modified BinSearch**
	* Used when:
		1. `arr` is modified (rotated/ elements are modified) 
		2. Multiple Requirements (target range/ leftmost/ rightmost)

* **Examples**: `1st_and_last_pos_of_val_in_sorted_array, sqrt(x), Search_in_Rotated_Sorted_Array, Find_Minimum_in_Sorted_Array`
#### Practice

###### **LeetCode 278. First Bad Version**
#BinSearch 
```
def firstBadVersion(self, n):
	l, r = 1, n
	while l < r:
		mid = (l+r)//2
		if isBadVersion(mid):
			r = mid
		else:
			l = mid + 1
	return r
```

###### **LeetCode 704. Binary Search**
#BinSearch 
```
def search(nums, target):
	l, r = 0, len(nums)-1
	while l <= r:
		mid = (l+r)//2
		if nums[mid] == target:
			return mid
		elif nums[mid] < target:
			l = mid + 1
		else:
			r = mid - 1
	return -1
```

###### **LeetCode 33.  Search in Rotated Sorted Array**
#BinSearch #Array 
```
def search(nums, target):
	l, r = 0, len(nums)-1
	while l <= r:
		mid = (l + r)//2
		if nums[mid] == target:
			return mid
		elif nums[l] <= nums[mid]:
			if nums[l] <= target < nums[mid]:
				r = mid - 1
			else:
				l = mid + 1
		else:
			if nums[mid] < target <= nums[r]:
				l = mid + 1
			else:
				r = mid - 1
	return -1
```

###### **LeetCode 34. Find First and Last Position of Element in Sorted Array**
#Array #BinSearch 
```
def searchRange(self, nums, target):
	if not nums:
		return [-1, -1]
	l, r = 0, len(target)-1
	while l <= r:
		mid = (l+r)//2
		if nums[mid] < target:
			l = mid + 1
		else:
			r = mid - 1
			
	if l >= len(nums) or nums[l] != target: 
		return [-1, -1]
	lower_end = l

	r = len(target)-1 ## DO NOT FORGET TO RESET
	while l <= r:
		mid = (l+r)//2
		if nums[mid] > target:
			r = mid - 1
		else:
			l = mid + 1
	return [lower_end, r]
```

###### **LeetCode 218. The Skyline Problem**
#BinSearch #DivideConquer #Heap 
```
def getSkyline(self, buildings: List[List[int]]) -> List[List[int]]:
	## Initialize lists: events, results, max_heap
	events = []
	results = []
	max_heap = [0] ## Negative Heap
	heapq.heapify(max_heap)
	
	## Create Events
	for l, r, h in buildings:
		events.append((l, h, 's'))
		events.append((r, h, 'e'))

	## Sort Events
	## Prioritize by x-cord then 's' over 'e', taller for start, shorter for end 
	events.sort(key = lambda e:(e[0], 
								e[1] if e[2] == 'e' else -e[1]))

	## Process with max-heap
	active_heights = collections.Counter()
	active_heights[0] = 1
	prev_max_height = 0
	
	for x, h, e in events:
		if e == 's':
			heapq.heappush(max_heap, -h)
			active_heights[h] += 1
		else:
			active_heights[h] -= 1
			if active_heights[h] == 0:
				del active_heights[h]
				
		while max_heap and active_heights[-max_heap[0]] == 0:
			heapq.heappop(max_heap)
			
		cur_max_height = -max_heap[0]
		if cur_max_height != prev_max_height:
			results.append([x, cur_max_height])
			prev_max_height = cur_max_height

	return results
```

###### **LeetCode 215. Kth Largest Element in an Array**
#Array #Sorting #Quickselect

```
def findKthLargest(self, nums, k):

	def quickselect(arr):
		pivot_idx = random.choice(arr)
		smaller, mid, bigger = [], [], []
		
		if n < pivot:
			smaller.append(n)
		elif n > pivot:
			bigger.append(n)
		else:
			mid.append(n)

		if k <= len(bigger):
			return quickselect(bigger, k)
		if len(bigger) + len(mid) < k:
			return quickselect(smaller, k - len(bigger)-len(mid))
		return pivot
		
	return quickselect(nums, k)
```

###### **LeetCode 852. Peak Index in a Mountain Array**
#Array #BinSearch 

```
def peakIndexInMountainArray(self, arr):
	l, r = 0, len(arr)-1
	while l <= r:
		mid = (l+r)//2
		if arr[mid-1] < arr[mid] and arr[mid] > arr[mid+1]:
			return mid
		elif arr[mid+1] > arr[mid]:
			l = mid + 1
		else:
			r = mid - 1
```

###### **LeetCode 981. Time Based Key-Value Store**
#Array #BinSearch #HashTable 

```
class TimeMap:

	def __init__(self):
		self.map = {}
	
	def set(self, key: str, value: str, timestamp: int) -> None:
		if key not in self.map:
			self.map[key] = []
		self.map[key].append([timestamp, value])

	def get(self, key: str, timestamp: int) -> str:
		if not key in self.map:
			return ""
		if timestamp < self.map[key][0][0]:
			return ""

		l, r = 0, len(self.map[key])
		while l < r:
			mid = (l + r)//2
			if self.map[key][mid][0] <= timestamp:
				l = mid + 1
			else:
				r = mid
		return "" if r == 0 else self.map[key][r-1][1]
```

