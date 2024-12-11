### **Data Structures**

1. **Arrays and Lists**
    
    - **Why:** Most foundational for storing and manipulating data. Use cases include sequences, stacks (with lists), or dynamic arrays.
    - **Know:** Indexing, slicing, and iteration.
2. **Hash Tables (Dictionaries in Python, HashMap in Java, etc.)**
    
    - **Why:** Extremely versatile for fast lookups, key-value storage, and frequency counting.
    - **Examples:** Caching, implementing sets, fast lookups in logs.
3. **Sets**
    
    - **Why:** Efficient for ensuring uniqueness, performing unions, intersections, or differences.
    - **Examples:** Deduplication, membership checks.
4. **Stacks and Queues**
    
    - **Why:** Useful for scenarios like undo operations, parsing, or managing tasks in order.
    - **Examples:** Browser history (stack), task schedulers (queue).
5. **Trees (Basic Binary Trees, Binary Search Trees, Tries)**
    
    - **Why:** Essential for hierarchical data, searching, or prefix matching (e.g., autocomplete).
    - **Examples:** File systems, routing tables, autocomplete.
6. **Graphs**
    
    - **Why:** Fundamental for modeling networks like social media, transport systems, or relationships.
    - **Examples:** Shortest path problems, dependency resolution.
7. **Heaps (Priority Queues)**
    
    - **Why:** Efficient for scenarios requiring frequent access to min/max elements.
    - **Examples:** Task scheduling, top-K problems.
8. **Linked Lists** (Know, but rarely used in practice unless needed for low-level manipulation)
    
    - **Why:** Useful in cases of dynamic memory usage, custom data structure design.

---

### **Algorithms**

1. **Sorting and Searching**
    
    - **Sorting:** Quicksort, Merge Sort (concepts; use library functions for implementation).
    - **Searching:** Binary Search (important for log(n) efficiency in sorted data).
2. **Graph Algorithms**
    
    - **Breadth-First Search (BFS):** For shortest paths in unweighted graphs.
    - **Depth-First Search (DFS):** For exploring all possible paths or detecting cycles.
    - **Dijkstra's Algorithm:** For shortest path in weighted graphs (real-world routing).
    - **A*** (A-star):** For pathfinding with heuristics (used in games and mapping).
3. **Dynamic Programming (DP)**
    
    - **Why:** For problems that can be broken into overlapping subproblems.
    - **Examples:** Caching, memoization, solving knapsack-like problems.
4. **Greedy Algorithms**
    
    - **Why:** Great for problems where local optimization leads to global solutions.
    - **Examples:** Scheduling, Huffman coding.
5. **Divide and Conquer**
    
    - **Why:** Break problems into smaller parts (e.g., binary search, mergesort).
    - **Examples:** Image processing, large-scale simulations.
6. **String Algorithms**
    
    - **Basic Matching:** Naive search, KMP, or use built-in regex libraries.
    - **Substring/Anagram Problems:** Sliding window or hashing techniques.
7. **Hashing and Caching**
    
    - **Why:** Essential for quick lookups and preventing redundant computations.
    - **Examples:** Caching web pages, reducing database queries.
8. **Backtracking**
    
    - **Why:** For exploring all possibilities, especially in constraint-based problems.
    - **Examples:** Solving Sudoku, N-Queens problem.

---

### **Why These Matter**

- **Understand trade-offs**: In the real world, often you care more about readability, maintainability, and scalability than shaving microseconds off runtime.
- **Scalability and latency awareness**: Algorithms like BFS/DFS, hash maps, and priority queues are common in scalable systems.
- **Avoid overengineering**: Focus on simplicity and optimizing only bottlenecks, not the entire system.

Data structures are like building blocks and there is always the right material for the job.
You wouldn't make your house out of paper, would you?

In each programming language some data structures are implemented but have different names.
For example arrays are called lists in python.