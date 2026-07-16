# Merge Sort

## 1. Idea

Merge Sort follows the **Divide and Conquer** approach.

It repeatedly divides the array into smaller halves until each subarray contains only one element.

Then, it merges the sorted subarrays back together to form the final sorted array.

Initially,

Whole Array

Example:

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

---

## 2. Mental Model

Imagine sorting a large stack of papers.

Instead of sorting the entire stack at once, split it into two smaller stacks.
Keep dividing until each stack contains only one paper.

Finally, merge the smaller sorted stacks to obtain one completely sorted stack.

---

## 3. Algorithm

1. Divide the array into two halves.
2. Recursively sort the left half.
3. Recursively sort the right half.
4. Merge the two sorted halves.

Repeat until the entire array becomes sorted.

---

## 4. Recursive Code

```java
public static void mergeSort(int[] arr, int left, int right){

    if(left >= right)
        return;

    int mid = left + (right - left) / 2;

    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);

    merge(arr, left, mid, right);
}

private static void merge(int[] arr, int left, int mid, int right){

    int[] temp = new int[right - left + 1];

    int i = left;
    int j = mid + 1;
    int k = 0;

    while(i <= mid && j <= right){

        if(arr[i] <= arr[j]){
            temp[k++] = arr[i++];
        }else{
            temp[k++] = arr[j++];
        }

    }
    while(i <= mid){
        temp[k++] = arr[i++];
    }
    while(j <= right){
        temp[k++] = arr[j++];
    }  
    for(int x = 0; x < temp.length; x++){
        arr[left + x] = temp[x];
    }
}
```

---

## 5. Iterative Code (Bottom-Up Merge Sort)

```java
public static void mergeSort(int[] arr){

    int n = arr.length;

    for(int size = 1; size < n; size *= 2){

        for(int left = 0; left < n - size; left += 2 * size){

            int mid = left + size - 1;
            int right = Math.min(left + 2 * size - 1, n - 1);

            merge(arr, left, mid, right);

        }

    }

}
```

> The `merge()` function remains the same as the recursive version.

---

## 6. How recursion replaces iteration

#### Recursive

Divide

→ Sort Left Half

→ Sort Right Half

→ Merge

#### Iterative

Merge subarrays of size 1

→ Merge subarrays of size 2

→ Merge subarrays of size 4

→ Merge subarrays of size 8

→ Continue until the entire array is merged.

Instead of recursively dividing the array,
the iterative version starts with single-element subarrays and repeatedly merges larger blocks.

---

## 7. Dry Run

Array:

[8,4,5,2]

Divide

8 4 | 5 2

↓

8 | 4 | 5 | 2

Merge

8 + 4

↓

4 8

Merge

5 + 2

↓

2 5

Final Merge

4 8

2 5

↓

2 4 5 8

Sorted.

---

## 8. Complexity

Best Case  : O(n log n)

Average    : O(n log n)

Worst      : O(n log n)

Extra Space: O(n)

Reason:

The array is divided into **log₂n** levels.
At every level, all **n** elements are processed during merging.

Hence, 
O(log n) × O(n) = **O(n log n)**

---

## 9. Stable?

✅ Yes

Reason:

When two elements are equal,

```java
if(arr[i] <= arr[j])
```

the left element is copied first.
Thus, the original relative order of equal elements is preserved.

---

## 10. In-place?

❌ No

Reason:

A temporary array is required during every merge operation.
Hence, Merge Sort requires O(n) extra space.

---

## 11. Adaptive?

❌ No

Reason:

Even if the array is already sorted,
Merge Sort still divides and merges the entire array.
Its running time remains O(n log n).

---

## 12. Number of Comparisons

Best Case : O(n log n)

Worst Case : O(n log n)

Reason:

Every level processes all elements,
and there are log₂n levels.

---

## 13. Number of Merge Operations

At every level, adjacent sorted subarrays are merged.

Number of levels = log₂n

Every level processes all n elements.

---

## 14. When is Merge Sort useful?

- Large datasets.
- Linked Lists.
- External Sorting.
- Stable sorting is required.
- Guaranteed O(n log n) performance is needed.

---

## 15. Key Takeaways

✅ Divide and Conquer algorithm.

✅ Stable.

✅ Guaranteed O(n log n) time complexity.

✅ Recursive and Iterative implementations are possible.

✅ Requires O(n) extra space.

✅ Preferred for Linked Lists and External Sorting.
