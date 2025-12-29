# Manacher's Algorithm

## Definition
Manacher's algorithm efficiently finds the longest palindromic substring in a string in O(n) time complexity.

## Key Idea
- Uses previously computed palindrome information
- Avoids unnecessary recomputations
- Expands around centers efficiently

## Algorithm
1. Transform string to handle even-length palindromes
2. Maintain center and right boundary of current palindrome
3. Use symmetry to avoid recomputations
4. Expand when needed

## Java Implementation

```java
public class ManacherAlgorithm {
    
    // Transform string: "abc" -> "^#a#b#c#$"
    private String transform(String s) {
        if (s.isEmpty()) return "^$";
        StringBuilder sb = new StringBuilder("^");
        for (char c : s.toCharArray()) {
            sb.append("#").append(c);
        }
        sb.append("#$");
        return sb.toString();
    }
    
    public String longestPalindrome(String s) {
        if (s == null || s.length() == 0) return "";
        
        String T = transform(s);
        int n = T.length();
        int[] P = new int[n];  // Palindrome lengths
        int center = 0, right = 0;
        int maxLen = 0, centerIndex = 0;
        
        for (int i = 1; i < n - 1; i++) {
            int mirror = 2 * center - i;  // Mirror of i around center
            
            // Use previously computed information
            if (i < right) {
                P[i] = Math.min(right - i, P[mirror]);
            }
            
            // Attempt to expand palindrome centered at i
            while (T.charAt(i + (1 + P[i])) == T.charAt(i - (1 + P[i]))) {
                P[i]++;
            }
            
            // Update center and right boundary if palindrome expands past right
            if (i + P[i] > right) {
                center = i;
                right = i + P[i];
            }
            
            // Update maximum length
            if (P[i] > maxLen) {
                maxLen = P[i];
                centerIndex = i;
            }
        }
        
        // Extract longest palindrome
        int start = (centerIndex - maxLen) / 2;
        return s.substring(start, start + maxLen);
    }
    
    // Count all palindromic substrings
    public int countPalindromicSubstrings(String s) {
        if (s == null || s.length() == 0) return 0;
        
        String T = transform(s);
        int n = T.length();
        int[] P = new int[n];
        int center = 0, right = 0;
        int count = 0;
        
        for (int i = 1; i < n - 1; i++) {
            int mirror = 2 * center - i;
            
            if (i < right) {
                P[i] = Math.min(right - i, P[mirror]);
            }
            
            while (T.charAt(i + (1 + P[i])) == T.charAt(i - (1 + P[i]))) {
                P[i]++;
            }
            
            if (i + P[i] > right) {
                center = i;
                right = i + P[i];
            }
            
            count += (P[i] + 1) / 2;  // Count palindromes centered at i
        }
        
        return count;
    }
    
    // Get all palindromic substrings
    public List<String> getAllPalindromes(String s) {
        List<String> result = new ArrayList<>();
        if (s == null || s.length() == 0) return result;
        
        String T = transform(s);
        int n = T.length();
        int[] P = new int[n];
        int center = 0, right = 0;
        
        for (int i = 1; i < n - 1; i++) {
            int mirror = 2 * center - i;
            
            if (i < right) {
                P[i] = Math.min(right - i, P[mirror]);
            }
            
            while (T.charAt(i + (1 + P[i])) == T.charAt(i - (1 + P[i]))) {
                P[i]++;
            }
            
            if (i + P[i] > right) {
                center = i;
                right = i + P[i];
            }
            
            // Extract all palindromes centered at i
            for (int len = 1; len <= P[i]; len++) {
                int start = (i - len) / 2;
                result.add(s.substring(start, start + len));
            }
        }
        
        return result;
    }
}
```

## Time Complexity
- **Time**: O(n) - each character is processed at most twice
- **Space**: O(n) - for transformed string and P array

## Advantages
- Linear time complexity
- Single pass algorithm
- Efficient memory usage

## Common Problems
- Longest Palindromic Substring
- Count Palindromic Substrings
- Shortest Palindrome
- Palindrome Partitioning II
- Valid Palindrome III

