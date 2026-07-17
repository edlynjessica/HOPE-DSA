# 🌳 Heap Sort

> [!NOTE]
> **Heap Sort** uses a **Binary Heap** to repeatedly extract the largest element and place it at its correct position.

### 🔗 Practice Links

- **GeeksforGeeks:** https://www.geeksforgeeks.org/problems/heap-sort/1
- **LeetCode:** https://leetcode.com/problems/sort-an-array/

---

# 1️⃣ Idea

Heap Sort uses the **Binary Heap** data structure to sort elements.

For **ascending order**,

1. Build a **Max Heap**.
2. The largest element will always be at the **root**.
3. Swap the root with the last element.
4. Reduce the heap size.
5. Heapify the root again.

Repeat until only one element remains.

### Max Heap Example

Array

```text
[4, 10, 3, 5, 1]
```

Max Heap

```text
        10
       /  \
      5    3
     / \
    4   1
```

### Sorting Process

```text
4 10 3 5 1

↓

Build Max Heap

↓

10 5 3 4 1

↓

Swap Root & Last

↓

1 5 3 4 | 10

↓

Heapify

↓

5 4 3 1 | 10

↓

Swap

↓

1 4 3 | 5 10

↓

Heapify

↓

4 1 3 | 5 10

↓

Swap

↓

3 1 | 4 5 10

↓

Heapify

↓

3 1 | 4 5 10

↓

Swap

↓

1 | 3 4 5 10
```

✅ Sorted Array

```text
1 3 4 5 10
```

---

# 2️⃣ Mental Model

> 🏆 Imagine a tournament.

The **strongest player** always reaches the top.

Remove the winner, conduct the tournament again, and repeat until everyone is ranked.

---

# 3️⃣ Algorithm

1. Build a **Max Heap**.
2. Swap the root with the last element.
3. Reduce the heap size.
4. Heapify the root.
5. Repeat until the heap size becomes **1**.

---

# 4️⃣ Iterative Code

```java
public static void heapSort(int[] arr){

    int n = arr.length;

    for(int i = n/2 - 1; i >= 0; i--){
        heapify(arr, n, i);
    }

    for(int i = n - 1; i > 0; i--){

        int temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;

        heapify(arr, i, 0);
    }
}

private static void heapify(int[] arr, int n, int i){

    while(true){

        int largest = i;
        int left = 2*i + 1;
        int right = 2*i + 2;

        if(left < n && arr[left] > arr[largest])
            largest = left;

        if(right < n && arr[right] > arr[largest])
            largest = right;

        if(largest == i)
            break;

        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;

        i = largest;
    }
}
```

---

# 5️⃣ Recursive Code

```java
public static void heapSort(int[] arr){

    int n = arr.length;

    for(int i = n/2 - 1; i >= 0; i--){
        heapify(arr, n, i);
    }

    for(int i = n - 1; i > 0; i--){

        int temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;

        heapify(arr, i, 0);
    }
}

private static void heapify(int[] arr, int n, int i){

    int largest = i;
    int left = 2*i + 1;
    int right = 2*i + 2;

    if(left < n && arr[left] > arr[largest])
        largest = left;

    if(right < n && arr[right] > arr[largest])
        largest = right;

    if(largest != i){

        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;

        heapify(arr, n, largest);
    }
}
```

---

# 6️⃣ How Recursion Replaces Iteration

### Iterative

```text
Build Heap
      ↓
Swap Root
      ↓
Heapify using loop
      ↓
Repeat
```

### Recursive

```text
Build Heap
      ↓
Swap Root
      ↓
Heapify using recursion
      ↓
Repeat
```

Instead of repeatedly moving down the heap using a **loop**, the recursive version calls **heapify()** on the affected child.

---

# 7️⃣ Dry Run

Array

```text
[4, 10, 3, 5, 1]
```

### Build Max Heap

```text
        10
       /  \
      5    3
     / \
    4   1
```

↓

```text
10 5 3 4 1
```

### Extract Maximum

```text
1 5 3 4 | 10
```

↓

Heapify

```text
5 4 3 1 | 10
```

↓

Extract Again

```text
1 4 3 | 5 10
```

↓

Heapify

```text
4 1 3 | 5 10
```

↓

Extract Again

```text
3 1 | 4 5 10
```

↓

Extract Again

```text
1 | 3 4 5 10
```

✅ Sorted Array

```text
1 3 4 5 10
```

---

# 8️⃣ Complexity

### Time Complexity

| Case | Complexity |
|------|------------|
| 🟢 Best | **O(n log n)** |
| 🟡 Average | **O(n log n)** |
| 🔴 Worst | **O(n log n)** |

### Why?

- Building the heap takes **O(n)**.
- Each extraction requires **O(log n)**.
- Total complexity becomes:

```text
O(n) + O(n log n)
      =
O(n log n)
```

### Space Complexity

| Implementation | Complexity |
|---------------|------------|
| Iterative | **O(1)** |
| Recursive | **O(log n)** |

### Why?

- The iterative implementation uses constant auxiliary space.
- The recursive implementation uses recursion stack space of **O(log n)**.

---

# 9️⃣ Stable?

❌ **No**

> [!TIP]
> Swapping the root with the last element may change the **relative order of equal elements**.
>
> Therefore, Heap Sort is **not stable**.

---

# 🔟 In-place?

✅ **Yes**

> [!TIP]
> Sorting is performed within the original array.
>
> No additional array is required.

---

# 1️⃣1️⃣ Adaptive?

❌ **No**

> [!TIP]
> Even if the array is already sorted, Heap Sort still builds the heap and performs every extraction.
>
> Therefore, its running time remains **O(n log n)**.

---

# 1️⃣2️⃣ Number of Comparisons

| Case | Comparisons |
|------|-------------|
| 🟡 Average | **O(n log n)** |
| 🔴 Worst | **O(n log n)** |

---

# 1️⃣3️⃣ Number of Swaps

| Case | Swaps |
|------|-------|
| Approximate | **O(n log n)** |

> Every extracted element may require multiple swaps while restoring the heap property.

---

# 1️⃣4️⃣ When is Heap Sort Useful?

✅ Guaranteed **O(n log n)** performance

✅ Memory-constrained systems

✅ Priority Queue implementation

✅ When worst-case performance matters

---

# 1️⃣5️⃣ Key Takeaways

> [!IMPORTANT]
>
> ✅ Uses a **Binary Heap**
>
> ✅ Root always contains the maximum element
>
> ✅ In-place
>
> ✅ Guaranteed **O(n log n)** time complexity
>
> ❌ Not Stable
>
> ✅ Preferred when worst-case guarantees are important

---

# 1️⃣6️⃣ Important Terminologies

| Term | Meaning |
|------|---------|
| **Heap** | A Complete Binary Tree satisfying the Heap Property |
| **Max Heap** | Parent ≥ Children |
| **Min Heap** | Parent ≤ Children |
| **Heapify** | Restores the heap property |
| **Build Heap** | Converts an array into a valid heap |
| **Root** | First element (index `0`) |
| **Leaf Nodes** | Nodes with no children |
| **Complete Binary Tree** | Every level is full except possibly the last, which is filled from left to right |

---

# 1️⃣7️⃣ Heap Index Formulae

For a node at index **i**,

| Node | Formula |
|------|---------|
| Left Child | `2 * i + 1` |
| Right Child | `2 * i + 2` |
| Parent | `(i - 1) / 2` |

### Visual Representation

```text
              i
            /   \
      2*i+1     2*i+2
       (L)       (R)

Parent of any node:

      (i-1)/2
         │
         ▼
         i
```
