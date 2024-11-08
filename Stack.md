#### Knowledge Check
* (LIFO): Last in First Out
* **Operations**
	* *Push:* $O(1)$
	* *Pop*: $O(1)$
	* *Peek:* $O(1)$
	* *Is Empty:* $O(1)$
	* *Size:* $O(1)$
* **Used When
	* Reverse Order Processing
	* Nested Structures: `(), [], functions`
	* State Tracking (Undo)
	* Expression Evaluation
* **Examples:** `Reverse_String, Evaluate_Postfix_Expression, Valid_Parantheses`

#### Practice
###### **LeetCode 20. Valid Parentheses**
#String #Stack 
```
def isValid(self, s):
	stack = []
	match = {
		")": "(",
		"]": "[",
		"}": "{"
	}
	for char in s:
		if char not in match:
			stack.append(char)
		elif not stack or match[char] != stack[-1]:
			return False
		else:
			stack.pop()
	return stack == []
```

###### **LeetCode 155. Min Stack**
#Stack
```
class MinStack:
	def __init__(self):
		self.values = []
		self.mins = []
	
	def push(self, val: int) -> None:
		self.values.append(val)
		if not self.mins or val < self.mins[-1]:
			self.mins.append(val)
		else:
			self.mins.append(self.mins[-1])
	
	def pop(self) -> None:
		self.values.pop()
		self.mins.pop()
	
	def top(self) -> int:
		return self.values[-1]
	
	def getMin(self) -> int:
		return self.mins[-1]

```

###### **LeetCode 739. Daily Temperatures**
#Array #Stack
```
def dailyTemperatures(self, temperatures):
	stack = []
	res = [0 for i in range(len(temperatures))]
	for i, n in enumerate(temperature):
		while stack and n > stack[-1][0]:
			prev_i = stack.pop()[1]
			res[prev_i] = i-prev_i
		stack.append((n, i))
	return res
```

###### **LeetCode 150. Evaluate Reverse Polish Notation**
#Array #Math #Stack 
```
def evalRPN(self, tokens):
	stack = []
	for t in tokens:
		if t == '+':
			stack.append(stack.pop() + stack.pop())
		elif t == '-':
			stack.append(-stack.pop() + stack.pop())
		elif t == '*':
			stack.append(stack.pop() * stack.pop())
		elif t == '/':
			stack.append(int(1/stack.pop() * stack.pop()))
		else:
			stack.append(int(t))
	return stack[-1]
```

###### **LeetCode 84. Largest Rectangle in Histogram**
#Stack #Array
```
def largestRectangleArea(self, heights):
	stack = []
	max_area = 0
	heights.append(0)
	for i, n in enumerate(heights):
		while stack and n < stack[-1][1]:
			h = stack.pop()[1]
			w = i-stack[-1][0]-1 if stack else i
			max_area = max(max_area, w*h)
			stack.append((i, n))
	return max_area
```

###### **LeetCode 42. Trapping Rain Water**
#Stack #TwoPointers #DP 
```
def trap(self, height):
	left_max, right_max = 0, 0
	res = 0
	i, j = 0, len(height)-1
	while i <= j:
		left_max = max(height[l], left_max)
		right_max = max(height[r], right_max)
		if left_max < right_max:
			res += left_max - height[i]
			i += 1
		else:
			res += right_max - height[j]
			j -= 1
	return res
```

###### **LeetCode 225. Implement Stack using Queues**
#Stack #Queue
* Queue is FIFO, first in first out
```
class MyStack:
	def __init__(self):
		self.q = []
			
	def push(self, x: int) -> None:
		self.q.append(x)
		for _ in range(len(self.q)-1):
			self.q.append(self.q.pop(0))
			
	def pop(self) -> int:
		return self.q.pop(0) if not self.empty() else None
	
	def top(self) -> int:
		return self.q[0] if not self.empty() else None
	
	def empty(self) -> bool:
		return self.q == []
```

###### **LeetCode 227. Basic Calculator II**
#Math #String #Stack 
```
def calculate(self, s):
	stack = []
	curnum = 0
	operand = '+'
	for i, n in enumerate(s):
		if n.isdigit():
			curnum = curnum*10 + int(n)
		if n in '+-*/' or i == len(s)-1:
			if n == '+':
				stack.append(curnum)
			elif n == '-':
				stack.append(-curnum)
			elif n == '*':
				stack.append(stack.pop() * curnum)
			elif n == '/':
				stack.append(int(stack.pop() / curnum))
			operand = n
			curnum = 0
	return sum(stack)	
```

