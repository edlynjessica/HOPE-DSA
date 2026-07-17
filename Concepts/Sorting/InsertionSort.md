# 📥 Insertion Sort

> [!NOTE]
> **Insertion Sort** builds a sorted portion of the array by inserting one element at a time into its correct position.

### 🔗 Practice Link

**GeeksforGeeks:**  
https://www.geeksforgeeks.org/problems/insertion-sort/1

---

# 1️⃣ Idea

Insertion Sort builds the sorted array **one element at a time**.

At every pass,

| Left Side | Right Side |
|-----------|------------|
| ✅ Sorted | ⏳ Unsorted |

Each new element is picked from the **unsorted** part and inserted into its correct position in the **sorted** part.

Initially,

```
Sorted   = First Element
Unsorted = Remaining Elements
```

### Example

```text
64 | 25 12 22 11

↓

25 64 | 12 22 11

↓

12 25 64 | 22 11

↓

12 22 25 64 | 11

↓

11 12 22 25 64
```

---

# 2️⃣ Mental Model

> 🃏 Imagine arranging playing cards in your hand.

You pick **one new card** at a time and insert it into its correct position among the cards that are already arranged.

---

# 3️⃣ Algorithm

For every element starting from **index 1**:

1. Store the current element (**key**).
2. Compare it with elements on its left.
3. Shift all larger elements one position to the right.
4. Insert the key into its correct position.

Repeat until the array is sorted.

---

# 4️⃣ Iterative Code

```java
public static void insertionSort(int[] arr){
    int n = arr.length;

    for(int i = 1; i < n; i++){
        int key = arr[i];
        int j = i - 1;

        while(j >= 0 && arr[j] > key){
            arr[j + 1] = arr[j];
            j--;
        }

        arr[j + 1] = key;
    }
}
```

---

# 5️⃣ Recursive Code

```java
public static void insertionSort(int[] arr){
    helper(arr,1);
}

private static void helper(int[] arr,int index){

    if(index == arr.length)
        return;

    int key = arr[index];
    int j = index - 1;

    while(j >= 0 && arr[j] > key){
        arr[j + 1] = arr[j];
        j--;
    }

    arr[j + 1] = key;

    helper(arr,index + 1);
}
```

---

# 6️⃣ How Recursion Replaces Iteration

### Iterative

```text
for every index
      ↓
pick key
      ↓
shift larger elements
      ↓
insert key
      ↓
next index
```

### Recursive

```text
helper(index)
      ↓
insert current element
      ↓
helper(index + 1)
```

Instead of the **loop** increasing the index, the **recursive call** increases it.

---

# 7️⃣ Dry Run

Array

```text
[5, 2, 4, 6, 1]
```

| Pass | Key | Action | Array |
|------|-----|--------|-------|
| Initial | - | Initial Array | 5 2 4 6 1 |
| 1 | 2 | 5 shifts right | 2 5 4 6 1 |
| 2 | 4 | 5 shifts right | 2 4 5 6 1 |
| 3 | 6 | Already in correct position | 2 4 5 6 1 |
| 4 | 1 | 6,5,4,2 shift right | 1 2 4 5 6 |

✅ Sorted Array

```text
1 2 4 5 6
```

---

# 8️⃣ Complexity

| Case | Time Complexity |
|------|-----------------|
| ✅ Best | **O(n)** |
| 📈 Average | **O(n²)** |
| ❌ Worst | **O(n²)** |
| 💾 Extra Space | **O(1)** |

### Why?

- If the array is already sorted, only **one comparison** is needed per pass.
- In the worst case (reverse sorted), every element shifts through the sorted portion.

---

# 9️⃣ Stable?

✅ **Yes**

> [!TIP]
> Only elements **greater than** the key are shifted.
>
> Equal elements never cross each other, so their **relative order remains unchanged**.

---

# 🔟 In-place?

✅ **Yes**

> [!TIP]
> Only a few variables are used.
>
> No extra array is created.

---

# 1️⃣1️⃣ Adaptive?

✅ **Yes**

> [!TIP]
> Already sorted arrays require very few comparisons and **no shifting**.
>
> Therefore, the algorithm runs in **O(n)** for the best case.

---

# 1️⃣2️⃣ Number of Comparisons

| Case | Comparisons |
|------|-------------|
| Best | **n − 1** |
| Worst | **n(n − 1) / 2** |

---

# 1️⃣3️⃣ Number of Shifts

| Case | Shifts |
|------|---------|
| Best | **0** |
| Worst | **n(n − 1) / 2** |

*(Reverse sorted array)*

---

# 1️⃣4️⃣ When is Insertion Sort Useful?

✅ Small datasets

✅ Nearly sorted arrays

✅ Online sorting (elements arrive one by one)

✅ Used inside advanced algorithms like **TimSort**

---

# 1️⃣5️⃣ Key Takeaways

> [!IMPORTANT]
>
> ✅ Builds the sorted portion **one element at a time**
>
> ✅ Inserts each element into its **correct position**
>
> ✅ Stable
>
> ✅ In-place
>
> ✅ Adaptive
>
> ✅ Extremely efficient for **nearly sorted arrays**
