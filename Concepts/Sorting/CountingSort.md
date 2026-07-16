# Counting Sort

## 1. Idea

Counting Sort sorts elements by **counting the frequency** of each value.

Instead of comparing elements,

it counts how many times each number occurs,

then reconstructs the sorted array using those frequencies.

Example:

4 2 2 8 3 3 1

→ Count Frequencies

1 → 1

2 → 2

3 → 2

4 → 1

8 → 1

→ Reconstruct Array

1 2 2 3 3 4 8

Sorted.

---

## 2. Mental Model

Imagine counting the number of students in each class.
Instead of arranging every student individually,
you count how many students belong to each class,
then write the classes in order.

---

## 3. Algorithm

1. Find the maximum element.
2. Create a count array.
3. Count the frequency of every element.
4. Compute cumulative frequencies (for stable version).
5. Place each element into its correct position.
6. Copy the sorted array back.

---

## 4. Iterative Code

```java
public static void countingSort(int[] arr){

    int max = arr[0];

    for(int num : arr){
        max = Math.max(max, num);
    }

    int[] count = new int[max + 1];

    for(int num : arr){
        count[num]++;
    }

    for(int i = 1; i < count.length; i++){
        count[i] += count[i - 1];
    }

    int[] output = new int[arr.length];

    for(int i = arr.length - 1; i >= 0; i--){
        output[count[arr[i]] - 1] = arr[i];
        count[arr[i]]--;
    }

    for(int i = 0; i < arr.length; i++){
        arr[i] = output[i];
    }
}
```

---

## 5. Recursive Code

❌ Counting Sort is naturally iterative.

Recursive implementations are uncommon and offer no practical advantage.

---

## 6. How it works

Find Maximum

→ Count Frequencies

→ Compute Prefix Sum

→ Place Elements

→ Copy Back

→ Sorted Array

---

## 7. Dry Run

Array:

[4,2,2,8,3,3,1]

Count Array

1 → 1

2 → 2

3 → 2

4 → 1

5 → 0

6 → 0

7 → 0

8 → 1

Prefix Sum

→ [1,3,5,6,6,6,6,7]

Output

→ 1 2 2 3 3 4 8

Sorted.

---

## 8. Complexity

Best Case  : O(n + k)

Average    : O(n + k)

Worst      : O(n + k)

Extra Space: O(n + k)

where

n = Number of elements

k = Maximum element value

Reason:

Counting frequencies takes O(n).

Processing the count array takes O(k).

---

## 9. Stable?

✅ Yes

Reason:

Elements are placed from **right to left** using the prefix sum array.
This preserves the relative order of equal elements.

---

## 10. In-place?

❌ No

Reason:

An additional output array and count array are required.

---

## 11. Adaptive?

❌ No

Reason:

Even if the array is already sorted,
Counting Sort still counts every element and reconstructs the array.

---

## 12. Number of Comparisons

None. Counting Sort is a **non-comparison sorting algorithm**.

---

## 13. Space Requirement

Count Array

→ O(k)

Output Array

→ O(n)

Total

→ O(n + k)

---

## 14. When is Counting Sort useful?

- Small range of integers.
- Integer keys only.
- As a subroutine in Radix Sort.
- When O(n log n) comparison sorting is unnecessary.

---

## 15. Key Takeaways

✅ Non-comparison sorting algorithm.

✅ Uses frequency counting.

✅ Stable.

✅ Not In-place.

✅ Time Complexity = O(n + k).

✅ Works only for integers within a limited range.

---

## 16. Terminologies

**Count Array** → Stores the frequency of each value.

**Prefix Sum** → Running total of frequencies used to determine final positions.

**Output Array** → Stores the sorted result before copying back.

**k** → Maximum value (or value range) in the input.
