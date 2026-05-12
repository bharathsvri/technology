# Data Structures and Algorithms - Complete Guide

Welcome to the comprehensive guide on Data Structures and Algorithms! This collection covers fundamental concepts, implementations, and problem-solving techniques.

## 📚 Table of Contents

### Data Structures

1. **[Arrays](01_Arrays.md)** - Contiguous memory, random access, dynamic arrays
2. **[Linked Lists](02_Linked_Lists.md)** - Singly, doubly, circular linked lists
3. **[Stacks](03_Stacks.md)** - LIFO structure, implementation, applications
4. **[Queues](04_Queues.md)** - FIFO structure, types, priority queues
5. **[Trees](05_Trees.md)** - Binary trees, tree properties, traversals
6. **[Binary Search Trees](06_Binary_Search_Trees.md)** - BST operations, balancing
7. **[Graphs](07_Graphs.md)** - Graph representation, traversal, algorithms
8. **[Hash Tables](08_Hash_Tables.md)** - Hashing, collision resolution
9. **[Heaps](09_Heaps.md)** - Min/max heaps, priority queues
10. **[Tries](10_Tries.md)** - Prefix trees, string operations
11. **[Sets](11_Sets.md)** - Hash sets, tree sets, operations
12. **[Maps/Dictionaries](12_Maps_Dictionaries.md)** - Key-value pairs, implementations

### Algorithms

13. **[Sorting Algorithms](13_Sorting_Algorithms.md)** - Bubble, Merge, Quick, Heap, Radix, etc.
14. **[Searching Algorithms](14_Searching_Algorithms.md)** - Linear, Binary, Interpolation, etc.
15. **[Dynamic Programming](15_Dynamic_Programming.md)** - Memoization, tabulation, classic problems
16. **[Greedy Algorithms](16_Greedy_Algorithms.md)** - Activity selection, MST, shortest path
17. **[Backtracking](17_Backtracking.md)** - N-Queens, Sudoku, permutations, combinations
18. **[Divide and Conquer](18_Divide_and_Conquer.md)** - Merge sort, quick sort, power function
19. **[Two Pointers](19_Two_Pointers_Technique.md)** - Converging pointers, fast-slow pointers
20. **[Sliding Window](20_Sliding_Window.md)** - Fixed/variable windows, substring problems
21. **[String Algorithms](21_String_Algorithms.md)** - KMP, Rabin-Karp, pattern matching
22. **[Recursion](22_Recursion.md)** - Recursive thinking, base cases, optimization
23. **[Bit Manipulation](23_Bit_Manipulation.md)** - Bit operations, tricks, optimization

### Advanced Data Structures

24. **[Union-Find (DSU)](24_Union_Find_Disjoint_Set.md)** - Disjoint Set Union, path compression
25. **[AVL Tree](25_AVL_Tree.md)** - Self-balancing BST, rotations
26. **[Segment Tree](26_Segment_Tree.md)** - Range queries, lazy propagation
27. **[Fenwick Tree (BIT)](27_Fenwick_Tree_BIT.md)** - Binary Indexed Tree, efficient range queries

### Advanced Algorithms

