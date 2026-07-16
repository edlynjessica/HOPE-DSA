# Cycle Sort

## 1. Idea

Cycle Sort places every element directly into its correct position.

Instead of repeatedly swapping adjacent elements,

it finds the correct index of an element and places it there.

This process forms a **cycle** of misplaced elements.

The cycle continues until every element reaches its correct position.

Example:

20 40 50 10 30

→ 10 40 50 20 30

→ 10 20 50 40 30

→ 10 20 30 40 50

Sorted.

---

## 2. Mental Model

Imagine five students standing in the wrong places.
Instead of exchanging neighbors one by one,
each student directly moves to their correct position,
causing another student to move, forming a cycle.

---

## 3. Algorithm

For every position,

1. Find the correct position of the current element.
2. If it is already correct, move to the next element.
3. Otherwise, swap it into its correct position.
4. Continue until the cycle completes.

Repeat for every position.

---

## 4. Iterative Code

```java
public static void cycleSort(int[] arr){

    int n = arr.length;

    for(int cycleStart = 0; cycleStart < n - 1; cycleStart++){

        int item = arr[cycleStart];
        int pos = cycleStart;

        for(int i = cycleStart + 1; i < n; i++){
            if(arr[i] < item)
                pos++;
        }

        if(pos == cycleStart) continue;

        while(item == arr[pos])  pos++;

        int temp = item;
        item = arr[pos];
        arr[pos] = temp;

        while(pos != cycleStart){

            pos = cycleStart;

            for(int i = cycleStart + 1; i < n; i++){
                if(arr[i] < item)
                    pos++;
            }

            while(item == arr[pos]) pos++;

            temp = item;
            item = arr[pos];
            arr[pos] = temp;
        }
    }
}
```

---

## 5. Recursive Code

❌ Cycle Sort is naturally an iterative algorithm.

A recursive implementation is possible but rarely used because
it increases complexity without improving performance.

Hence, the iterative implementation is preferred.

---

## 6. How it works

Current Element

→ Find Correct Position

→ Place it there

→ Pick displaced element

→ Find its Correct Position

→ Continue until the cycle ends

---

## 7. Dry Run

Array:

[3,5,2,1,4]

Cycle 1

3 belongs at index 2

→ 2 5 3 1 4

2 belongs at index 1

→ 5 2 3 1 4

5 belongs at index 4

→ 4 2 3 1 5

4 belongs at index 3

→ 1 2 3 4 5

Sorted.

---

## 8. Complexity

Best Case  : O(n²)

Average    : O(n²)

Worst      : O(n²)

Extra Space: O(1)

Reason:

For every element,

its correct position may require scanning the remaining array.

---

## 9. Stable?

❌ No

Reason:

Elements may jump directly to distant positions,
changing the relative order of equal elements.

---

## 10. In-place?

✅ Yes

Reason:

Only a few variables are used.
No extra array is required.

---

## 11. Adaptive?

❌ No

Reason:

Even if the array is already sorted,
the algorithm still checks every element.

---

## 12. Number of Comparisons

Worst Case : O(n²)

Every element may need to search the remaining array.

---

## 13. Number of Writes

**Minimum among all comparison-based sorting algorithms.**

Each element is written at most once into its final position.

This is the *biggest advantage of Cycle Sort.*

---

## 14. When is Cycle Sort useful?

- When memory writes are very expensive.
- Flash memory / EEPROM.
- Embedded systems.
- Situations where minimizing writes is more important than execution time.

---

## 15. Key Takeaways

✅ Places every element directly into its correct position.

✅ Forms cycles of misplaced elements.

✅ In-place.

✅ Minimizes the number of writes.

✅ Not Stable.

✅ Time Complexity = O(n²).

---

## 16. Terminologies

**Cycle** → A sequence of elements that rotate into each other's correct positions.

**Cycle Start** → The first element of a cycle.

**Correct Position** → The index where an element should finally be placed.

**Writes** → Number of times elements are written back into the array.
