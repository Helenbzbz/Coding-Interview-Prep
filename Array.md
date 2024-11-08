#### Knowledge Check
1. **Static Array**
	* Read: $O(1)$
	* Insert: $O(n)$ shift all values and insert
	* Remove: $O(1)$ by put 0
2. **Dynamic Array**
	* When length > size, allocate new array and free original memory (E.g. size. = 4 and length = 3) + double the size
	* Insert/ Remove End: $O(1)$
	* Insert/ Remove Middle: $O(n)$
#### Basic Algorithms
1. **Two Pointers**
	[[Two Pointers]]
	- Linear Data Structure
	- Process Pairs
	- Dynamic Pointer Moving
	```
	l, r = 0, len(arrays)-1
	while l < r:
	iterate & update l/r based on conditions
	```
	**Examples:**  `is_Palindrone, Reverse_Array, Two_Sum, ThreeSum, Container_With_Most_Water, Product_Except_Itself, Remove_Nodes_From_End_of_List`
	
2. **Fast and Slow Pointers**
	* Linear Data Structure
	* Cycle/ Intersection 
	* Find Quantiles
	```
	slow, fast = 0, 0
	slow + & fast ++
	```
	**Examples:** `middle_of_LinkedList, Detect_Cycle`
	
3. **Sliding Window**
	[[Sliding Window]]
#### Practices

###### **LeetCode 1. Two Sum** 
#Array #HashTable #Easy

```
def twoSum(self, nums, target):
	seen = {}
	for i, n in enumerate(nums):
		if target - n not in seen:
			seen[n] = i
		else:
			return [seen[target-n], i]
	## Time: O(n); Space: O(n)
```

###### **LeetCode26. Remove Duplicates from Sorted Array**
#Array #TwoPointers

```
def removeDuplicates(self, nums):
	i, j = 0, 0
	while i < len(nums):
		nums[j] = nums[i]
		j += 1
		i += 1
		while i < len(nums) and nums[i] == nums[i-1]:
			i += 1
	return j
	## Time: O(n); Space O(1)
```

###### **LeetCode 238. Product of Array Except Self**
#Array #PrefixSum

```
def productExceptSelf(self, nums):
	res = [1]
	for n in nums[:-1]:
		res.append(res[-1]*n)
	right_prod = 1
	for i in range(len(nums)-1, -1, -1):
		res[i]*= right_prod
		right_prod *= nums[i]
	return res
	## Time: O(n); Space: O(1) --> res is not additional space
```

###### **LeetCode 209. Minimum Size Subarray Sum**
#Array #PrefixSum #BinSearch #SlidingWindow

```
def minSubArrayLen(self, target, nums):
	min_length = float('inf')
	i, j = 0, 0
	cur_sum = 0
	while j < len(nums):
		cur_sum += nums[j]
		j += 1
		while cur_sum >= target:
			min_length = min(min_length, j-i)
			cur_sum -= nums[i]
			i += 1
	return min_length if min_length != float('inf') else 0
	## Time: O(n); Space: O(1)
```

###### **LeetCode 42. Trapping Rain Water**
#Array #TwoPointers 

```
def trap(self, height):
	left_max = []
	res = 0
	for h in height:
		if left_max and h < left_max[-1]:
			left_max.append(left_max[-1])
		else:
			left_max.append(h)
	right_max = -1
	for i in range(len(height)-1, -1, -1):
		right_max = max(right_max, height[i])
		res += min(right_max, left_max[i]) - height[i]
	return res
	## Time: O(n); Space: O(n)
```

```
def trap(self, height):
	l, r = 0, len(height)-1
	left_max, right_max = 0, 0
	res = 0
	while l < r:
		if heigh[l] < height[r]:
			left_max = max(left_max, height[l])
			res += left_max - height[l]
			l += 1
		else:
			right_max = max(right_max, height[r])
			res += right_max - height[r]
			r -= 1
	return res
```

###### **LeetCode 135. Candy**
#Array #Greedy

```
def candy(self, ratings):
	## Logic, Left or Right climbing mountain :)
	candies = [1 for i in range(len(ratings))]
	for i in range(1, len(ratings)):
		if ratings[i] > ratings[i-1]:
			candies[i] = candies[i-1] + 1
	res = candies[-1]
	for i in range(len(ratings)-2, -1, -1):
		if ratings[i] > ratings[i+1]:
			candies[i] = max(candies[i], candies[i+1]+1)
		res += candies[i]
	return res
	## Time: O(n); Space: O(n)
```

###### **LeetCode 53. Maximum Subarray**
#Array #DP

```
def maxSubArray(self, nums):
	largest_sum = float('-inf')
	cur_sum = 0
	for n in nums:
		cur_sum = max(cur_sum + n, n)
		largest_sum = max(largest_sum, cur_sum)
	return largest_sum
	## Time: O(n); Space: O(1)
```

###### **LeetCode 152. Maximum Product Subarray**
#Array #DP

```
def maxProduct(self, nums):
	smallest, biggest, res = nums[0], nums[0], nums[0]
	for n in nums[1:]:
		candidate1, candidate2 = smallest*n, biggest*n
		smallest = min(candidate1, candidate2, n)
		biggest = max(candidate1, candidate2, n)
		res = max(res, biggest)
	return res
	## Time: O(n); Space: O(1)
```

###### **Meta 2. Yearbook Counts**
#Array #CycleDetection

```
def findSignatureCounts(arr):
	n = len(arr)
	visited = [False for i in range(n)]
	res = [0 for i in range(n)]
	for i in range(n):
		if not visited[i]:
			cycle = []
			curr = i
	    while not visited[curr]:
	        visited[curr] = True
	        cycle.append(curr)
	        curr = arr[curr]-1
	    for student in cycle:
		    res[student] = len(cycle)
  return res
```

###### **Meta3. Count SubArrays**
#Array  #Stack

```
def countSubArray(arr):
	n = len(arr)
	
	left_stack = []
	left_res = [0 for i in range(n)]
	for i in range(n):
		while left_stack and arr[left_stack[-1]] < arr[i]:
			left_stack.pop()
		left_res[i] = i-left_stack[-1] if left_stack else i+1
		left_stack.append(i)
		
	right_stack = []
	right_res = [0 for i in range(n)]
	for i in range(n):
		while right_stack and arr[right_res[-1]] < arr[i]:
			right_stack.pop()
		right_res[i] = right_stack[-1]-i if right_stack else n-i
		right_stack.append(i)
	
	return [left_res[i] + right_res[i] -1 for i in range(n)]
```
