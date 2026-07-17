# 🎯 Selection Sort

> [!NOTE]
> **Selection Sort** repeatedly finds the **smallest**(minimum) element from the unsorted portion and places it at its correct position.

### 🔗 Practice Link

**GeeksforGeeks:**  
https://www.geeksforgeeks.org/problems/selection-sort/1

---

# 1️⃣ Idea

Selection Sort repeatedly finds the **smallest element** from the unsorted part of the array and places it at its correct position.

At every pass,

| Left Side | Right Side |
|-----------|------------|
| ✅ Sorted | ⏳ Unsorted |

Initially,

```text
Sorted   = Empty
Unsorted = Entire Array
```

### Example

```text
64 25 12 22 11

↓

11 | 25 12 22 64

↓

11 12 | 25 22 64

↓

11 12 22 | 25 64

↓

11 12 22 25 | 64

↓

11 12 22 25 64
```

---

# 2️⃣ Mental Model

> 📏 Imagine arranging students by height.

Instead of swapping every time you see a shorter student, you first walk through the **entire line**, find the **shortest student**, and then place them at the front.

Repeat the same process for the remaining students.

---

# 3️⃣ Algorithm

For every position **i**,

1. Assume the current element is the minimum.
2. Search the remaining array.
3. Find the actual minimum.
4. Swap it with index **i**.

Repeat until only one element remains.

---

# 4️⃣ Iterative Code

```java
public static void selectionSort(int[] arr){
    int n = arr.length;

    for(int i = 0; i < n - 1; i++){

        int minIndex = i;

        for(int j = i + 1; j < n; j++){
            if(arr[j] < arr[minIndex]){
                minIndex = j;
            }
        }

        int temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
}
```

---

# 5️⃣ Recursive Code

```java
public static void selectionSort(int[] arr){
    helper(arr,0);
}

private static void helper(int[] arr,int start){

    if(start == arr.length - 1)
        return;

    int minIndex = start;

    for(int i = start + 1; i < arr.length; i++){
        if(arr[i] < arr[minIndex]){
            minIndex = i;
        }
    }

    int temp = arr[start];
    arr[start] = arr[minIndex];
    arr[minIndex] = temp;

    helper(arr,start + 1);
}
```

---

# 6️⃣ How Recursion Replaces Iteration

### Iterative

```text
for every i
      ↓
find minimum
      ↓
swap
      ↓
next i
```

### Recursive

```text
helper(start)
      ↓
find minimum
      ↓
swap
      ↓
helper(start + 1)
```

Instead of the **loop** changing the starting index, the **recursive call** changes it.

---

# 7️⃣ Dry Run

Array

```text
[29, 10, 14, 37, 13]
```

| Pass | Minimum | Action | Array |
|------|---------|--------|-------|
| Initial | - | Initial Array | 29 10 14 37 13 |
| 1 | 10 | Swap with 29 | 10 29 14 37 13 |
| 2 | 13 | Swap with 29 | 10 13 14 37 29 |
| 3 | 14 | Already in correct position | 10 13 14 37 29 |
| 4 | 29 | Swap with 37 | 10 13 14 29 37 |

✅ Sorted Array

```text
10 13 14 29 37
```

---

# 8️⃣ Complexity

### Time Complexity

| Case | Complexity |
|------|------------|
| 🟢 Best | **O(n²)** |
| 🟡 Average | **O(n²)** |
| 🔴 Worst | **O(n²)** |

### Why?

- Selection Sort always scans the **entire unsorted portion** to find the minimum.
- Therefore, the number of comparisons remains the same regardless of the input.

### Space Complexity

| Type | Complexity |
|------|------------|
| Auxiliary Space | **O(1)** |

### Why?

Only a few extra variables (`minIndex`, `temp`) are used.

No additional array or data structure is created.

---

# 9️⃣ Stable?

❌ **No**

> [!TIP]
> The minimum element may **jump ahead** of equal elements during swapping.
>
> This changes their **relative order**, making Selection Sort **unstable**.

---

# 🔟 In-place?

✅ **Yes**

> [!TIP]
> Only a few variables are used.
>
> No extra array is created.

---

# 1️⃣1️⃣ Adaptive?

❌ **No**

> [!TIP]
> Even if the array is already sorted, Selection Sort still performs all comparisons.
>
> Therefore, its running time remains **O(n²)**.

---

# 1️⃣2️⃣ Number of Comparisons

| Case | Comparisons |
|------|-------------|
| Best | **n(n − 1) / 2** |
| Average | **n(n − 1) / 2** |
| Worst | **n(n − 1) / 2** |

> Comparisons are **fixed** for every input.

---

# 1️⃣3️⃣ Number of Swaps

| Case | Swaps |
|------|-------|
| Maximum | **n − 1** |

> Only **one swap per pass** is performed.
>
> This is significantly fewer than Bubble Sort.

---

# 1️⃣4️⃣ When is Selection Sort Useful?

✅ Memory is limited

✅ Swapping is expensive

✅ Small datasets

---

# 1️⃣5️⃣ Key Takeaways

> [!IMPORTANT]
>
> ✅ Finds the **minimum element** in every pass
>
> ✅ Performs only **one swap per pass**
>
> ✅ Number of comparisons never reduces
>
> ❌ Not Stable
>
> ✅ In-place
>
> ❌ Not Adaptive
