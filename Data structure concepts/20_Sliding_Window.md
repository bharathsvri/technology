# Sliding Window Technique (Java Implementation)

## Definition
Sliding Window is a technique used to efficiently solve problems involving contiguous subarrays or substrings by maintaining a window that slides through the data structure.

## Characteristics
- **Efficient**: Reduces O(n²) or O(n³) to O(n)
- **Contiguous Elements**: Works with consecutive elements
- **Window Maintenance**: Expands or contracts based on conditions
- **Single Pass**: Usually requires only one pass through data

## Types of Sliding Window

### 1. Fixed Size Window
- Window size is constant
- Window slides one position at a time
- Example: Maximum sum of subarray of size k

### 2. Variable Size Window
- Window size changes based on conditions
- Expand when condition not met, contract when condition met
- Example: Longest substring with k distinct characters

## Classic Problems

### 1. Maximum Sum of Subarray of Size K
**Problem**: Find maximum sum of contiguous subarray of size k

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public class SlidingWindow {
    public static int maxSumSubarrayK(int[] arr, int k) {
        if (arr.length < k) {
            return 0;
        }
        
        // Calculate sum of first window
        int windowSum = 0;
        for (int i = 0; i < k; i++) {
            windowSum += arr[i];
        }
        int maxSum = windowSum;
        
        // Slide the window
        for (int i = k; i < arr.length; i++) {
            windowSum = windowSum - arr[i - k] + arr[i];
            maxSum = Math.max(maxSum, windowSum);
        }
        
        return maxSum;
    }
}
```

---

### 2. First Negative in Every Window of Size K
**Problem**: Find first negative number in every window of size k

**Time Complexity**: O(n)
**Space Complexity**: O(k)

**Java Implementation**:
```java
import java.util.*;

public static List<Integer> firstNegativeInWindow(int[] arr, int k) {
    List<Integer> result = new ArrayList<>();
    Queue<Integer> negIndices = new LinkedList<>();
    
    for (int i = 0; i < arr.length; i++) {
        // Remove indices out of current window
        while (!negIndices.isEmpty() && negIndices.peek() < i - k + 1) {
            negIndices.poll();
        }
        
        // Add current index if negative
        if (arr[i] < 0) {
            negIndices.offer(i);
        }
        
        // Window of size k reached
        if (i >= k - 1) {
            if (!negIndices.isEmpty()) {
                result.add(arr[negIndices.peek()]);
            } else {
                result.add(0);
            }
        }
    }
    
    return result;
}
```

---

### 3. Longest Substring Without Repeating Characters
**Problem**: Find length of longest substring without repeating characters

**Time Complexity**: O(n)
**Space Complexity**: O(min(n, m)) where m is charset size

**Java Implementation**:
```java
import java.util.*;

public static int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> charMap = new HashMap<>();
    int left = 0;
    int maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        // If character seen before, move left pointer
        if (charMap.containsKey(c) && charMap.get(c) >= left) {
            left = charMap.get(c) + 1;
        }
        
        charMap.put(c, right);
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}

