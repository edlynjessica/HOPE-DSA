# ⚡ Quick Sort

> [!NOTE]
> **Quick Sort** follows the **Divide and Conquer** paradigm by choosing a **pivot**, partitioning the array around it, and recursively sorting the resulting subarrays.

### 🔗 Practice Links

- **GeeksforGeeks:** https://www.geeksforgeeks.org/problems/quick-sort/1
- **LeetCode:** https://leetcode.com/problems/sort-an-array/

---

# 1️⃣ Idea

Quick Sort follows the **Divide and Conquer** approach.

It selects a **pivot** element and partitions the array such that

| Left Side | Pivot | Right Side |
|-----------|-------|------------|
| Smaller Elements | Pivot | Larger Elements |

The same process is then repeated recursively for the left and right subarrays.

Initially,

```text
Array = Unsorted
```

### Example

```text
38 27 43 3 9 82 10

↓

Choose Pivot = 3

↓

3 | 27 43 38 9 82 10

↓

Sort Right Part

↓

3 9 | 10 27 43 38 82

↓

Sort Remaining Parts

↓

3 9 10 27 38 43 82
```

---

# 2️⃣ Mental Model

> 👥 Imagine choosing one student as a reference.

Move all **shorter students** to the left.

Move all **taller students** to the right.

Now repeat the same process separately for both groups.

---

# 3️⃣ Algorithm

1. Choose a pivot element.
2. Partition the array around the pivot.
3. Recursively sort the left subarray.
4. Recursively sort the right subarray.

Repeat until every subarray contains at most one element.

---

# 4️⃣ Recursive Code

```java
public static void quickSort(int[] arr, int low, int high){

    if(low >= high)
        return;

    int start = low;
    int end = high;
    int pivot = arr[low + (high - low) / 2];

    while(start <= end){

        while(arr[start] < pivot)
            start++;

        while(arr[end] > pivot)
            end--;

        if(start <= end){

            int temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;

            start++;
            end--;
        }
    }

    quickSort(arr, low, end);
    quickSort(arr, start, high);
}
```

---

# 5️⃣ Iterative Code

```java
public static void quickSort(int[] arr){

    Stack<int[]> stack = new Stack<>();
    stack.push(new int[]{0, arr.length - 1});

    while(!stack.isEmpty()){

        int[] range = stack.pop();

        int low = range[0];
        int high = range[1];

        if(low >= high)
            continue;

        int start = low;
        int end = high;
        int pivot = arr[low + (high - low) / 2];

        while(start <= end){

            while(arr[start] < pivot)
                start++;

            while(arr[end] > pivot)
                end--;

            if(start <= end){

                int temp = arr[start];
                arr[start] = arr[end];
                arr[end] = temp;

                start++;
                end--;
            }
        }

        if(low < end)
            stack.push(new int[]{low, end});

        if(start < high)
            stack.push(new int[]{start, high});
    }
}
```

---

# 6️⃣ How Recursion Replaces Iteration

### Recursive

```text
Choose Pivot
      ↓
Partition
      ↓
Sort Left Half
      ↓
Sort Right Half
```

### Iterative

```text
Choose Pivot
      ↓
Partition
      ↓
Push Left Range into Stack
      ↓
Push Right Range into Stack
      ↓
Repeat until Stack becomes empty
```

Instead of recursive calls maintaining the subarrays, the **stack stores the subarray boundaries**.

---

# 7️⃣ Dry Run

Array

```text
[8, 4, 7, 9, 3, 10, 5]
```

### First Partition

```text
Pivot = 9

↓

8 4 7 5 3 | 9 | 10
```

### Sort Left Part

```text
Pivot = 7

↓

4 3 5 | 7 | 8
```

### Sort Remaining Part

```text
3 4 5
```

✅ Sorted Array

```text
3 4 5 7 8 9 10
```

---

# 8️⃣ Complexity

### Time Complexity

| Case | Complexity |
|------|------------|
| 🟢 Best | **O(n log n)** |
| 🟡 Average | **O(n log n)** |
| 🔴 Worst | **O(n²)** |

### Why?

- Every partition processes all elements once.
- Balanced partitions produce **log₂ n** recursive levels.
- Highly unbalanced partitions lead to **O(n²)**.

### Space Complexity

| Case | Complexity |
|------|------------|
| Average | **O(log n)** |
| Worst | **O(n)** |

### Why?

Quick Sort is **in-place**, but recursive calls require stack space.

Balanced partitions require **O(log n)** recursion depth, while highly unbalanced partitions require **O(n)**.

---

# 9️⃣ Stable?

❌ **No**

> [!TIP]
> Swapping elements during partitioning may change the **relative order of equal elements**.
>
> Therefore, Quick Sort is **not stable**.

---

# 🔟 In-place?

✅ **Yes**

> [!TIP]
> Only a few variables are used during partitioning.
>
> No temporary array is required.

---

# 1️⃣1️⃣ Adaptive?

❌ **No**

> [!TIP]
> Even if the array is already or nearly sorted, Quick Sort still performs partitioning.
>
> A poor pivot choice may even lead to the **worst-case O(n²)** complexity.

---

# 1️⃣2️⃣ Number of Comparisons

| Case | Comparisons |
|------|-------------|
| 🟢 Best / Average | **O(n log n)** |
| 🔴 Worst | **O(n²)** |

> The number of comparisons depends on how balanced the partitions are.

---

# 1️⃣3️⃣ Number of Swaps

| Case | Swaps |
|------|-------|
| 🟡 Average | **O(n log n)** |
| 🔴 Worst | **O(n²)** |

> The number of swaps depends on the chosen pivot and partitioning.

---

# 1️⃣4️⃣ When is Quick Sort Useful?

✅ Large arrays

✅ In-memory sorting

✅ General-purpose sorting

✅ Faster than Merge Sort in practice due to better cache performance

✅ Used in many standard library implementations (with optimizations)

---

# 1️⃣5️⃣ Key Takeaways

> [!IMPORTANT]
>
> ✅ Divide and Conquer algorithm
>
> ✅ Pivot-based sorting
>
> ✅ In-place
>
> ✅ Very fast on average
>
> ✅ Average Time Complexity = **O(n log n)**
>
> ❌ Worst Case = **O(n²)**
>
> ❌ Not Stable

---

# 1️⃣6️⃣ Pivot Selection Methods

| Method | Description |
|--------|-------------|
| First Element | Simplest approach, but may degrade on sorted arrays |
| Last Element | Common implementation, same drawbacks as first element |
| Middle Element | Better for many practical cases |
| Random Pivot | Reduces the probability of worst-case partitions |
| Median of Three | Chooses the median of first, middle, and last elements for more balanced partitions |

> [!TIP]
> A **good pivot** produces balanced partitions, giving **O(n log n)** performance.
>
> A **poor pivot** creates highly unbalanced partitions, degrading the algorithm to **O(n²)**.
