# Two Pointers Technique (Java Implementation)

## Definition
Two Pointers is a technique where two pointers traverse an array or linked list in a coordinated way to solve problems efficiently, often reducing time complexity from O(n²) to O(n).

## Characteristics
- **Efficient**: Usually O(n) time complexity
- **Space Efficient**: O(1) extra space typically
- **Synchronized Movement**: Two pointers move based on conditions
- **Pattern Recognition**: Often used with sorted arrays

## Types of Two Pointers

### 1. Opposite Ends (Converging)
- Start from both ends
- Move towards each other
- Used for: Two Sum, Palindrome, Container With Most Water

### 2. Same Direction (Fast and Slow)
- Both start from beginning
- Different speeds
- Used for: Remove Duplicates, Cycle Detection, Middle Element

### 3. Different Starting Points
- Start from different positions
- Move independently
- Used for: Merge Sorted Arrays, Intersection

## Classic Problems

### 1. Two Sum (Sorted Array)
**Problem**: Find two numbers that add up to target

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public class TwoPointers {
    public static int[] twoSumSorted(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        
        while (left < right) {
            int currentSum = arr[left] + arr[right];
            if (currentSum == target) {
                return new int[]{left, right};
            } else if (currentSum < target) {
                left++;
            } else {
                right--;
            }
        }
        
        return new int[0];  // Not found
    }
}
```

---

### 2. Three Sum
**Problem**: Find all triplets that sum to zero

**Time Complexity**: O(n²)
**Space Complexity**: O(1) excluding output

**Java Implementation**:
```java
import java.util.*;

public static List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> result = new ArrayList<>();
    
    for (int i = 0; i < nums.length - 2; i++) {
        // Skip duplicates
        if (i > 0 && nums[i] == nums[i - 1]) {
            continue;
        }
        
        int left = i + 1;
        int right = nums.length - 1;
        
        while (left < right) {
            int currentSum = nums[i] + nums[left] + nums[right];
            
            if (currentSum == 0) {
                result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                
                // Skip duplicates
                while (left < right && nums[left] == nums[left + 1]) {
                    left++;
                }
                while (left < right && nums[right] == nums[right - 1]) {
                    right--;
                }
                
                left++;
                right--;
            } else if (currentSum < 0) {
                left++;
            } else {
                right--;
            }
        }
    }
    
    return result;
}
```

---

### 3. Container With Most Water
**Problem**: Find two lines that form container with most water

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public static int maxArea(int[] height) {
    int left = 0;
    int right = height.length - 1;
    int maxWater = 0;
    
    while (left < right) {
        int width = right - left;
        int currentArea = Math.min(height[left], height[right]) * width;
        maxWater = Math.max(maxWater, currentArea);
        
        // Move pointer with smaller height
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    
    return maxWater;
}
```

---

### 4. Trapping Rain Water
**Problem**: Calculate trapped rainwater between bars

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public static int trapWater(int[] height) {
    if (height.length == 0) {
        return 0;
    }
    
    int left = 0;
    int right = height.length - 1;
    int leftMax = 0;
    int rightMax = 0;
    int water = 0;
    
    while (left < right) {
        if (height[left] < height[right]) {
            if (height[left] >= leftMax) {
                leftMax = height[left];
            } else {
                water += leftMax - height[left];
            }
            left++;
        } else {
            if (height[right] >= rightMax) {
                rightMax = height[right];
            } else {
                water += rightMax - height[right];
            }
            right--;
        }
    }
    
    return water;
}
```

---

### 5. Remove Duplicates from Sorted Array
**Problem**: Remove duplicates in-place, return new length

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public static int removeDuplicates(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    
    // Slow pointer (write position)
    int write = 1;
    
    // Fast pointer (read position)
    for (int read = 1; read < nums.length; read++) {
        if (nums[read] != nums[read - 1]) {
            nums[write] = nums[read];
            write++;
        }
    }
    
    return write;
}
```

---

### 6. Remove Element
**Problem**: Remove all instances of value in-place

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public static int removeElement(int[] nums, int val) {
    int write = 0;
    
    for (int read = 0; read < nums.length; read++) {
        if (nums[read] != val) {
            nums[write] = nums[read];
            write++;
        }
    }
    
    return write;
}
```

---

### 7. Move Zeros to End
**Problem**: Move all zeros to end while maintaining order

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public static void moveZeros(int[] nums) {
    int write = 0;
    
    // Move non-zeros to front
    for (int read = 0; read < nums.length; read++) {
        if (nums[read] != 0) {
            nums[write] = nums[read];
            write++;
        }
    }
    
    // Fill remaining with zeros
    for (int i = write; i < nums.length; i++) {
        nums[i] = 0;
    }
}

// Alternative: Swap approach
public static void moveZerosSwap(int[] nums) {
    int write = 0;
    
    for (int read = 0; read < nums.length; read++) {
        if (nums[read] != 0) {
            int temp = nums[write];
            nums[write] = nums[read];
            nums[read] = temp;
            write++;
        }
    }
}
```

---

