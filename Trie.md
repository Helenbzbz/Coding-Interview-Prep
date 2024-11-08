#### Knowledge Check
* **Overview**
	* Trie/ Prefix Tree: Store a set of strings or words
	* Useful for prefix matching, autocomplete features, and dictionary storage
* **Key Components**
	* ***Node***: Each node contains: `val str, children [], is_word bool`
	- ***Root***: Starting point, empty node
- **Operations**:
    - *Insertion*: $O(m)$
    - *Search*: $O(m)$
* **Implementation**
```
class Node:
	def __init__(self):
		self.children = {}
		self.end = False

class Trie:
	def __init__(self):
		self.root = Node()

	def insert(self, word):
		curr = self.root
		for char in word:
			if char not in curr.children:
				curr.children[char] = Node()
			curr = curr.children[char]
		curr.end = True

	def search_word(self, word):
		curr = self.root
		for char in word:
			if char not in curr.children:
				return False
			curr = curr.children[char]
		return curr.end

	def search_prefix(self, prefix):
		curr = self.root
		for char in prefix:
			if char not in curr.children:
				return False
			curr = curr.children[char]
		return True
```

#### Practice

###### LeetCode 208. Implement Trie (Prefix Tree)  
#Trie #Design  
```
class Node:
	def __init__(self):
		self.children = {}
		self.end = False
		
class Trie:
	def __init__(self):
		self.root = Node()

	def insert(self, word):
		curr = self.root
		for char in word:
			if char not in curr.children:
				curr.children[char] = Node()
			curr = curr.children[char]
		curr.end = True
	
	def search(self, word):
		curr = self.root
		for char in word:
			if char not in curr.children:
				return False
			curr = curr.children[char]
		return curr.end
		
	def startsWith(self, prefix):
		curr = self.root
		for char in prefix:
			if char not in curr.children:
				return False
			curr = curr.children[char]
		return True
```
###### LeetCode 211. Add and Search Word - Data structure design  
#Trie #Design  
```
class Node:
	def __init__(self):
		self.children = {}
		self.end = False
		
class WordDictionary:
	def __init__(self):
		self.root = Node()
		
	def addWord(self, word):  
		curr = self.root
		for char in word:
			if char not in curr.children:
				curr.children[char] = Node()
			curr = curr.children[char]
		curr.end = True
	
	def search(self, word):
		curr = [self.root]
		for char in word:
			new_curr = []
			if char == '.':
				for curr_n in curr:
					for next_n in curr_n.children.values():
						new_curr.append(next_n)
			else:
				for curr_n in curr:
					if char in curr_n.children:
						new_curr.append(curr_n.children[char])
			
			if not new_curr:
				return False
			curr = new_curr

		for n in curr:
			if n.end:
				return True
		return False
```

###### LeetCode 212. Word Search II  
#Trie #Backtracking  
```
class Node:
	def __init__(self):
		self.children = {}
		self.word = None

class Trie:
	def __init__(self):
		self.root = Node()

	def add_word(self, word):
		curr = self.root
		for char in word:
			if char not in curr.children:
				curr.children[char] = Node()
			curr = curr.children[char] 
		curr.word = word

class Solution:
	def findWords(self, board, words):
		def dfs(r, c, node):
			if r < 0 or c < 0 or c >= C or r >= R or (r, c) in visited or board[r][c] not in node.children:
				return
			visited.add((r, c))
			node = node.children[board[r][c]]
			if node.word:
				res.add(node.word)
			dfs(r+1, c, node)
			dfs(r-1, c, node)
			dfs(r, c+1, node)
			dfs(r, c-1, node)
			visited.remove((r, c))

		R, C = len(board), len(board[0])
		T = Trie()
		res = set()
		for w in words:
			T.add_word(w)
		for r in range(R):
			for c in range(C):
				visited = set()
				dfs(r, c, T.root)
		return list(res)
```
###### LeetCode 1268. Search Suggestions System  
#Trie #String  
```
class Node:
	def __init__(self):
		self.children = {}
		self.word = None

class Trie:
	def __init__(self):
		self.root = Node()

	def add_word(self, word):
		curr = self.root
		for char in word:
			if char not in curr.children:
				curr.children[char] = Node()
			curr = curr.children[char] 
		curr.word = word

	def search_prefix(self, prefix):
		curr = self.root
		for char in prefix:
			if char not in curr.children:
				return False
			curr = curr.children[char] 
		return curr

class Solution:
	def suggestedProducts(self, products, searchWord):
		res = []
		
		def dfs(node):
			if len(cur_res) == 3:
				return 
			if node.word:
				cur_res.append(node.word)
			if not node.children:
				return
			for char in sorted(node.children):
				dfs(node.children[char])

		T = Trie()
		products.sort()
		for p in products:
			T.add_word(p)
		for i in range(len(searchWord)):
			cur_res = []
			n = T.search_prefix(searchWord[:i+1])
			if n:
				dfs(n)
			res.append(cur_res)
		return res	
```
###### LeetCode 648. Replace Words  
#Trie #HashTable  
```
class Node:
	def __init__(self):
		self.children = {}
		self.word = None

class Trie:
	def __init__(self):
		self.root = Node()

	def add_word(self, word):
		curr = self.root
		for char in word:
			if char not in curr.children:
				curr.children[char] = Node()
			curr = curr.children[char] 
			if curr.word:
				return
		curr.word = word

	def search_prefix(self, word):
		curr = self.root
		for char in word:
			if char not in curr.children:
				return False
			curr = curr.children[char] 
			if curr.word:
				return curr.word
				
class Solution:
	def replaceWords(self, dictionary, sentence):
		T = Trie()
		for w in dictionary:
			T.add_word(w)
		words = sentence.split(" ")
		for i in range(len(words)):
			tmp = T.search_prefix(words[i])
			if tmp:
				words[i] = tmp
		return ' '.join(words)
```
###### LeetCode 820. Short Encoding of Words  
#Trie #String  
```
class Node:
	def __init__(self):
		self.children = {}
		self.word = None

class Trie:
	def __init__(self):
		self.root = Node()

	def add_word(self, word):
		curr = self.root
		for char in word:
			if char not in curr.children:
				curr.children[char] = Node()
			curr = curr.children[char] 
		curr.word = word
		
def minimumLengthEncoding(self, words):
	T = Trie()
	for w in words:
		T.add(w[::-1])
	def dfs(n):
		nonlocal res
		if not n.children:
			res += len(n.word)+1
			return 
		for c in n.children:
			dfs(n.children[c])
	res = 0
	dfs(T.root)
	return res
```