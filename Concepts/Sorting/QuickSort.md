# Quick Sort

## 1. Idea

Quick Sort follows the **Divide and Conquer** approach.

It selects a **pivot** element and partitions the array such that

- Elements smaller than the pivot are placed on the left.
- Elements larger than the pivot are placed on the right.

The same process is then repeated recursively for the left and right subarrays.

Initially,

Array = Unsorted

Example:

38 27 43 3 9 82 10

→ Choose Pivot = 3

→ 3 | 27 43 38 9 82 10

→ Sort Right Part

→ 3 9 | 10 27 43 38 82

→ Sort Remaining Parts

→ 3 9 10 27 38 43 82

---

## 2. Mental Model

Imagine choosing one student as a reference.
Move all shorter students to the left.
Move all taller students to the right.
Now repeat the same process separately for both groups.

---

## 3. Algorithm

1. Choose a pivot element.
2. Partition the array around the pivot.
3. Recursively sort the left subarray.
4. Recursively sort the right subarray.

Repeat until every subarray contains at most one element.

---

## 4. Recursive Code

```java
public static void quickSort(int[] arr, int low, int high){

    if(low >= high)
        return;

    int start = low;
    int end = high;
    int pivot = arr[low + (high - low) / 2];

    while(start <= end){

        while(arr[start] < pivot) start++;

        while(arr[end] > pivot) end--;

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

## 5. Iterative Code

```java
public static void quickSort(int[] arr){

    Stack<int[]> stack = new Stack<>();
    stack.push(new int[]{0, arr.length-1});

    while(!stack.isEmpty()){

        int[] range = stack.pop();

        int low = range[0];
        int high = range[1];

        if(low >= high)
            continue;

        int start = low;
        int end = high;
        int pivot = arr[low + (high-low)/2];

        while(start <= end){

            while(arr[start] < pivot) start++;

            while(arr[end] > pivot) end--;

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

## 6. How recursion replaces iteration

#### Recursive:

Choose Pivot

→ Partition

→ Sort Left Half

→ Sort Right Half

#### Iterative:

Choose Pivot

→ Partition

→ Push Left Range into Stack

→ Push Right Range into Stack

→ Repeat until Stack becomes empty.

Instead of the recursive calls maintaining the subarrays,
the stack stores the subarray boundaries.

---

## 7. Dry Run

Array:

[8,4,7,9,3,10,5]

Pivot = 9

→ 8 4 7 5 3 | 9 | 10

Sort Left

Pivot = 7

→ 4 3 5 | 7 | 8

Sort Left

→ 3 4 5

Final Array

→ 3 4 5 7 8 9 10

Sorted.

---

## 8. Complexity

Best Case  : O(n log n)

Average    : O(n log n)

Worst      : O(n²)

Extra Space:

Recursive : O(log n) (Average)

Worst Case : O(n)

Reason:

Each partition processes all elements once.
Balanced partitions produce log₂n levels.
Highly unbalanced partitions lead to O(n²).

---

## 9. Stable?

❌ No

Reason:

Swapping during partitioning may change the relative order of equal elements.

---

## 10. In-place?

✅ Yes

Reason:

Only a few variables are used.
No temporary array is required.

---

## 11. Adaptive?

❌ No

Reason:

Even if the array is nearly sorted,

Quick Sort still performs partitioning.
A poor pivot choice may even produce the worst-case complexity.

---

## 12. Number of Comparisons

Best / Average : O(n log n)

Worst : O(n²)

Depends on how balanced the partitions are.

---

## 13. Number of Swaps

Depends on the pivot.

Average : O(n log n)

Worst : O(n²)

---

## 14. When is Quick Sort useful?

- Large arrays.
- In-memory sorting.
- General-purpose sorting.
- Faster than Merge Sort in practice due to better cache performance.
- Used in many standard library implementations (with optimizations).

---

## 15. Key Takeaways

✅ Divide and Conquer algorithm.

✅ Pivot-based sorting.

✅ In-place.

✅ Very fast on average.

✅ Average Time Complexity = O(n log n).

✅ Worst Case = O(n²).

✅ Not Stable.

---

## 16. Pivot Selection Methods

- First Element
- Last Element
- Middle Element
- Random Pivot
- Median of Three

A good pivot gives balanced partitions and O(n log n).

A poor pivot gives highly unbalanced partitions and O(n²).
