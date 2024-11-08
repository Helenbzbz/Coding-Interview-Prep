#### Knowledge Check
* **Basic Idea:** 
	* Solve problems that involve finding subarrays or substrings within a larger array
	* Reduces the time complexity from $O(n^2)$ (nested loop) to $O(n)$ by maintaining a range (window) over the data structure.
* **Types:**
	- **Fixed-Size Window**: The window has a constant size
	- **Dynamic-Size Window**: The window size can expand or contract based on conditions (e.g., sum, character frequency).
* **Common Use Cases:**
	- *Maximum/Minimum Sums*
	- *Substring Problems*: Longest/shortest substring that meets specific criteria.
	- *Distinct Elements:* Counting distinct elements within a window.
* **Tips:**
	- **hash map** or **counter** to keep track of elements or characters within the current window
	- Optimize by minimizing unnecessary movements

#### Practice
###### LeetCode 643. Maximum Average Subarray I  
#Array #SlidingWindow  
```
def findMaxAverage(self, nums, k):
	cur_sum = sum(nums[:k])
	maxavg = cur_sum/k
	for i in range(k, len(nums)):
		cur_sum -= nums[i-k]
		cur_sum += nums[i]
		maxavg = max(maxavg, cur_sum/k)
	return maxavg
```
######  LeetCode 424. Longest Repeating Character Replacement  
#String #SlidingWindow  
```
def characterReplacement(self, s, k):
	l = 0
	count_t = {}
	max_length = 0
	max_count = 0
	for i in range(len(s)):
		count_t[s[i]] = count_t.get(s[i], 0) + 1
		max_count = max(max_count, count_t[s[i]])
		while (i - l + 1) - max_count > k:
			count_t[s[l]] -= 1
			l += 1
		max_length = max(max_length, (i - l + 1))
	return max_length
```
######  LeetCode 239. Sliding Window Maximum  
#Array #Heap #SlidingWindow  
```
def maxSlidingWindow(self, nums, k):
	res = []
	dq = deque()
	for i in range(len(nums)):
		while dq and (i - dq[0] + 1) > k:
			dq.popleft()
		while dq and nums[dq[-1]] < nums[i]:
			dq.pop()
		dq.append(i)
		if i >= k-1:
			res.append(nums[dq[0]])
	return res
```

###### LeetCode 3. Longest Substring Without Repeating Characters  
#String #SlidingWindow  
```
def lengthOfLongestSubstring(self, s):
	seen = set()
	l = 0
	max_length = 0
	for i in range(len(s)):
		while s[i] in seen:
			seen.remove(s[l])
			l += 1
		seen.add(s[i])
		max_length = (max_length, i - l + 1)
	return max_length
```

######  LeetCode 340. Longest Substring with At Most K Distinct Characters  
#String #SlidingWindow  
```
def lengthOfLongestSubstringKDistinct(self, s, k):
	w_freq = {}
	l = 0
	length = 0 
	for i in range(len(s)):
		w_freq[s[i]] = w_freq.get(s[i], 0) + 1
		while len(w_freq) > k:
			w_freq[s[l]] -= 1
			if w_freq[s[l]] == 0:
				del w_freq[s[l]]
			l += 1
		length = max(length, i - l + 1)
	return length
```