28. **[Advanced Graph Algorithms](28_Advanced_Graph_Algorithms.md)** - SCC, Articulation Points, Bridges, Network Flow
29. **[Manacher's Algorithm](29_Manacher_Algorithm.md)** - Longest palindromic substring in O(n)
30. **[Advanced DP Patterns](30_Advanced_DP_Patterns.md)** - DP on Trees, Bitmask DP, Digit DP, Interval DP

### Complexity, Selection, Caching, and Text Indexing

31. **[Amortized Analysis and Master Theorem](31_Amortized_Analysis_and_Master_Theorem.md)** - Amortized bounds; divide-and-conquer recurrences
32. **[Order Statistics (Quickselect)](32_Order_Statistics_Quick_Select.md)** - k-th element in expected linear time
33. **[LRU Cache and System Design](33_LRU_Cache_System_Design.md)** - O(1) LRU with HashMap + doubly linked list; distributed cache notes
34. **[Suffix Array Introduction](34_Suffix_Array_Intro.md)** - Suffix ordering, LCP array, construction sketches, applications

35. **[Big-O, Big-Ω, and Big-Θ](35_Big_O_Omega_Theta_Complexity.md)** - Asymptotic upper, lower, and tight bounds; common growth classes

36. **[B-Trees and B+ Trees](36_B_Trees_and_B_Plus_Trees.md)** - High fanout search trees; why databases use B+ trees for indexes

37. **[Monotonic Stack](37_Monotonic_Stack.md)** - Next/previous greater/smaller element patterns in linear time

38. **[Sweep Line Algorithm](38_Sweep_Line_Algorithm.md)** - Event sorting for intervals, overlaps, and geometry sketches

39. **[Bloom Filter](39_Bloom_Filter.md)** - Probabilistic membership; space-efficient sets with false positives

40. **[Binary Search on Answer](40_Binary_Search_on_Answer.md)** - Monotone feasibility predicates; optimization via binary search

41. **[Eulerian Path and Circuit](41_Eulerian_Path_and_Circuit.md)** - Edge-disjoint trails; Hierholzer’s algorithm sketch

42. **[Matrix Exponentiation](42_Matrix_Exponentiation.md)** - Fast linear recurrences and walk counts via binary powering

43. **[Merging Intervals and Line Sweep](43_Merging_Intervals_and_Line_Sweep.md)** - Union, gaps, and overlap patterns

44. **[Bit Trie (XOR Maximum)](44_Bit_Trie_XOR_Maximum.md)** - Binary trie for max XOR pairs and variants

## 🎯 Quick Reference

### Time Complexity Cheat Sheet

| Data Structure | Access | Search | Insertion | Deletion |
|----------------|--------|--------|-----------|----------|
| Array | O(1) | O(n) | O(n) | O(n) |
| Linked List | O(n) | O(n) | O(1) | O(1) |
| Stack | O(n) | O(n) | O(1) | O(1) |
| Queue | O(n) | O(n) | O(1) | O(1) |
| BST | O(log n) | O(log n) | O(log n) | O(log n) |
| Hash Table | N/A | O(1) | O(1) | O(1) |
| Heap | O(1) | O(n) | O(log n) | O(log n) |

### Algorithm Complexity

| Algorithm | Best | Average | Worst | Space |
|-----------|------|---------|-------|-------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) |
| Binary Search | O(1) | O(log n) | O(log n) | O(1) |
| Linear Search | O(1) | O(n) | O(n) | O(1) |

## 📖 How to Use This Guide

1. **Start with Fundamentals**: Begin with Arrays and Linked Lists
2. **Learn Linear Structures**: Stacks and Queues
3. **Master Trees**: Binary Trees → BST → Advanced Trees
4. **Understand Graphs**: Representation → Traversal → Algorithms
5. **Practice Algorithms**: Sorting → Searching → DP → Greedy
6. **Apply Techniques**: Two Pointers, Sliding Window, Backtracking

## 🔑 Key Concepts by Category

### Problem-Solving Patterns

- **Two Pointers**: Opposite ends, fast-slow pointers
- **Sliding Window**: Fixed size, variable size windows
- **Dynamic Programming**: Memoization, tabulation
- **Greedy**: Local optimal choices
- **Backtracking**: Explore all possibilities with pruning
- **Divide and Conquer**: Break into smaller subproblems

### Common Problem Types

- **Array Problems**: Two sum, maximum subarray, sliding window
- **String Problems**: Pattern matching, palindrome, anagrams
- **Tree Problems**: Traversal, LCA, path sum, validation
- **Graph Problems**: Shortest path, MST, cycle detection
- **DP Problems**: Knapsack, LCS, edit distance, coin change
- **Greedy Problems**: Activity selection, interval scheduling

## 💡 Study Tips

1. **Understand First**: Don't memorize, understand the concept
2. **Code Implementation**: Implement each data structure yourself
3. **Practice Problems**: Solve problems after learning each concept
4. **Draw Diagrams**: Visualize data structures and algorithms
5. **Trace Execution**: Step through algorithms with examples
6. **Time Complexity**: Always analyze time and space complexity
7. **Compare Approaches**: Understand when to use which approach

## 📝 Notes

- **All code examples are in Java** (complete implementations)
- Time complexities are average case unless specified
- Space complexities exclude input/output space
- Practice problems listed at end of each section
- Includes both fundamental and advanced concepts for mastering DSA

## 🚀 Next Steps

1. Complete reading all data structure concepts
2. Study algorithm techniques thoroughly
3. Solve problems on platforms like LeetCode, HackerRank
4. Implement data structures from scratch
5. Build projects using these concepts

---

**Happy Learning! 🎓**