### 8. Valid Palindrome
**Problem**: Check if string is palindrome (ignoring non-alphanumeric)

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public static boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        // Skip non-alphanumeric
        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
            left++;
        }
        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
            right--;
        }
        
        if (Character.toLowerCase(s.charAt(left)) != 
            Character.toLowerCase(s.charAt(right))) {
            return false;
        }
        
        left++;
        right--;
    }
    
    return true;
}
```

---

### 9. Reverse String
**Problem**: Reverse string in-place

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public static void reverseString(char[] s) {
    int left = 0;
    int right = s.length - 1;
    
    while (left < right) {
        char temp = s[left];
        s[left] = s[right];
        s[right] = temp;
        left++;
        right--;
    }
}
```

---

### 10. Merge Sorted Arrays
**Problem**: Merge two sorted arrays (one has enough space)

**Time Complexity**: O(m + n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public static void mergeSortedArrays(int[] nums1, int m, int[] nums2, int n) {
    // Start from end
    int i = m - 1;
    int j = n - 1;
    int k = m + n - 1;
    
    while (i >= 0 && j >= 0) {
        if (nums1[i] > nums2[j]) {
            nums1[k] = nums1[i];
            i--;
        } else {
            nums1[k] = nums2[j];
            j--;
        }
        k--;
    }
    
    // Copy remaining elements from nums2
    while (j >= 0) {
        nums1[k] = nums2[j];
        j--;
        k--;
    }
}
```

---

### 11. Intersection of Two Arrays
**Problem**: Find intersection of two sorted arrays

**Time Complexity**: O(m + n)
**Space Complexity**: O(1) excluding output

**Java Implementation**:
```java
import java.util.*;

public static List<Integer> intersection(int[] nums1, int[] nums2) {
    List<Integer> result = new ArrayList<>();
    int i = 0;
    int j = 0;
    
    while (i < nums1.length && j < nums2.length) {
        if (nums1[i] == nums2[j]) {
            // Avoid duplicates
            if (result.isEmpty() || result.get(result.size() - 1) != nums1[i]) {
                result.add(nums1[i]);
            }
            i++;
            j++;
        } else if (nums1[i] < nums2[j]) {
            i++;
        } else {
            j++;
        }
    }
    
    return result;
}
```

---

### 12. Linked List Cycle Detection
**Problem**: Detect if linked list has cycle

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}

public static boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) {
        return false;
    }
    
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            return true;
        }
    }
    
    return false;
}

// Find cycle start
public static ListNode detectCycleStart(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    
    // Find meeting point
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            break;
        }
    }
    
    if (fast == null || fast.next == null) {
        return null;  // No cycle
    }
    
    // Find cycle start
    slow = head;
    while (slow != fast) {
        slow = slow.next;
        fast = fast.next;
    }
    
    return slow;
}
```

---

### 13. Remove Nth Node From End
**Problem**: Remove nth node from end of linked list

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public static ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    
    ListNode first = dummy;
    ListNode second = dummy;
    
    // Move first pointer n+1 steps ahead
    for (int i = 0; i <= n; i++) {
        first = first.next;
    }
    
    // Move both until first reaches end
    while (first != null) {
        first = first.next;
        second = second.next;
    }
    
    // Remove node
    second.next = second.next.next;
    
    return dummy.next;
}
```

---

### 14. Middle of Linked List
**Problem**: Find middle node of linked list

**Time Complexity**: O(n)
**Space Complexity**: O(1)

**Java Implementation**:
```java
public static ListNode middleNode(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    return slow;
}
```

---

### 15. Squares of Sorted Array
**Problem**: Return squares of array sorted in non-decreasing order

**Time Complexity**: O(n)
**Space Complexity**: O(n)

**Java Implementation**:
```java
public static int[] sortedSquares(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    int left = 0;
    int right = n - 1;
    int index = n - 1;
    
    while (left <= right) {
        int leftSq = nums[left] * nums[left];
        int rightSq = nums[right] * nums[right];
        
        if (leftSq > rightSq) {
            result[index] = leftSq;
            left++;
        } else {
            result[index] = rightSq;
            right--;
        }
        index--;
    }
    
    return result;
}
```

---

## Patterns and Tips

### Pattern 1: Opposite Ends
- Use when searching pairs
- Works well with sorted arrays
- Move based on comparison

### Pattern 2: Fast and Slow
- Use for cycle detection
- Use for finding middle
- Different speeds reveal structure

### Pattern 3: Read-Write Pointer
- Use for in-place modifications
- Write pointer tracks position
- Read pointer scans array

## When to Use Two Pointers

1. **Sorted Arrays**: Often work with sorted data
2. **In-place Operations**: Need to modify array in-place
3. **O(n) Requirement**: Need linear time solution
4. **Pair/Triplet Problems**: Finding pairs or triplets
5. **Linked Lists**: Cycle detection, finding middle

## Advantages
- Efficient: Usually O(n) time
- Space efficient: O(1) extra space
- Simple implementation
- Clear logic

## Common Problems
- Two Sum (sorted)
- Three Sum
- Four Sum
- Container With Most Water
- Trapping Rain Water
- Remove Duplicates
- Remove Element
- Move Zeros
- Valid Palindrome
- Reverse String
- Merge Sorted Arrays
- Intersection of Arrays
- Cycle Detection
- Remove Nth Node
- Middle of Linked List
- Squares of Sorted Array
