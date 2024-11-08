### Knowledge Check
* **Bit Manipulation**
	* AND: `a & b` (Return True if and only if 2 Trues)
	* OR: `a | b` (Return True if at least 1 True)
	* XOR: `a ^ b` (Return True if and only if 1 True and 1 False)
	* NOT: `~a` Negation
* **Bit Shifting**
	* Left Shift: `001 <<1 --> 010`
	* Right Shift: `100 >>1 --> 010`

### Practice
###### **LeetCode 191. Number of 1 Bits**
#Bit 
```
def hammingWeight(self, n):
	count = 0
	while n > 0:
		count += (n & 1)
		n = n >> 1
	return count
```
###### **LeetCode 231. Power of Two**
#Bit 
```
def isPowerOfTwo(self, n):
	if n <= 0:
		return False
	while n > 0:
		digit = (n & 1)
		n = n >> 1
		if n > 0 and digit == 1:
			return False
	return True
```
###### **LeetCode 338. Counting Bits**
#Bit #DP 
```
def countBits(self, n):
	dp = [0 for i in range(n+1)]
	for i in range(1, n+1):
		if i == 1 or i == 2:
			dp[i] = 1
		elif i % 2 == 0:
			dp[i] = dp[i//2]
		else:
			dp[i] = dp[i-1] + 1
	return dp
```
