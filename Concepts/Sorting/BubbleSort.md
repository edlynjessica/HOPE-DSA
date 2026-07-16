# Bubble Sort

## 1. Idea

Bubble Sort repeatedly compares **adjacent elements** and swaps them if they are in the wrong order.

At every pass,

- Left side → Unsorted
- Right side → Sorted

Initially,

Unsorted = Entire Array

Sorted = Empty

Example:

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

---

## 2. Mental Model

Imagine air bubbles rising in water.

The largest bubble always rises to the top.

Similarly, after every pass, the largest element moves to the end of the array.

---

## 3. Algorithm

For every pass,

1. Compare adjacent elements.
2. Swap them if they are in the wrong order.
3. Continue until the unsorted part ends.
4. The largest element reaches its correct position.

Repeat until the array is sorted.

---

## 4. Iterative Code

```java
public static void bubbleSort(int[] arr){
    int n = arr.length;
    for(int i=0;i<n-1;i++){
        boolean swapped = false;
        for(int j=0;j<n-i-1;j++){
            if(arr[j] > arr[j+1]){
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
                swapped = true;
            }
        }
        if(!swapped)
            break;
    }
}
```

---

## 5. Recursive Code

```java
public static void bubbleSort(int[] arr){
    helper(arr, arr.length);
}

private static void helper(int[] arr, int n){
    if(n == 1)
        return;
    boolean swapped = false;
    for(int i=0;i<n-1;i++){
        if(arr[i] > arr[i+1]){
            int temp = arr[i];
            arr[i] = arr[i+1];
            arr[i+1] = temp;
            swapped = true;
        }
    }
    if(!swapped)
        return;
    helper(arr, n-1);
}
```

---

## 6. How recursion replaces iteration

#### Iterative:

for every pass → compare adjacent elements → largest reaches end → next pass

#### Recursive:

helper(n) → one pass till n → largest reaches end → helper(n-1)

Instead of the loop reducing the unsorted size,
the recursive call reduces it.

---

## 7. Dry Run

Array: [5,1,4,2]

Pass 1

5 1 4 2

↓

1 5 4 2

↓

1 4 5 2

↓

1 4 2 5

Pass 2

↓

1 4 2 5

↓

1 2 4 5

Pass 3

No swaps

Array is already sorted.

---

## 8. Complexity

Best Case  : O(n)

Average    : O(n²)

Worst      : O(n²)

Extra Space: O(1)

Reason:

If no swaps occur during a pass,
the array is already sorted,
so the algorithm terminates early.

---

## 9. Stable?

✅ Yes

Reason:

Only adjacent elements are swapped.
Equal elements are never swapped because the condition is

```java
arr[j] > arr[j+1]
```
and not
```java
arr[j] >= arr[j+1]
```
Hence, their relative order is preserved.

---

## 10. In-place?

✅ Yes

Reason:

Only a temporary variable is used for swapping.
No extra array is created.

---

## 11. Adaptive?

✅ Yes

Reason:

Using the `swapped` flag,
if no swaps occur during a pass,
the algorithm stops immediately.

---

## 12. Number of Comparisons

Worst Case → (n-1)+(n-2)+...+1 = n(n-1)/2

Best Case (Optimized) → n-1

---

## 13. Number of Swaps

Best Case → 0

Worst Case → n(n-1)/2

(Reverse sorted array)

---

## 14. When is Bubble Sort useful?

- Small datasets.
- Detecting whether an array is already sorted.
- Educational purposes.
- Rarely used in real-world applications.

---

## 15. Key Takeaways

✅ Compares adjacent elements.

✅ Largest element reaches the end after every pass.

✅ Stable.

✅ In-place.

✅ Adaptive (using swapped flag).

✅ Early termination is possible.
