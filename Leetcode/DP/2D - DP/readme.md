# 📘 2D Dynamic Programming

![Topic](https://img.shields.io/badge/Topic-2D%20Dynamic%20Programming-blue?style=for-the-badge)
![Language](https://img.shields.io/badge/Language-Java-blueviolet?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Medium-orange?style=for-the-badge)

---

## 📖 About

**2D Dynamic Programming** is a category of DP problems where the state is represented using **two variables**, most commonly a row and column in a matrix or grid.

The DP state is typically represented as:

```text
dp[i][j]
```

where each state depends on previously computed neighboring states.

Most 2D DP problems follow patterns like:

```text
dp[i][j] = function(
    dp[i-1][j],
    dp[i][j-1],
    dp[i-1][j-1],
    ...
)
```

These problems help in understanding how Dynamic Programming extends from one dimension to multiple dimensions while maintaining the same core principles.

---

## 🧠 Common Pattern

1. Define the DP state (`dp[i][j]`).
2. Identify the recurrence relation.
3. Initialize the base row and/or base column.
4. Fill the DP table in the correct traversal order.
5. Return the required cell (or compute the final answer from the DP table).

---

## 🎯 Key Takeaways

- Uses a **2D DP table** (`dp[i][j]`).
- Each state usually depends on adjacent cells (top, left, diagonal, etc.).
- Base cases are often the **first row**, **first column**, or the **starting cell**.
- The traversal order is important since every state depends on previously computed values.
- Forms the foundation for advanced topics such as Grid DP, String DP (LCS/Edit Distance), Interval DP, and Matrix DP.
