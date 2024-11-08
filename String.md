#### Knowledge Check & Common Operation

* Length: `len(s)`, $O(1)$
* Access i-th Character: `s[i]`, $O(1)$
* Concatenate: 
	* $O(n^2)$: `res = ""; for i in range(1000): res += str(i)`, Each time, new string is created
	* $O(n)$: `"".join([str(i) for i in range(1000)])`
* Substring: `s[start, end]`, $O(k)$
* Contains: `substr in s`, $O(n)$
* Starts with/ End with: `s.startswith('prefix')`/ `s.endswith('prefix')`, $O(k/len(prefix))$
* Comparison: `s1 == s2`, `s1 > s2`, `s1 < s2`, $O(n)$
* Conversion: 
	* `int(s)`, $O(n)$
	* `str(i)`, $O(log(i))$
* Character Code: `ord(), chr()`, $O(1)$
* UpperCase/ LowerCase: `.isupper(), .islower()`, $O(n)$

#### Practice
###### **LeetCode 20. Valid Parentheses**
#String #Stack

```
def isValid(self, s):
	matching = {
		')': '(',
		']': '[',
		'}': '{'
	}
	stack = []
	for char in s:
		if stack and char in matchingand matching[char] == stack[-1]:
			stack.pop()
		else:
			stack.append(char)
	return stack == []
```

###### **LeetCode 125. Valid Palindrome**
#String #TwoPointers 

```
def isPalindrome(self, s):
	i, j = 0, len(s)-1
	while i < j:
		if not s[i].isalnum():
			i += 1
		elif not s[j].isalnum():
			j -= 1
		elif s[i].lower() == s[j].lower():
			i += 1
			j -= 1
		else:
			return False
	return True	
	# Time: O(n), Space: O(1)	
```

###### **LeetCode 3. Longest Substring Without Repeating Characters**
#String #Hash #SlidingWindow 

```
def lengthOfLongestSubstring(self, s):
	i = 0
	seen = set()
	longest = 0
	for j in range(len(s)):
		while s[j] in seen:
			seen.remove(s[i])
			i += 1
		seen.add(s[j])
		longest = max(longest, j-i+1)
	return longest
	## Time: O(n), Space: O(n)
```

###### **LeetCode 5. Longest Palindromic Substring**
#String #TwoPointers #DP

```
def longestPalindrome(self, s):
	def check_palin(l, r):
		nonlocal res
		while l >= 0 and r < len(s) and s[l] == s[r]:
			if res[1]-res[0] < r-l:
				res = (l, r)
			l -= 1
			r += 1
		res = (0, 0)
		for i in range(len(s)):
			check_palin(i, i)
			check_palin(i, i+1)
		return s[res[0]: res[1]+1]
	# Time: O(n^2), Space: O(1)		
```

###### **LeetCode 76. Minimum Window Substring**
#String #SlidingWindow #Hash 

```
def minWindow(self, s, t):
	res = (-1, len(s))
	i = 0
	count_t = {}
	for char in t:
		count_t[char] = count_t.get(char, 0) + 1
	conditions = len(count_t)

	for j in range(len(s)):
		if s[j] in count_t:
			count_t[s[j]] -= 1
			if count_t[s[j]] == 0:
				conditions -= 1
			while conditions == 0:
				if j - i < res[1] - res[0]:
					res = (i, j)
				if s[i] in count_t:
					count_t[s[i]] += 1
					if count_t[s[i]] > 0:
						conditions += 1
				i += 1
	return s[res[0]: res[1]+1] if res[0] != -1 else ""
```

