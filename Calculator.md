#### Knowledge Check
* **Definition**
	* *Expression*: `+, -, *, /`
	* *Operand*: 
		* Number
		* Variable
		* Subexpression
	* *Operators*
		* Higher Levels: `*, /` 
		* Lower Levels: `+, -`
* **Rules**
	1. *Seperation*: 
		* Level 1: handles `+, -`, default `l1 = 0, o1 = +`
		* Level 2: handles `*, /`, default `l2 = 1, op2 = *`
	2. *Precedence*: Operands initially evaluated at Level 2, higher solved first
	3. *Demotion*: Once Level 1 operator or end of expression, demote result of level 2 to level 1
* **Implementation**
	* Number: parse and evaluate at Level 2
	* Variables: Look up and evaluate at Level 2
	* Subexpression: Recursively evaluate inner expressions
	* Operators: Update or Demotion
	* End of expression: Finalize combine l2 and l1
* **Specific Cases**
	* *Basic Calculator I*: `+, -` and level 1
	* *Basic Calculator II:* `+, -, *, /`, No Paren or variable
	* *Basic Calculator III:*
		* Adds support of parentheses
		* Recursive to handle subexpression $O(n^2)$
		* Iterative to handle subexpression $O(n)$
	* *Basic Calculator IV:*
		* `+, -, *` and Variable
		* `Term: [a, b] -> 2 for 2*a*b, Expression: collection of terms` Handle Variable
		* Operations: Addition/Substruction; Multiplication

***PseudoCode***
```
INITIALIZE:
	l1 = 0, o1 = +1
	l2 = 1, o2 = +1
	stack = []

i = 0
LOOP THROUGH:
while i < len(expression)
	c = expression[i]

	HANDLE NUMBER
	if c.isdigit():
		num = 0
		while i < l and expression[i].isdigit():
			num = num*10 + int(expression[i])
			i += 1
		i -= 1

		EVALUATE AT LEVEL 2
		if o2 == 1: ## *
			l2 *= num
		eles:
			l2 //= num

	HANDLE VARIABLE
	elif c.isalpha():
		var = ''
		while i < l and expression[i].isalpha():
			var += expression[i]
			i ++ 1
		i -= 1
		num = map[var]
		
		EVALUATE AT LEVEL 2
		if o2 == 1: ## *
			l2 *= num
		eles:
			l2 //= num
			
	HANDLE SUBEXPRESSION
	elif c == '(':
		stack.append(l1, o1, l2, o2)
		l1 = 0
		o1 = 1
		l2 = 1
		o2 = 1
	elif c == ')':
		l1 += o1*l2
		pl1, po1, pl2, po2 = stack.pop()
		if po2 == 1:
			l2 = pl2*l1
		else:
			l2 = pl2//l1
		l1, o1 = pl1, po1

	HANDLE OPERATOR
	elif c == '+':
		l1 += o1*l2
		o1, l2, o2 = 1, 1, 1
	elif char == '-':
		l1 += o1*l2
		o1, l2, o2 = -1, 1, 1
	elif char == '*':
		o2 = 1
	elif char == '/':
		o2 = -1
	i += 1

l1 += o1*l2
return l1
```