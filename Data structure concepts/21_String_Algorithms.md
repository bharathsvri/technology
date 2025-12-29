# String Algorithms

## Definition
String algorithms are techniques for efficiently processing, searching, and manipulating strings. They are fundamental in text processing, pattern matching, and bioinformatics.

## Important String Algorithms

### 1. Pattern Matching Algorithms

#### Naive Pattern Matching
**Time Complexity**: O(m × n) where m=pattern length, n=text length

```python
def naive_search(text, pattern):
    n, m = len(text), len(pattern)
    result = []
    
    for i in range(n - m + 1):
        if text[i:i + m] == pattern:
            result.append(i)
    
    return result
```

---

#### Knuth-Morris-Pratt (KMP) Algorithm
**Time Complexity**: O(m + n)
**Space Complexity**: O(m)

**Key Idea**: Preprocess pattern to avoid unnecessary comparisons

```python
def kmp_search(text, pattern):
    def build_lps(pattern):
        """Build Longest Proper Prefix which is also Suffix array"""
        m = len(pattern)
        lps = [0] * m
        length = 0
        i = 1
        
        while i < m:
            if pattern[i] == pattern[length]:
                length += 1
                lps[i] = length
                i += 1
            else:
                if length != 0:
                    length = lps[length - 1]
                else:
                    lps[i] = 0
                    i += 1
        
        return lps
    
    n, m = len(text), len(pattern)
    if m == 0:
        return []
    
    lps = build_lps(pattern)
    result = []
    i = j = 0  # i for text, j for pattern
    
    while i < n:
        if text[i] == pattern[j]:
            i += 1
            j += 1
        
        if j == m:
            result.append(i - j)
            j = lps[j - 1]
        elif i < n and text[i] != pattern[j]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
    
    return result
```

---

#### Rabin-Karp Algorithm
**Time Complexity**: Average O(n + m), Worst O(n × m)
**Key Idea**: Use rolling hash to compare substrings

```python
def rabin_karp_search(text, pattern):
    n, m = len(text), len(pattern)
    if m == 0:
        return []
    
    base = 256
    mod = 101  # Prime number
    
    # Calculate hash of pattern and first window
    pattern_hash = 0
    text_hash = 0
    h = 1
    
    # h = base^(m-1) % mod
    for i in range(m - 1):
        h = (h * base) % mod
    
    # Calculate initial hashes
    for i in range(m):
        pattern_hash = (base * pattern_hash + ord(pattern[i])) % mod
        text_hash = (base * text_hash + ord(text[i])) % mod
    
    result = []
    
    # Slide the pattern over text
    for i in range(n - m + 1):
        # Check if hashes match
        if pattern_hash == text_hash:
            # Verify actual match (to handle hash collisions)
            if text[i:i + m] == pattern:
                result.append(i)
        
        # Calculate hash for next window
        if i < n - m:
            text_hash = (base * (text_hash - ord(text[i]) * h) + ord(text[i + m])) % mod
            # Make sure hash is positive
            if text_hash < 0:
                text_hash += mod
    
    return result
```

---

#### Z-Algorithm (Z-Array)
**Time Complexity**: O(n + m)
**Key Idea**: Precompute Z-array for pattern matching

```python
def z_algorithm(s):
    """Build Z-array for string s"""
    n = len(s)
    z = [0] * n
    left = right = 0
    
    for i in range(1, n):
        if i <= right:
            z[i] = min(right - i + 1, z[i - left])
        
        while i + z[i] < n and s[z[i]] == s[i + z[i]]:
            z[i] += 1
        
        if i + z[i] - 1 > right:
            left = i
            right = i + z[i] - 1
    
    return z

def z_search(text, pattern):
    """Search pattern in text using Z-algorithm"""
    combined = pattern + '$' + text
    z = z_algorithm(combined)
    result = []
    
    for i in range(len(pattern) + 1, len(combined)):
        if z[i] == len(pattern):
            result.append(i - len(pattern) - 1)
    
    return result
```

---

### 2. String Manipulation

#### Reverse Words in String
```python
def reverse_words(s):
    words = s.split()
    return ' '.join(words[::-1])

def reverse_words_in_place(s):
    # Convert to list for in-place modification
    chars = list(s)
    
    # Reverse entire string
    def reverse(start, end):
        while start < end:
            chars[start], chars[end] = chars[end], chars[start]
            start += 1
            end -= 1
    
    reverse(0, len(chars) - 1)
    
    # Reverse each word
    start = 0
    for i in range(len(chars)):
        if chars[i] == ' ':
            reverse(start, i - 1)
            start = i + 1
    reverse(start, len(chars) - 1)  # Reverse last word
    
    return ''.join(chars)
```

---

#### Valid Anagram
```python
def is_anagram(s, t):
    if len(s) != len(t):
        return False
    
    count = {}
    for char in s:
        count[char] = count.get(char, 0) + 1
    
    for char in t:
        if char not in count:
            return False
        count[char] -= 1
        if count[char] < 0:
            return False
    
    return True

# Using sorting
def is_anagram_sort(s, t):
    return sorted(s) == sorted(t)
```

