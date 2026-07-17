# 🔀 Merge Sort

> [!NOTE]
> **Merge Sort** follows the **Divide and Conquer** paradigm by recursively splitting the array into smaller halves, sorting them, and merging them back together.

### 🔗 Practice Links

- **GeeksforGeeks:** https://www.geeksforgeeks.org/problems/merge-sort/1
- **LeetCode:** https://leetcode.com/problems/sort-an-array/
- **My LeetCode Solution:** https://leetcode.com/problems/sort-an-array/submissions/2071306168

---

# 1️⃣ Idea

Merge Sort follows the **Divide and Conquer** approach.

It repeatedly divides the array into smaller halves until each subarray contains only one element.

Then, it merges the sorted subarrays back together to form the final sorted array.

Initially,

```text
Whole Array
```

### Example

```text
38 27 43 3 9 82 10

↓

38 27 43 3 | 9 82 10

↓

38 27 | 43 3 | 9 82 | 10

↓

38 | 27 | 43 | 3 | 9 | 82 | 10

↓

27 38 | 3 43 | 9 82 | 10

↓

3 27 38 43 | 9 10 82

↓

3 9 10 27 38 43 82
```

---

# 2️⃣ Mental Model

> 📄 Imagine sorting a large stack of papers.

Instead of sorting the entire stack at once, split it into **two smaller stacks**.

Keep dividing until each stack contains only **one paper**.

Finally, merge the smaller sorted stacks to obtain one completely sorted stack.

---

# 3️⃣ Algorithm

1. Divide the array into two halves.
2. Recursively sort the left half.
3. Recursively sort the right half.
4. Merge the two sorted halves.

Repeat until the entire array becomes sorted.

---

# 4️⃣ Recursive Code

```java
public static void mergeSort(int[] arr, int left, int right){

    if(left >= right)
        return;

    int mid = left + (right - left) / 2;

    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);

    merge(arr, left, mid, right);
}

public static void merge(int[] arr, int left, int mid, int right){

    // Create Left Array
    int[] leftArr = new int[mid - left + 1];

    // Create Right Array
    int[] rightArr = new int[right - mid];

    // Copy Left Half
    for(int i = 0; i < leftArr.length; i++){
        leftArr[i] = arr[left + i];
    }

    // Copy Right Half
    for(int i = 0; i < rightArr.length; i++){
        rightArr[i] = arr[mid + 1 + i];
    }

    int i = 0;
    int j = 0;
    int k = left;

    // Merge both arrays
    while(i < leftArr.length && j < rightArr.length){

        if(leftArr[i] <= rightArr[j]){
            arr[k] = leftArr[i];
            i++;
        }else{
            arr[k] = rightArr[j];
            j++;
        }

        k++;
    }

    // Copy remaining left elements
    while(i < leftArr.length){
        arr[k] = leftArr[i];
        i++;
        k++;
    }

    // Copy remaining right elements
    while(j < rightArr.length){
        arr[k] = rightArr[j];
        j++;
        k++;
    }
}
```

---

# 5️⃣ Iterative Code (Bottom-Up Merge Sort)

```java
public static void mergeSort(int[] arr){

    int n = arr.length;

    // size = current size of subarrays to merge
    for(int size = 1; size < n; size *= 2){

        // left = starting index of first subarray
        for(int left = 0; left < n - size; left += 2 * size){

            // Ending index of first subarray
            int mid = left + size - 1;

            // Ending index of second subarray
            int right = Math.min(left + 2 * size - 1, n - 1);

            // Merge the two sorted subarrays
            merge(arr, left, mid, right);
        }
    }
}
```

> [!TIP]
> The `merge()` function remains the same as the recursive version.

---

# 6️⃣ How Recursion Replaces Iteration

### Recursive

```text
Divide
      ↓
Sort Left Half
      ↓
Sort Right Half
      ↓
Merge
```

### Iterative

```text
Merge subarrays of size 1
      ↓
Merge subarrays of size 2
      ↓
Merge subarrays of size 4
      ↓
Merge subarrays of size 8
      ↓
Continue until the entire array is merged
```

Instead of **recursively dividing** the array, the iterative version starts with **single-element subarrays** and repeatedly merges larger blocks.

---

# 7️⃣ Dry Run

Array

```text
[8, 4, 5, 2]
```

### Divide

```text
8 4 | 5 2

↓

8 | 4 | 5 | 2
```

### Merge

```text
8 + 4

↓

4 8
```

```text
5 + 2

↓

2 5
```

### Final Merge

```text
4 8

2 5

↓

2 4 5 8
```

✅ Sorted Array

```text
2 4 5 8
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

- The array is divided into **log₂ n** levels.
- At each level, all **n** elements are processed during merging.

Therefore,

**O(log n) × O(n) = O(n log n)**

### Space Complexity

| Type | Complexity |
|------|------------|
| Auxiliary Space | **O(n)** |

### Why?

Temporary arrays are created during each merge operation.

Hence, Merge Sort requires **O(n)** extra space.

---

# 9️⃣ Stable?

✅ **Yes**

> [!TIP]
> When two elements are equal,
>
> ```java
> if(leftArr[i] <= rightArr[j])
> ```
>
> the element from the **left subarray** is copied first.
>
> Therefore, the **relative order of equal elements is preserved**.

---

# 🔟 In-place?

❌ **No**

> [!TIP]
> Merge Sort creates temporary arrays while merging.
>
> Hence, it requires **O(n)** auxiliary space.

---

# 1️⃣1️⃣ Adaptive?

❌ **No**

> [!TIP]
> Even if the array is already sorted, Merge Sort still performs every divide and merge step.
>
> Therefore, the running time remains **O(n log n)**.

---

# 1️⃣2️⃣ Number of Comparisons

| Case | Comparisons |
|------|-------------|
| 🟢 Best | **O(n log n)** |
| 🔴 Worst | **O(n log n)** |

> Every level processes all elements, and there are **log₂ n** levels.

---

# 1️⃣3️⃣ Number of Merge Operations

- Adjacent sorted subarrays are merged at every level.
- Number of levels = **log₂ n**
- Every level processes all **n** elements.

---

# 1️⃣4️⃣ When is Merge Sort Useful?

✅ Large datasets

✅ Linked Lists

✅ External Sorting

✅ Stable sorting is required

✅ Guaranteed **O(n log n)** performance is needed

---

# 1️⃣5️⃣ Key Takeaways

> [!IMPORTANT]
>
> ✅ Divide and Conquer algorithm
>
> ✅ Stable
>
> ✅ Guaranteed **O(n log n)** time complexity
>
> ✅ Recursive and Iterative implementations are possible
>
> ❌ Not In-place (requires **O(n)** extra space)
>
> ✅ Preferred for Linked Lists and External Sorting
