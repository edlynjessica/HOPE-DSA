# 📘 1D Dynamic Programming

![Topic](https://img.shields.io/badge/Topic-1D%20Dynamic%20Programming-blue?style=for-the-badge)
![Language](https://img.shields.io/badge/Language-Java-orange?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Easy%20→%20Medium-green?style=for-the-badge)

---

## 📖 About

**1D Dynamic Programming** is a category of DP problems where the state is represented using a **single variable or index**, and the solution is built using answers to previous states.

Most 1D DP problems follow the idea:

```text
dp[i] = function(dp[i-1], dp[i-2], ...)
```

They are excellent for learning the fundamentals of Dynamic Programming, including:

- Defining the DP state
- Finding the recurrence relation
- Memoization (Top-Down)
- Tabulation (Bottom-Up)
- Space Optimization

---

## 🧠 Common Pattern

1. Define the state.
2. Identify the recurrence relation.
3. Determine the base cases.
4. Implement using Memoization or Tabulation.
5. Optimize space whenever possible.

---

## 🎯 Key Takeaways

- Uses a **1D DP array** (`dp[i]`).
- Each state depends on one or more previous states.
- Many problems can be optimized from **O(n)** space to **O(1)** space.
- Forms the foundation for advanced DP topics like Grid DP, Knapsack DP, and String DP.
