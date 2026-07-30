# Dynamic Programming (DP)

## Definition

Dynamic Programming (DP) is an algorithmic technique used to solve problems by breaking them down into smaller overlapping subproblems and efficiently solving each subproblem only once by storing the solutions.

---

# Core Concepts of Dynamic Programming

## Optimal Substructure

The problem's optimal solution can be constructed from the optimal solutions of its subproblems.

## Overlapping Subproblems

The same smaller subproblems are solved multiple times during recursion or problem solving.

---

# How DP Works

- Divide the problem into smaller subproblems.
- Solve each subproblem once and store the result (using a data structure like an array or table).
- Use the stored results to build up solutions to bigger subproblems, ultimately reaching the final solution.

---

# Approaches to DP

## Top-Down (Memoization)

Solve the problem recursively while storing results of subproblems to avoid recomputation.

## Bottom-Up (Tabulation)

Solve subproblems iteratively starting from the smallest, building up to the full problem.

---

# Difference between Tabulation and Memoization

**Tabulation:** Bottom-up approach, iterative, pre-fills solutions from smallest to largest subproblems.

**Memoization:** Top-down approach, recursive, stores results of recursive calls as they occur.

---

# Memoization

• **Approach:** Top-down (recursive).

• **How it works:** Uses recursion to solve the problem by breaking it into subproblems. Results of subproblems are stored (cached) in a data structure like a hash table or array.

• **Execution:** Solves subproblems on demand, only when needed.

### Advantages

- Easier to implement if the problem has a natural recursive structure.
- Efficient if not all subproblems need to be solved (sparse subproblem space).

### Disadvantages

- Recursive overhead can slow down the solution.
- Risk of stack overflow with deep recursion.

### Space Usage

Requires extra space for recursion call stack and storage of results.

---

# Tabulation

• **Approach:** Bottom-up (iterative).

• **How it works:** Solves all subproblems iteratively starting from the smallest, building up solutions in a table or array until the full problem is solved.

• **Execution:** All subproblems are solved regardless of need, in a specific order.

### Advantages

- Faster in practice due to no recursion overhead.
- No risk of stack overflow.
- Often more space efficient in terms of call stack.

### Disadvantages

- Code can be more complex and less intuitive than memoization.
- Solves all subproblems even if some are unnecessary.

### Space Usage

Only requires space for the DP table.

---

# Summary Comparison

| Aspect | Memoization | Tabulation |
|---------|-------------|------------|
| Approach | Top-down (recursive) | Bottom-up (iterative) |
| Execution Order | Solves on demand | Solves all subproblems systematically |
| Implementation | Easier with recursion | Iterative, sometimes more complex |
| Speed | Slower due to recursive overhead | Faster due to iteration |
| Space (Call Stack) | Uses extra space for recursion | No recursion stack space needed |
| Applicability | Useful for sparse subproblems | Useful when all subproblems are needed |
| Risk | Stack overflow possible | No stack overflow risk |

---

# When to Use Dynamic Programming

A problem is generally suitable for DP if it satisfies both:

- **Optimal Substructure**
- **Overlapping Subproblems**

---

# General DP Workflow

1. Identify the **state**.
2. Identify the **choices/decisions**.
3. Form the **recurrence relation (transition)**.
4. Define the **base cases**.
5. Implement using:
   - Memoization (Top-Down), or
   - Tabulation (Bottom-Up).
6. Optimize space if possible.

---

# Key Takeaways

- DP is an optimization technique, not a separate algorithm.
- It avoids repeated computations by storing previously computed results.
- DP can be implemented using Memoization or Tabulation.
- Memoization is recursive (Top-Down).
- Tabulation is iterative (Bottom-Up).
- Many exponential recursive solutions can be reduced to polynomial time using DP.