// Alternative using set
public static int lengthOfLongestSubstringSet(String s) {
    Set<Character> charSet = new HashSet<>();
    int left = 0;
    int maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        // Shrink window until no duplicate
        while (charSet.contains(s.charAt(right))) {
            charSet.remove(s.charAt(left));
            left++;
        }
        
        charSet.add(s.charAt(right));
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

---

### 4. Longest Substring with K Distinct Characters
**Problem**: Find longest substring with exactly k distinct characters

**Time Complexity**: O(n)
**Space Complexity**: O(k)

**Java Implementation**:
```java
import java.util.*;

public static int longestSubstringKDistinct(String s, int k) {
    if (k == 0) {
        return 0;
    }
    
    Map<Character, Integer> charCount = new HashMap<>();
    int left = 0;
    int maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        // Expand window
        charCount.put(c, charCount.getOrDefault(c, 0) + 1);
        
        // Shrink window if more than k distinct
        while (charCount.size() > k) {
            char leftChar = s.charAt(left);
            charCount.put(leftChar, charCount.get(leftChar) - 1);
            if (charCount.get(leftChar) == 0) {
                charCount.remove(leftChar);
            }
            left++;
        }
        
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

---

### 5. Minimum Window Substring
**Problem**: Find minimum window in string containing all characters of pattern

**Time Complexity**: O(|s| + |t|)
**Space Complexity**: O(|s| + |t|)

**Java Implementation**:
```java
import java.util.*;

public static String minWindow(String s, String t) {
    if (s == null || t == null || s.length() < t.length()) {
        return "";
    }
    
    // Count characters in pattern
    Map<Character, Integer> patternCount = new HashMap<>();
    for (char c : t.toCharArray()) {
        patternCount.put(c, patternCount.getOrDefault(c, 0) + 1);
    }
    
    int required = patternCount.size();
    int formed = 0;
    
    Map<Character, Integer> windowCount = new HashMap<>();
    int left = 0;
    int minLength = Integer.MAX_VALUE;
    int minLeft = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        // Expand window
        windowCount.put(c, windowCount.getOrDefault(c, 0) + 1);
        
        // Check if current character completes requirement
        if (patternCount.containsKey(c) && 
            windowCount.get(c).intValue() == patternCount.get(c).intValue()) {
            formed++;
        }
        
        // Try to contract window
        while (left <= right && formed == required) {
            // Update minimum window
            if (right - left + 1 < minLength) {
                minLength = right - left + 1;
                minLeft = left;
            }
            
            // Remove leftmost character
            char leftChar = s.charAt(left);
            windowCount.put(leftChar, windowCount.get(leftChar) - 1);
            if (patternCount.containsKey(leftChar) && 
                windowCount.get(leftChar) < patternCount.get(leftChar)) {
                formed--;
            }
            
            left++;
        }
    }
    
    return minLength == Integer.MAX_VALUE ? "" : 
           s.substring(minLeft, minLeft + minLength);
}
```

---

### 6. Longest Repeating Character Replacement
**Problem**: Find longest substring with same character after k replacements

**Time Complexity**: O(n)
**Space Complexity**: O(1) - limited to 26 characters

**Java Implementation**:
```java
import java.util.*;

public static int characterReplacement(String s, int k) {
    Map<Character, Integer> charCount = new HashMap<>();
    int left = 0;
    int maxLength = 0;
    int maxFreq = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        // Expand window
        charCount.put(c, charCount.getOrDefault(c, 0) + 1);
        maxFreq = Math.max(maxFreq, charCount.get(c));
        
        // Shrink if need more than k replacements
        while ((right - left + 1) - maxFreq > k) {
            char leftChar = s.charAt(left);
            charCount.put(leftChar, charCount.get(leftChar) - 1);
            left++;
        }
        
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

---

### 7. Maximum Average Subarray of Size K
**Problem**: Find maximum average of contiguous subarray of size k

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public static double findMaxAverage(int[] nums, int k) {
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += nums[i];
    }
    int maxSum = windowSum;
    
    for (int i = k; i < nums.length; i++) {
        windowSum = windowSum - nums[i - k] + nums[i];
        maxSum = Math.max(maxSum, windowSum);
    }
    
    return (double) maxSum / k;
}
```

---

### 8. Fruits into Baskets
**Problem**: Pick maximum fruits where each basket can hold one type of fruit

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
import java.util.*;

public static int totalFruit(int[] fruits) {
    Map<Integer, Integer> basket = new HashMap<>();
    int left = 0;
    int maxFruits = 0;
    
    for (int right = 0; right < fruits.length; right++) {
        // Add fruit to basket
        basket.put(fruits[right], basket.getOrDefault(fruits[right], 0) + 1);
        
        // Shrink if more than 2 types
        while (basket.size() > 2) {
            basket.put(fruits[left], basket.get(fruits[left]) - 1);
            if (basket.get(fruits[left]) == 0) {
                basket.remove(fruits[left]);
            }
            left++;
        }
        
        maxFruits = Math.max(maxFruits, right - left + 1);
    }
    
    return maxFruits;
}
```

---

### 9. Permutation in String
**Problem**: Check if one string contains permutation of another

**Time Complexity**: O(n)
**Space Complexity**: O(1) - limited to 26 characters

**Java Implementation**:
```java
import java.util.*;

public static boolean checkInclusion(String s1, String s2) {
    if (s1.length() > s2.length()) {
        return false;
    }
    
    Map<Character, Integer> s1Count = new HashMap<>();
    for (char c : s1.toCharArray()) {
        s1Count.put(c, s1Count.getOrDefault(c, 0) + 1);
    }
    
    Map<Character, Integer> windowCount = new HashMap<>();
    int left = 0;
    
    for (int right = 0; right < s2.length(); right++) {
        // Expand window
        char c = s2.charAt(right);
        windowCount.put(c, windowCount.getOrDefault(c, 0) + 1);
        
        // Maintain window of size len(s1)
        if (right - left + 1 > s1.length()) {
            char leftChar = s2.charAt(left);
            windowCount.put(leftChar, windowCount.get(leftChar) - 1);
            if (windowCount.get(leftChar) == 0) {
                windowCount.remove(leftChar);
            }
            left++;
        }
        
        // Check if window matches s1
        if (windowCount.equals(s1Count)) {
            return true;
        }
    }
    
    return false;
}
```

