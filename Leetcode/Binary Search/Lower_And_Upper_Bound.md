# 🔍 Lower Bound & Upper Bound

![Language](https://img.shields.io/badge/Language-Java-blueviolet?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Binary%20Search-blue?style=for-the-badge)
![Paradigm](https://img.shields.io/badge/Paradigm-Decrease%20%26%20Conquer-green?style=for-the-badge)

---

## 📖 Why Lower Bound and Upper Bound?

Normal Binary Search asks:

> **"Does the target exist?"**

But sometimes we need more specific information from a **sorted array**.

For example:

```text
arr = [1, 2, 2, 2, 4, 5]
```

For target `2`, normal Binary Search may return **any** `2`.

But we may want:

* Where does `2` **first appear**?
* Where does the block of `2`s **end**?
* Where should a value be **inserted**?

This is where **Lower Bound** and **Upper Bound** are useful.

---

# 🔽 Lower Bound

### Definition

The **Lower Bound** is the **smallest index** where:

```text
arr[index] >= target
```

If no such index exists, return:

```text
arr.length
```

### Why `arr.length` instead of `-1`?

Because Lower Bound is not simply asking:

> "Was the target found?"

It is asking:

> **"What is the first position where I can place this target without violating sorted order?"**

If every element is smaller than the target:

```text
arr = [1, 2, 3]
target = 5
```

There is no valid index inside the array.

But `5` would be inserted **after the last element**:

```text
[1, 2, 3, 5]
          ↑
        index 3
```

And:

```text
arr.length = 3
```

So returning `arr.length` naturally represents the **insertion position**.

---

## 💻 Lower Bound Code

```java
class LowerBoundFinder {

    // Function to find the lower bound index using binary search
    public int lowerBound(int[] arr, int target) {
        int low = 0; // Start index
        int high = arr.length - 1; // End index

        int ans = arr.length; 
        // Default value if no arr[index] >= target exists.
        // arr.length represents the insertion position after the last element.

        while (low <= high) {

            int mid = (low + high) / 2; // Find mid index

            if (arr[mid] >= target) {
                ans = mid;            // Store possible answer
                high = mid - 1;       // Move left to find an earlier answer
            } else {
                low = mid + 1;        // Move right
            }
        }

        return ans; // Return the lower bound index
    }
}
```

---

## ✨ Example

```text
arr = [1, 2, 2, 2, 4, 5]
target = 2
```

Lower Bound:

```text
index:  0  1  2  3  4  5
value:  1  2  2  2  4  5
           ↑
          LB
```

Answer:

```text
1
```

Because index `1` is the **smallest index** where:

```text
arr[index] >= 2
```

---

# 🔼 Upper Bound

### Definition

The **Upper Bound** is the **smallest index** where:

```text
arr[index] > target
```

Notice the important difference:

```text
Lower Bound → arr[index] >= target

Upper Bound → arr[index] > target
```

---

## 💻 Upper Bound Code

```java
class UpperBoundFinder {

    // Binary search to find upper bound
    public int upperBound(int[] arr, int target) {

        int low = 0;
        int high = arr.length - 1;

        int ans = arr.length;
        // Default to length if no element > target exists

        while (low <= high) {

            int mid = (low + high) / 2;

            if (arr[mid] > target) {
                ans = mid;        // Store current index as potential answer
                high = mid - 1;   // Move left to find an earlier answer
            } else {
                low = mid + 1;    // Move right
            }
        }

        return ans;  // Return final answer
    }
}
```

---

# 🔥 Lower Bound vs Upper Bound

For:

```text
arr = [1, 2, 2, 2, 4, 5]
target = 2
```

```text
index:  0  1  2  3  4  5
value:  1  2  2  2  4  5
           ↑        ↑
          LB        UB
```

```text
Lower Bound = 1
Upper Bound = 4
```

Because:

```text
Lower Bound → first index where arr[index] >= 2
            → index 1

Upper Bound → first index where arr[index] > 2
            → index 4
```

---

# 🎯 Easy Way to Remember

| Algorithm   | Condition          | Meaning                    |
| ----------- | ------------------ | -------------------------- |
| Lower Bound | `arr[i] >= target` | First element **≥ target** |
| Upper Bound | `arr[i] > target`  | First element **> target** |

The only difference is:

```text
Lower → >=

Upper → >
```

---

# 🔗 Connection With First Occurrence

If the target **exists** in the sorted array:

```text
Lower Bound = First Occurrence
```

Example:

```text
arr = [1, 2, 2, 2, 4]
target = 2

Lower Bound = 1
First Occurrence = 1
```

But Lower Bound is **more general**.

It still returns a meaningful index even when the target does not exist.

Example:

```text
arr = [1, 3, 5, 7]
target = 4
```

```text
Lower Bound = 2
```

because:

```text
arr[2] = 5
```

is the first value `>= 4`.

---

# 📈 Complexity

| Approach    | Time Complexity | Space Complexity |
| ----------- | --------------- | ---------------- |
| Lower Bound | O(log n)        | O(1)             |
| Upper Bound | O(log n)        | O(1)             |

---

# 🎯 Key Takeaways

* Both are variations of **Binary Search**.
* Both require a **sorted search space**.
* **Lower Bound** finds the first position where:

```text
arr[i] >= target
```

* **Upper Bound** finds the first position where:

```text
arr[i] > target
```

* If no valid position exists, both return:

```text
arr.length
```

* If the target exists:

```text
Lower Bound = First Occurrence
```

* The key idea is **not to immediately return when a valid element is found**.

Instead:

```text
Store the answer
↓
Continue searching left
↓
Find the FIRST possible position
```

That is the small but important modification that turns normal Binary Search into **Lower/Upper Bound Binary Search**.