###### **LeetCode 30. Substring with Concatenation of All Words**
#Hash #String #SlidingWindow 
```
def findSubstring(self, s, words):
	def findvalid(start):
		count_curr = {}
		j = start
		for i in range(start, len(s), n):
			w = s[i:i+n]
			if w not in word_count:
				j = i+n
				count_curr.clear()
			else:
				count_curr[w] = count_curr.get(w, 0)+1
				if count_curr == word_count:
					res.append(j)
				if i-j+n >= l:
					count_curr[s[j:j+n]] -= 1
					j += n
	res = []
	word_count = {}
	n, l = len(words[0]), len(words) * len(words[0])
	for w in words:
		word_count[w] = word_count.get(w, 0) + 1
	for start in range(0, n):
		findvalid(start)
	return res
	## Time: O(l*n*s); Space: O(n*l)
```

###### **LeetCode 14. Longest Common Prefix**
#Hash #String #Tire

```
class Node:
	def __init__(self, val = None, length = 0):
		self.children = {}
		self.end = False
		
class TrieNode:
	def __init__(self, words):
		self.head = Node()
		for w in words:
			curr = self.head
			for char in w:
				if char not in curr.children:
					curr.children[char] = Node()
				curr = curr.children[char]
			curr.end = True
		
				
def longestCommonPrefix(self, strs):
	if not strs or "" in strs:
		return ""
	T = TrieNode(strs)
	curr = T.head
	prefix = ""
	while len(curr.children) == 1 and not curr.end:
		char, next_node = next(iter(curr.children.items()))
		prefix += char
		curr = next_node
	return prefix
	
	# Time: O(n)
```

###### **LeetCode 49. Group Anagrams** 
#String #Hash

```
def groupAnagrams(strs):
	groups = defaultdict(list)
	for s in strs:
		idx = [0 for i in range(26)]
		for c in s:
			idx[ord(c)-ord('a')] += 1
		groups[tuple(idx)].append(s)
	return list(groups.values())
```

###### **Meta 1. Rotational Cipher**
#String 
- Upper Case Letter has different `ord` comparing to Lower Case Letter
- `ord('a')` is the base, but it is not 0
- integers is rotated every 10; letters is rotated every 26
```
def rotationalCipher(input_str, rotation_factor):
  res = ""
  for char in input_str:
    if char.isalpha():
      new_char = chr((ord(char.lower())-ord('a')+ rotation_factor%26)%26+ord('a'))
      if char.isupper():
        res += new_char.upper()
      else:
        res += new_char
    elif char.isnumeric():
      res += str((int(char) + rotation_factor%10)%10)
    else:
      res += char
  return res
```

###### **Meta 2. Matching Pairs**
#String #Hash 
```
def matching_pairs(s, t):
	  from collections import defaultdict
	  matches = 0
	  unmatches = []
	  for i in range(len(s)):
	    if s[i] == t[i]:
	        matches += 1
	    else:
	        unmatches.append((s[i], t[i]))
  
	  for i in range(len(unmatches)):
	    sc1, tc1 = unmatches[i]
	    for j in range(i+1, len(unmatches)):
	      sc2, tc2 = unmatches[j]
	      if sc1 == tc2 and sc2 == tc1:
	        return matches + 2
	      elif sc1 == tc2 or sc2 == tc1:
	        swap = True
	        
	  if len(unmatches) and swap:
	    return matches + 1
	    
	  return matches - 2
```

###### **Meta 3. Minimum Length Substrings**
#Hash #SlidingWindow #String 

```
def min_length_substring(s, t):
  count_t = {}
  for char in t:
    count_t[char] = count_t.get(char, 0) + 1
  condition = len(count_t)
  
  i = 0
  res = (-1, len(s))
  
  for j in range(len(s)):
    if s[j] in count_t:
      count_t[s[j]] -= 1
      if count_t[s[j]] == 0:
        condition -= 1
        
      while condition == 0:
        if j-i < res[1]-res[0]:
          res = (i, j)
        if s[i] in count_t:
          count_t[s[i]] += 1
          if count_t[s[i]] > 0:
            condition += 1
        i += 1
        
  return res[1]-res[0]+1 if res[0] != -1 else -1
  ```