---

### 10. Count Anagrams
**Problem**: Count occurrences of anagram of pattern in string

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
import java.util.*;

public static int countAnagrams(String s, String pattern) {
    Map<Character, Integer> patternCount = new HashMap<>();
    for (char c : pattern.toCharArray()) {
        patternCount.put(c, patternCount.getOrDefault(c, 0) + 1);
    }
    
    Map<Character, Integer> windowCount = new HashMap<>();
    int left = 0;
    int count = 0;
    
    for (int right = 0; right < s.length(); right++) {
        // Expand window
        char c = s.charAt(right);
        windowCount.put(c, windowCount.getOrDefault(c, 0) + 1);
        
        // Maintain window size
        if (right - left + 1 > pattern.length()) {
            char leftChar = s.charAt(left);
            windowCount.put(leftChar, windowCount.get(leftChar) - 1);
            if (windowCount.get(leftChar) == 0) {
                windowCount.remove(leftChar);
            }
            left++;
        }
        
        // Check anagram
        if (right - left + 1 == pattern.length() && 
            windowCount.equals(patternCount)) {
            count++;
        }
    }
    
    return count;
}
```

---

### 11. Maximum of All Subarrays of Size K
**Problem**: Find maximum in every contiguous subarray of size k

**Time Complexity**: O(n)
**Space Complexity**: O(k)

**Java Implementation**:
```java
import java.util.*;

public static int[] maxSlidingWindow(int[] nums, int k) {
    if (nums.length == 0 || k == 0) {
        return new int[0];
    }
    
    Deque<Integer> dq = new ArrayDeque<>();  // Store indices
    List<Integer> result = new ArrayList<>();
    
    for (int i = 0; i < nums.length; i++) {
        // Remove indices out of current window
        while (!dq.isEmpty() && dq.peekFirst() < i - k + 1) {
            dq.pollFirst();
        }
        
        // Remove indices whose values are smaller than current
        while (!dq.isEmpty() && nums[dq.peekLast()] < nums[i]) {
            dq.pollLast();
        }
        
        dq.offerLast(i);
        
        // Window of size k reached
        if (i >= k - 1) {
            result.add(nums[dq.peekFirst()]);
        }
    }
    
    return result.stream().mapToInt(i -> i).toArray();
}
```

---

## Common Patterns

### Pattern 1: Fixed Window
```java
// Template for fixed window
public static int fixedWindow(int[] arr, int k) {
    // Calculate first window
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    int result = windowSum;
    
    // Slide window
    for (int i = k; i < arr.length; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        result = Math.max(result, windowSum);
    }
    
    return result;
}
```

### Pattern 2: Variable Window (Expand-Contract)
```java
import java.util.*;

// Template for variable window
public static int variableWindow(String s) {
    int left = 0;
    Map<Character, Integer> charCount = new HashMap<>();
    int maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        // Expand window
        char c = s.charAt(right);
        charCount.put(c, charCount.getOrDefault(c, 0) + 1);
        
        // Contract window until condition met
        while (conditionNotMet(charCount)) {
            char leftChar = s.charAt(left);
            charCount.put(leftChar, charCount.get(leftChar) - 1);
            if (charCount.get(leftChar) == 0) {
                charCount.remove(leftChar);
            }
            left++;
        }
        
        // Update result
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}

private static boolean conditionNotMet(Map<Character, Integer> charCount) {
    // Define your condition here
    return charCount.size() > 2;  // Example condition
}
```

## When to Use Sliding Window

1. **Contiguous Subarrays/Substrings**: Problems involving consecutive elements
2. **Fixed Size**: Window of constant size
3. **Variable Size**: Window that expands/contracts
4. **Optimization Problems**: Maximum/minimum/longest/shortest
5. **Frequency Counting**: Problems involving character/element counts

## Advantages
- Efficient: O(n) time complexity
- Single pass: Only one iteration needed
- Space efficient: Often O(1) or O(k) extra space
- Intuitive: Easy to understand and implement

## Common Problems
- Maximum Sum Subarray of Size K
- Longest Substring Without Repeating Characters
- Longest Substring with K Distinct Characters
- Minimum Window Substring
- Longest Repeating Character Replacement
- Maximum Average Subarray
- Fruits into Baskets
- Permutation in String
- Count Anagrams
- Maximum Sliding Window
- First Negative in Window
