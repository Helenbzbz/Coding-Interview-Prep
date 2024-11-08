#### Knowledge Overview
1. **Insertion Sort**
	* Stable Sort & $O(n^2)$
	* Basic Idea: 
		* Loop through `idx`
		* Start from start from `j = idx-1 to 0`
		* Compare `j + 1` and `j`, Swap number `if nums at j + 1 < nums at j`
		* (In summary, for each idx, we kept moving it forward until it's > numbers on the left and < than the numbers on the right)
	* *Pseudocode*
	```
	def insertionsort(nums):
		for i in range(1, len(nums)):
			j = i - 1
			while j >= 0 and nums[j+1] < nums[j]:
				nums[j+1], nums[j] = nums[j+1], nums[j]
				j -= 1
		return nums
	```

2. **Merge Sort**
	* Stable Sort & $Time: O(nlogn); Space: O(n)$
	* Basic Idea:
		* Find middle index of current array
		* Start at the beginning of 2 subarrays
		* Iteratively merge 2 array together based on value
	* *PseudoCode*
	```
	def mergesort(nums, l, r)
		if r-l+1 <= 1:
			return nums
		m = (l+r)//2
		mergesort(nums, l, m)
		mergesort(nums, m+1, r)
		merge(nums, l, m, r)
		return nums
	```

3. **Quick Sort**
	* Unstable Sort & $Time: O(nlogn), Space: O(n^2)$
	* Relevant to 
		* [[Search#*LeetCode 215. Kth Largest Element in an Array**]]
		* [[Hash#**LeetCode 347. Top K Frequent Elements**]]
	* Basic Idea:
		* Random Puck a value, `end`, `mid`, `random.choice`
		* Iterate to put smaller values to the left and bigger values to the right
		* Based on size of left/right, we move search range
	* *PseudoCode*
	```
	def quicksort(nums, l, r):
		if r - l + 1 <= 1:
			return nums
		pivot = nums[r]
		idx = l
		for i in range(l, r):
			if nums[i] < pivot:
				nums[i], nums[idx] = nums[idx], nums[i]
				idx += 1
		nums[r], nums[idx] = nums[idx], nums[r]
		return idx (idx of pivot value at where is should be)
	```

4. **Bucket Sort**
	* Unstable & $O(1)$ + Should only be used when we are storing arrays with limited unique values
	* Basic Idea: Count Frequency of each element and replace each idx based on frequency of each element

#### Practice
###### **LeetCode 21. Merge Two Sorted Lists**
#LinkedList #Recursion
```
def mergeTwoLists(self, list1, list2):
	pseudo_head = ListNode()
	curr = pseudo_head
	n1, n2 = list1, list2
	while n1 and n2:
		if n1.val < n2.val:
			curr.next = n1
			n1 = n1.next
		else:
			curr.next = n2
			n2 = n2.next
		curr = curr.next
	if n1: curr.next = n1
	if n2: curr.next = n2
	return pseudo_head.next
```

###### **LeetCode 88. Merge Sorted Array**
#Array #TwoPointers #Sorting 
```
def merge(self, nums1, m, nums2, n):
	i, j = m-1, n-1
	for idx in range(n+m-1, -1, -1):
		if j < 0:
			break
		if i >= 0 and nums1[i] > nums2[j]:
			nums1[idx] = nums1[i]
			i -= 1
		else:
			nums1[idx] = nums2[j]
			j -= 1
```

###### **LeetCode 56. Merge Intervals**
#Sorting #Array 
```
def merge(self, intervals):
	intervals.sort()
	res = [intervals[0]]
	for s, e in intervals[1:]
		if s <= res[-1][-1]:
			res[-1][-1] = max(res[-1][-1], e)
		else:
			res.append([s, e])
	return res
```

###### **LeetCode 179. Largest Number**
#Array #String #Greedy #Sorting 
```
def largestNumber(self, nums):

	nums = [str(n) for n in nums]
	
	def quick_sort(l, r):
		if r <= l:
			return
		pivot = (l + r) // 2
		idx = partition(l, r, pivot)
		quick_sort(l, idx - 1)
		quick_sort(idx + 1, r)
	
	def partition(l, r, pivot):
		idx = l
		pivot_val = nums[pivot]
		nums[pivot], nums[r] = nums[r], nums[pivot] 
		for i in range(l, r):
			if nums[i] + pivot_val > pivot_val + nums[i]:
				nums[idx], nums[i] = nums[i], nums[idx]
				idx += 1
		nums[idx], nums[r] = nums[r], nums[idx]
		return idx
		
	quick_sort(0, len(nums) - 1)
	largest_num = "".join(nums)
	return "0" if largest_num[0] == "0" else largest_num
	
```

###### **LeetCode 315. Count of Smaller Numbers After Self**
#BinSearch #Array 
```
def countSmaller(self, nums):
	if not nums:
		return []
	counts = [0] * len(nums)
	pairs = [(nums[i], i) for i in range(len(nums))]

	def mergesort(l, r):
		if r - l <= 1:
			return
		mid = (l+r)//2
		mergesort(l, mid)
		mergesort(mid, r)

		tmp = []
		i, j = l, mid
		while i < mid and j < r:
			if pairs[i][0] <= pairs[j][0]:
				tmp.append(pairs[j])
				j += 1
			else:
				counts[pairs[i][1]] += (r - j)
				tmp.append(pairs[i])
				i += 1

		while i < mid:
			tmp.append(pairs[i])
			i += 1
		while j < r:
			tmp.append(pairs[j])
			j += 1

		for i in range(l, r):
			pairs[i] = tmp[i-l]

	mergesort(0, len(nums))
	return counts
```

###### **LeetCode 493. Reverse Pairs**
#BinSearch #Array 
```
def reversePairs(nums):
	def mergecount(l, r):
		## 1. Handle Base case if length = 0/1
		if r - l <= 1:
			return 0
			
		## 2. Find mid and get count within left and right
		mid = (l+r)//2
		count = mergecount(l, mid) + mergecount(mid, r)
	
		## 3. Count and compare left and right
		j = mid
		for i in range(l, mid):
			while j < r and nums[i] > 2*nums[j]:
			j += 1
		count += j - mid
		
		## 4. Merge Sort and Copy Back
		tmp = []
		i, j = l, mid
		while i < mid and j < r:
			if nums[i] < nums[j]:
			tmp.append(nums[i])
			i += 1
		else:
			tmp.append(nums[j])
			j += 1
	
		while i < mid:
			tmp.append(nums[i])
			i += 1
		while j < r:
			tmp.append(nums[j])
			j += 1
		
		for i in range(l, r):
			nums[i] = tmp[i-l]
		return count
	
	return mergecount(0, len(nums))
```

###### **LeetCode 148. Sort List**
#LinkedList #TwoPointers 
```
def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
	if not head or not head.next:
		return head
	mid = self.findmid(head)
	l, r = self.sortList(head), self.sortList(mid)
	return self.merge(l, r)
	
def findmid(self, node):
	slow, fast = node, node.next
	while fast and fast.next:
		slow = slow.next
		fast = fast.next.next
	mid = slow.next
	slow.next = None
	return mid
	
def merge(self, n1, n2):
	pseudo_head = ListNode()
	curr = pseudo_head
	while n1 and n2:
		if n1.val < n2.val:
			curr.next = n1
			n1 = n1.next
		else:
			curr.next = n2
			n2 = n2.next
		curr = curr.next
	if n1: curr.next = n1
	if n2: curr.next = n2
	return pseudo_head.next
```

###### **LeetCode 347. Top K Frequent Elements**
[[Hash#**LeetCode 347. Top K Frequent Elements**]]





