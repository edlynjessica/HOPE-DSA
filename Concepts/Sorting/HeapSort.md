# Heap Sort

## 1. Idea

Heap Sort uses the **Binary Heap** data structure to sort elements.

For ascending order,

- Build a **Max Heap**.
- The largest element will be at the root.
- Swap the root with the last element.
- Reduce the heap size.
- Heapify the root again.

Repeat until only one element remains.

Example:

4 10 3 5 1

→ Build Max Heap

10 5 3 4 1

→ Swap Root & Last

1 5 3 4 | 10

→ Heapify

5 4 3 1 | 10

→ Swap

1 4 3 | 5 10

→ Heapify

4 1 3 | 5 10

→ Swap

3 1 | 4 5 10

→ Heapify

3 1 | 4 5 10

→ Swap

1 | 3 4 5 10

Sorted.

---

## 2. Mental Model

Imagine a tournament.

The strongest player always reaches the top.
Remove the winner,
conduct the tournament again,
and repeat until everyone is ranked.

---

## 3. Algorithm

1. Build a Max Heap.
2. Swap the root with the last element.
3. Reduce the heap size.
4. Heapify the root.
5. Repeat until the heap size becomes 1.

---

## 4. Iterative Code

```java
public static void heapSort(int[] arr){

    int n = arr.length;

    for(int i=n/2-1;i>=0;i--){
        heapify(arr,n,i);
    }

    for(int i=n-1;i>0;i--){

        int temp=arr[0];
        arr[0]=arr[i];
        arr[i]=temp;

        heapify(arr,i,0);
    }
}

private static void heapify(int[] arr,int n,int i){

    while(true){

        int largest=i;
        int left=2*i+1;
        int right=2*i+2;

        if(left<n && arr[left]>arr[largest])
            largest=left;

        if(right<n && arr[right]>arr[largest])
            largest=right;

        if(largest==i)
            break;

        int temp=arr[i];
        arr[i]=arr[largest];
        arr[largest]=temp;

        i=largest;

    }
}
```

---

## 5. Recursive Code

```java
public static void heapSort(int[] arr){

    int n=arr.length;

    for(int i=n/2-1;i>=0;i--){
        heapify(arr,n,i);
    }

    for(int i=n-1;i>0;i--){

        int temp=arr[0];
        arr[0]=arr[i];
        arr[i]=temp;

        heapify(arr,i,0);
    }

}

private static void heapify(int[] arr,int n,int i){

    int largest=i;
    int left=2*i+1;
    int right=2*i+2;

    if(left<n && arr[left]>arr[largest])  largest=left;

    if(right<n && arr[right]>arr[largest])  largest=right;

    if(largest!=i){

        int temp=arr[i];
        arr[i]=arr[largest];
        arr[largest]=temp;

        heapify(arr,n,largest);
    }
}
```

---

## 6. How recursion replaces iteration

#### Iterative

Build Heap

→ Swap Root

→ Heapify using loop

→ Repeat

#### Recursive

Build Heap

→ Swap Root

→ Heapify using recursion

→ Repeat

Instead of repeatedly moving down the heap using a loop,

the recursive version calls heapify() on the affected child.

---

## 7. Dry Run

Array:

[4,10,3,5,1]

Build Max Heap

→ 10 5 3 4 1

Swap Root

→ 1 5 3 4 | 10

Heapify

→ 5 4 3 1 | 10

Swap

→ 1 4 3 | 5 10

Heapify

→ 4 1 3 | 5 10

Swap

→ 3 1 | 4 5 10

Swap

→ 1 | 3 4 5 10

Sorted.

---

## 8. Complexity

Best Case  : O(n log n)

Average    : O(n log n)

Worst      : O(n log n)

Extra Space:

Iterative : O(1)

Recursive : O(log n)

Reason:

Building the heap takes O(n).
Each extraction requires O(log n).
Total : O(n) + O(n log n) = **O(n log n)**

---

## 9. Stable?

❌ No

Reason:

Swapping the root with the last element may change the relative order of equal elements.

---

## 10. In-place?

✅ Yes

Reason:

Sorting is done within the original array.
No extra array is required.

---

## 11. Adaptive?

❌ No

Reason:

Even if the array is already sorted,
Heap Sort still builds the heap and performs all extractions.

---

## 12. Number of Comparisons

Average : O(n log n)

Worst : O(n log n)

---

## 13. Number of Swaps

Approximately : O(n log n)

Each extracted element may require multiple swaps during heapify.

---

## 14. When is Heap Sort useful?

- Guaranteed O(n log n) performance.
- Memory-constrained systems.
- Priority Queue implementation.
- When worst-case performance matters.

---

## 15. Key Takeaways

✅ Uses Binary Heap.

✅ Root always contains the maximum element.

✅ In-place.

✅ Guaranteed O(n log n).

✅ Not Stable.

✅ Preferred when worst-case guarantees are important.

---

## 16. Terminologies

**Heap** → A Complete Binary Tree satisfying the Heap Property.

**Max Heap** → Parent ≥ Children.

**Min Heap** → Parent ≤ Children.

**Heapify** → Restore the heap property.

**Build Heap** → Convert an array into a valid heap.

**Root** → The first element of the heap (index 0).

**Leaf Nodes** → Nodes with no children.

**Complete Binary Tree** → Every level is completely filled except possibly the last, which is filled from left to right.

---

## 17. Heap Index Formulae

For a node at index `i`:

Left Child  → `2*i + 1`

Right Child → `2*i + 2`

Parent      → `(i-1)/2`
