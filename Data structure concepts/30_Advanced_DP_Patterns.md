# Advanced Dynamic Programming Patterns

## 1. DP on Trees

### Maximum Path Sum in Binary Tree
```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int val) { this.val = val; }
}

public class TreeDP {
    private int maxSum;
    
    public int maxPathSum(TreeNode root) {
        maxSum = Integer.MIN_VALUE;
        maxGain(root);
        return maxSum;
    }
    
    private int maxGain(TreeNode node) {
        if (node == null) return 0;
        
        // Max gain from left and right subtrees
        int leftGain = Math.max(maxGain(node.left), 0);
        int rightGain = Math.max(maxGain(node.right), 0);
        
        // Current path sum
        int currentPathSum = node.val + leftGain + rightGain;
        
        // Update maximum
        maxSum = Math.max(maxSum, currentPathSum);
        
        // Return max gain for parent
        return node.val + Math.max(leftGain, rightGain);
    }
}
```

### House Robber III
```java
public int rob(TreeNode root) {
    int[] result = robHelper(root);
    return Math.max(result[0], result[1]);
}

// Returns [rob this node, don't rob this node]
private int[] robHelper(TreeNode node) {
    if (node == null) return new int[]{0, 0};
    
    int[] left = robHelper(node.left);
    int[] right = robHelper(node.right);
    
    // Rob current node
    int rob = node.val + left[1] + right[1];
    
    // Don't rob current node
    int notRob = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
    
    return new int[]{rob, notRob};
}
```

## 2. Bitmask DP

### Traveling Salesman Problem
```java
public class TSP {
    public int tsp(int[][] graph) {
        int n = graph.length;
        int allVisited = (1 << n) - 1;
        Integer[][] memo = new Integer[n][1 << n];
        
        return dfs(0, 1, graph, memo, allVisited);
    }
    
    private int dfs(int pos, int mask, int[][] graph, Integer[][] memo, int allVisited) {
        if (mask == allVisited) {
            return graph[pos][0];  // Return to start
        }
        
        if (memo[pos][mask] != null) {
            return memo[pos][mask];
        }
        
        int minCost = Integer.MAX_VALUE;
        for (int next = 0; next < graph.length; next++) {
            if ((mask & (1 << next)) == 0) {  // Not visited
                int cost = graph[pos][next] + 
                          dfs(next, mask | (1 << next), graph, memo, allVisited);
                minCost = Math.min(minCost, cost);
            }
        }
        
        return memo[pos][mask] = minCost;
    }
}
```

### Assignment Problem
```java
public int assignment(int[][] cost) {
    int n = cost.length;
    int[] dp = new int[1 << n];
    Arrays.fill(dp, Integer.MAX_VALUE);
    dp[0] = 0;
    
    for (int mask = 0; mask < (1 << n); mask++) {
        int x = Integer.bitCount(mask);  // Number of set bits
        for (int j = 0; j < n; j++) {
            if ((mask & (1 << j)) == 0) {  // Job j not assigned
                dp[mask | (1 << j)] = Math.min(
                    dp[mask | (1 << j)],
                    dp[mask] + cost[x][j]
                );
            }
        }
    }
    
    return dp[(1 << n) - 1];
}
```

## 3. Digit DP

### Count Numbers with Unique Digits
```java
public class DigitDP {
    private Integer[][][] memo;
    private char[] digits;
    
    public int countNumbersWithUniqueDigits(int n) {
        String num = String.valueOf((long)Math.pow(10, n) - 1);
        digits = num.toCharArray();
        memo = new Integer[digits.length][2][1 << 10];
        return dfs(0, true, 0);
    }
    
    private int dfs(int pos, boolean isLimit, int mask) {
        if (pos == digits.length) {
            return 1;
        }
        
        if (memo[pos][isLimit ? 1 : 0][mask] != null) {
            return memo[pos][isLimit ? 1 : 0][mask];
        }
        
        int limit = isLimit ? digits[pos] - '0' : 9;
        int count = 0;
        
        for (int d = 0; d <= limit; d++) {
            if ((mask & (1 << d)) != 0) continue;  // Digit already used
            
            boolean nextIsLimit = isLimit && (d == limit);
            int nextMask = (mask == 0 && d == 0) ? 0 : (mask | (1 << d));
            count += dfs(pos + 1, nextIsLimit, nextMask);
        }
        
        return memo[pos][isLimit ? 1 : 0][mask] = count;
    }
}
```

## 4. DP on Subsets

### Partition Equal Subset Sum
```java
public boolean canPartition(int[] nums) {
    int sum = Arrays.stream(nums).sum();
    if (sum % 2 != 0) return false;
    
    int target = sum / 2;
    boolean[] dp = new boolean[target + 1];
    dp[0] = true;
    
    for (int num : nums) {
        for (int j = target; j >= num; j--) {
            dp[j] = dp[j] || dp[j - num];
        }
    }
    
    return dp[target];
}
```

## 5. Interval DP

### Matrix Chain Multiplication
```java
public int matrixChainMultiplication(int[] dims) {
    int n = dims.length - 1;
    int[][] dp = new int[n][n];
    
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            dp[i][j] = Integer.MAX_VALUE;
            
            for (int k = i; k < j; k++) {
                int cost = dp[i][k] + dp[k + 1][j] + 
                          dims[i] * dims[k + 1] * dims[j + 1];
                dp[i][j] = Math.min(dp[i][j], cost);
            }
        }
    }
    
    return dp[0][n - 1];
}
```

### Palindrome Partitioning
```java
public int minCut(String s) {
    int n = s.length();
    boolean[][] isPalindrome = new boolean[n][n];
    
    // Precompute palindromes
    for (int i = 0; i < n; i++) {
        isPalindrome[i][i] = true;
    }
    
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            if (s.charAt(i) == s.charAt(j)) {
                isPalindrome[i][j] = (len == 2) || isPalindrome[i + 1][j - 1];
            }
        }
    }
    
    int[] dp = new int[n];
    for (int i = 0; i < n; i++) {
        if (isPalindrome[0][i]) {
            dp[i] = 0;
        } else {
            dp[i] = i;
            for (int j = 0; j < i; j++) {
                if (isPalindrome[j + 1][i]) {
                    dp[i] = Math.min(dp[i], dp[j] + 1);
                }
            }
        }
    }
    
    return dp[n - 1];
}
```

## Common Patterns Summary

1. **DP on Trees**: Process subtrees, combine results
2. **Bitmask DP**: Represent states as bits
3. **Digit DP**: Count numbers with properties
4. **Interval DP**: Process intervals of increasing length
5. **DP on Subsets**: Process all subsets

## Common Problems
- Maximum Path Sum in Binary Tree
- House Robber III
- Traveling Salesman Problem
- Count Numbers with Unique Digits
- Partition Equal Subset Sum
- Matrix Chain Multiplication
- Palindrome Partitioning
- Burst Balloons
- Remove Boxes

