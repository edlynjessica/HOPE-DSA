# 🔍 Floor and Ceil

![Language](https://img.shields.io/badge/Language-Java-blueviolet?style=for-the-badge)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Binary%20Search-blue?style=for-the-badge)

---

## 📖 What are Floor and Ceil?

For a given value `x` in a **sorted array**:

### 🔽 Floor

The **floor** is the **largest value that is less than or equal to `x`**.

```text
Floor = largest value <= x
```

### 🔼 Ceil

The **ceil** is the **smallest value that is greater than or equal to `x`**.

```text
Ceil = smallest value >= x
```

---

## 💡 Example

```text
arr = [3, 4, 4, 7, 8, 10]
x = 5
```

```text
3   4   4   5   7   8   10
        ↑       ↑
      Floor    Ceil
```

So:

```text
Floor = 4
Ceil = 7
```

---

# 🧠 Floor using Binary Search

We want:

```text
largest arr[mid] <= x
```

If:

```text
arr[mid] <= x
```

Then `arr[mid]` **could be the floor**.

Store it:

```java
ans = arr[mid];
```

But there might be a larger valid value on the right.

So:

```java
low = mid + 1;
```

If:

```text
arr[mid] > x
```

then `arr[mid]` is too large, so move left:

```java
high = mid - 1;
```

---

## 🔽 Floor Code

```java
class FloorCeilFinder {

    // Function to find floor
    public int findFloor(int[] arr, int x) {

        int low = 0;
        int high = arr.length - 1;
        int ans = -1;

        while (low <= high) {

            int mid = (low + high) / 2;

            if (arr[mid] <= x) {
                ans = arr[mid];     // Potential floor
                low = mid + 1;      // Search for a larger valid value
            } else {
                high = mid - 1;     // Current value is too large
            }
        }

        return ans;
    }
}
```

### Why `-1`?

If there is no value `<= x`, then a floor does not exist.

Example:

```text
arr = [3, 4, 7, 8]
x = 2
```

There is no valid floor, so:

```text
Floor = -1
```

---

# 🧠 Ceil using Binary Search

We want:

```text
smallest arr[mid] >= x
```

If:

```text
arr[mid] >= x
```

Then `arr[mid]` **could be the ceil**.

Store it:

```java
ans = arr[mid];
```

But there might be a smaller valid value on the left.

So:

```java
high = mid - 1;
```

If:

```text
arr[mid] < x
```

then the current value is too small, so move right:

```java
low = mid + 1;
```

---

## 🔼 Ceil Code

```java
class FloorCeilFinder {

    // Function to find ceiling
    public int findCeil(int[] arr, int x) {

        int low = 0;
        int high = arr.length - 1;
        int ans = -1;

        while (low <= high) {

            int mid = (low + high) / 2;

            if (arr[mid] >= x) {
                ans = arr[mid];     // Potential ceil
                high = mid - 1;     // Search for a smaller valid value
            } else {
                low = mid + 1;      // Current value is too small
            }
        }

        return ans;
    }
}
```

### Why `-1`?

If there is no value `>= x`, then a ceil does not exist.

Example:

```text
arr = [3, 4, 7, 8]
x = 10
```

There is no valid ceil:

```text
Ceil = -1
```

---

# 🔥 Floor vs Ceil

|                      | Floor                | Ceil                  |
| -------------------- | -------------------- | --------------------- |
| Meaning              | Largest value `<= x` | Smallest value `>= x` |
| If condition is true | Move **right**       | Move **left**         |
| Search for           | Larger valid value   | Smaller valid value   |
| No answer            | `-1`                 | `-1`                  |

The important pattern is:

```text
FLOOR
arr[mid] <= x
      ↓
  possible answer
      ↓
move RIGHT
```

```text
CEIL
arr[mid] >= x
      ↓
  possible answer
      ↓
move LEFT
```

---

# 📊 Complexity

| Operation | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Floor     | O(log n)        | O(1)             |
| Ceil      | O(log n)        | O(1)             |

---

# 🎯 Key Takeaways

* **Floor** = largest value `<= x`.
* **Ceil** = smallest value `>= x`.
* Both can be found efficiently using **Binary Search**.
* For Floor, when a valid value is found, **move right** to find a larger valid value.
* For Ceil, when a valid value is found, **move left** to find a smaller valid value.
* Return `-1` when no valid floor/ceil exists.
* Both work because the array is **sorted**.
