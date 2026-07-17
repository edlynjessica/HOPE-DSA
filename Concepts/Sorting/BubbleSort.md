# 🫧 Bubble Sort

> [!NOTE]
> **Bubble Sort** repeatedly compares adjacent elements and swaps them if they are in the wrong order.

### 🔗 Practice Link

**GeeksforGeeks:**  
https://www.geeksforgeeks.org/problems/bubble-sort/1

---

# 1️⃣ Idea

Bubble Sort repeatedly compares **adjacent elements** and swaps them if they are in the wrong order.

At every pass,

| Left Side | Right Side |
|-----------|------------|
| ⏳ Unsorted | ✅ Sorted |

Initially,

```text
Unsorted = Entire Array
Sorted   = Empty
```

### Example

```text
64 34 25 12 22

↓

34 25 12 22 | 64

↓

25 12 22 | 34 64

↓

12 22 | 25 34 64

↓

12 | 22 25 34 64

↓

12 22 25 34 64
```

---

# 2️⃣ Mental Model

> 🫧 Imagine air bubbles rising in water.

The **largest bubble** always rises to the top.

Similarly, after every pass, the **largest element** moves to the end of the array.

---

# 3️⃣ Algorithm

For every pass,

1. Compare adjacent elements.
2. Swap them if they are in the wrong order.
3. Continue until the unsorted part ends.
4. The largest element reaches its correct position.

Repeat until the array is sorted.

---

# 4️⃣ Iterative Code

```java
public static void bubbleSort(int[] arr){
    int n = arr.length;

    for(int i = 0; i < n - 1; i++){

        boolean swapped = false;

        for(int j = 0; j < n - i - 1; j++){

            if(arr[j] > arr[j + 1]){
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapped = true;
            }
        }

        if(!swapped)
            break;
    }
}
```

---

# 5️⃣ Recursive Code

```java
public static void bubbleSort(int[] arr){
    helper(arr, arr.length);
}

private static void helper(int[] arr, int n){

    if(n == 1)
        return;

    boolean swapped = false;

    for(int i = 0; i < n - 1; i++){

        if(arr[i] > arr[i + 1]){
            int temp = arr[i];
            arr[i] = arr[i + 1];
            arr[i + 1] = temp;
            swapped = true;
        }
    }

    if(!swapped)
        return;

    helper(arr, n - 1);
}
```

---

# 6️⃣ How Recursion Replaces Iteration

### Iterative

```text
for every pass
      ↓
compare adjacent elements
      ↓
largest reaches the end
      ↓
next pass
```

### Recursive

```text
helper(n)
      ↓
one pass till n
      ↓
largest reaches the end
      ↓
helper(n - 1)
```

Instead of the **loop** reducing the unsorted size, the **recursive call** reduces it.

---

# 7️⃣ Dry Run

Array

```text
[5, 1, 4, 2]
```

| Pass | Action | Array |
|------|--------|-------|
| Initial | Initial Array | 5 1 4 2 |
| 1 | Swap 5 & 1 | 1 5 4 2 |
| 1 | Swap 5 & 4 | 1 4 5 2 |
| 1 | Swap 5 & 2 | 1 4 2 5 |
| 2 | Swap 4 & 2 | 1 2 4 5 |
| 3 | No swaps | 1 2 4 5 |

✅ Sorted Array

```text
1 2 4 5
```

---

# 8️⃣ Complexity

### Time Complexity

| Case | Complexity |
|------|------------|
| 🟢 Best | **O(n)** |
| 🟡 Average | **O(n²)** |
| 🔴 Worst | **O(n²)** |

### Why?

- **Best Case:** No swaps occur during the first pass, so the algorithm terminates early.
- **Worst Case:** Every adjacent pair needs swapping (reverse sorted array).

### Space Complexity

| Type | Complexity |
|------|------------|
| Auxiliary Space | **O(1)** |

### Why?

Only one temporary variable is used during swapping.

No additional array or data structure is created.

---

# 9️⃣ Stable?

✅ **Yes**

> [!TIP]
> Only **adjacent elements** are swapped.
>
> Since the condition is
>
> ```java
> arr[j] > arr[j + 1]
> ```
>
> and **not**
>
> ```java
> arr[j] >= arr[j + 1]
> ```
>
> equal elements never swap places, so their **relative order remains unchanged**.

---

# 🔟 In-place?

✅ **Yes**

> [!TIP]
> Only one temporary variable is used for swapping.
>
> No extra array is created.

---

# 1️⃣1️⃣ Adaptive?

✅ **Yes**

> [!TIP]
> Using the **swapped** flag, if no swaps occur during a pass, the algorithm stops immediately.
>
> Therefore, the best-case time complexity becomes **O(n)**.

---

# 1️⃣2️⃣ Number of Comparisons

| Case | Comparisons |
|------|-------------|
| 🟢 Best (Optimized) | **n − 1** |
| 🔴 Worst | **n(n − 1) / 2** |

---

# 1️⃣3️⃣ Number of Swaps

| Case | Swaps |
|------|-------|
| 🟢 Best | **0** |
| 🔴 Worst | **n(n − 1) / 2** |

*(Reverse sorted array)*

---

# 1️⃣4️⃣ When is Bubble Sort Useful?

✅ Small datasets

✅ Detecting whether an array is already sorted

✅ Educational purposes

✅ Rarely used in real-world applications

---

# 1️⃣5️⃣ Key Takeaways

> [!IMPORTANT]
>
> ✅ Compares **adjacent elements**
>
> ✅ Largest element reaches the **end after every pass**
>
> ✅ Stable
>
> ✅ In-place
>
> ✅ Adaptive (using the **swapped** flag)
>
> ✅ Early termination is possible
