Relevant: [[Array]]
#### Knowledge Check
- **Use When**:
	- **Sorted Arrays**: Find pairs or triplets with specific sums
	- **Palindrome Checks**: Compare characters from the beginning and end of a string
	- **Container Problems**: Determine the maximum area or volume between boundaries

#### Practice

###### LeetCode 167. Two Sum II - Input Array Is Sorted  
#Array #TwoPointers  
```
def twoSum(self, numbers, target):
	i, j = 0, len(numbers)-1
	while i < j:
		cur_sum = numbers[i] + numbers[j]
		if cur_sum == target:
			return [i+1, j+1]
		elif cur_sum < target:
			i += 1
		else:
			j -= 1
```
###### LeetCode 11. Container With Most Water  
#Array #TwoPointers  
```
def maxArea(self, height):
	l, r = 0, len(height)-1
	maxarea = 0
	while l < r:
		maxarea = max(maxarea, (r-l)*min(height[l], height[r]))
		if height[l] < height[r]:
			l += 1
		else:
			r -= 1
	return maxarea
```

###### LeetCode 16. 3Sum Closest  
#Array #TwoPointers  
```
def threeSumClosest(self, nums, target):
	nums.sort()
	closest = float('inf')
	
	for i in range(len(nums)):
		j, k = i+1, len(nums)-1
		while j < k:
			cur_sum = nums[i] + nums[j] + nums[k]
			if cur_sum == target:
				return target
			if abs(cur_sum - target) < abs(closest - target):
				closest = cur_sum
			if cur_sum < target:
				j += 1
			else:
				k -= 1
	return closest
```

###### LeetCode 15. 3Sum  
#Array #TwoPointers  
```
def threeSum(self, nums):
	solution = set()
	nums.sort()
	for i in range(len(nums)):
		if nums[i] > 0:
			break
		j, k = i+1, len(nums)-1
		while j < k:
			cur_sum = nums[i] + nums[j] + nums[k]
			if cur_sum == 0:
				solution.add((nums[i], nums[j], nums[k]))
			elif cur_sum < 0:
				j += 1
			else:
				k -= 1
	return list(solution)
```