---

#### Group Anagrams
```python
def group_anagrams(strs):
    groups = {}
    
    for s in strs:
        key = ''.join(sorted(s))
        if key not in groups:
            groups[key] = []
        groups[key].append(s)
    
    return list(groups.values())
```

---

#### Longest Common Prefix
```python
def longest_common_prefix(strs):
    if not strs:
        return ""
    
    prefix = strs[0]
    for i in range(1, len(strs)):
        while not strs[i].startswith(prefix):
            prefix = prefix[:-1]
            if not prefix:
                return ""
    
    return prefix

# Vertical scanning
def longest_common_prefix_vertical(strs):
    if not strs:
        return ""
    
    for i in range(len(strs[0])):
        char = strs[0][i]
        for j in range(1, len(strs)):
            if i >= len(strs[j]) or strs[j][i] != char:
                return strs[0][:i]
    
    return strs[0]
```

---

#### Valid Parentheses
```python
def is_valid_parentheses(s):
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in mapping:
            if not stack or stack.pop() != mapping[char]:
                return False
        else:
            stack.append(char)
    
    return not stack
```

---

### 3. Palindrome Algorithms

#### Check Palindrome
```python
def is_palindrome(s):
    left, right = 0, len(s) - 1
    
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    
    return True

# Case-insensitive, alphanumeric only
def is_palindrome_clean(s):
    cleaned = ''.join(char.lower() for char in s if char.isalnum())
    return cleaned == cleaned[::-1]
```

---

#### Longest Palindromic Substring
```python
def longest_palindrome(s):
    def expand_around_center(left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return s[left + 1:right]
    
    if not s:
        return ""
    
    longest = ""
    for i in range(len(s)):
        # Odd length palindrome
        odd = expand_around_center(i, i)
        if len(odd) > len(longest):
            longest = odd
        
        # Even length palindrome
        even = expand_around_center(i, i + 1)
        if len(even) > len(longest):
            longest = even
    
    return longest
```

---

#### Longest Palindromic Subsequence
```python
def longest_palindrome_subseq(s):
    n = len(s)
    dp = [[0] * n for _ in range(n)]
    
    # Single character is palindrome of length 1
    for i in range(n):
        dp[i][i] = 1
    
    # Fill table
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j]:
                dp[i][j] = 2 + dp[i + 1][j - 1]
            else:
                dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
    
    return dp[0][n - 1]
```

---

### 4. String Transformation

#### Edit Distance (Levenshtein Distance)
```python
def edit_distance(s1, s2):
    m, n = len(s1), len(s2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Base cases
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i - 1] == s2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(
                    dp[i - 1][j],      # Delete
                    dp[i][j - 1],      # Insert
                    dp[i - 1][j - 1]   # Replace
                )
    
    return dp[m][n]
```

---

#### String Compression
```python
def compress(chars):
    write = 0
    read = 0
    
    while read < len(chars):
        char = chars[read]
        count = 0
        
        # Count consecutive characters
        while read < len(chars) and chars[read] == char:
            read += 1
            count += 1
        
        # Write character
        chars[write] = char
        write += 1
        
        # Write count if > 1
        if count > 1:
            for digit in str(count):
                chars[write] = digit
                write += 1
    
    return write
```

---

### 5. Advanced String Problems

#### Word Break
```python
def word_break(s, word_dict):
    word_set = set(word_dict)
    dp = [False] * (len(s) + 1)
    dp[0] = True
    
    for i in range(1, len(s) + 1):
        for j in range(i):
            if dp[j] and s[j:i] in word_set:
                dp[i] = True
                break
    
    return dp[len(s)]
```

---

#### Decode Ways
```python
def num_decodings(s):
    if not s or s[0] == '0':
        return 0
    
    n = len(s)
    dp = [0] * (n + 1)
    dp[0] = dp[1] = 1
    
    for i in range(2, n + 1):
        # Single digit
        if s[i - 1] != '0':
            dp[i] = dp[i - 1]
        
        # Two digits
        two_digit = int(s[i - 2:i])
        if 10 <= two_digit <= 26:
            dp[i] += dp[i - 2]
    
    return dp[n]
```

---

## Algorithm Comparison

| Algorithm | Time Complexity | Space Complexity | Best For |
|-----------|----------------|------------------|----------|
| Naive | O(m × n) | O(1) | Simple cases |
| KMP | O(m + n) | O(m) | Pattern matching |
| Rabin-Karp | O(n + m) avg | O(1) | Multiple patterns |
| Z-Algorithm | O(n + m) | O(n + m) | Pattern matching |

## Common Problems
- Pattern Matching (KMP, Rabin-Karp)
- Longest Common Substring
- Longest Common Subsequence
- Edit Distance
- Palindrome Problems
- Anagram Problems
- Word Break
- Decode Ways
- String Compression
- Valid Parentheses
- Group Anagrams
- Reverse Words
- Longest Common Prefix

