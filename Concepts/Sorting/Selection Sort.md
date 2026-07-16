# Selection Sort

## 1. Idea

Selection Sort repeatedly finds the **smallest element** from the unsorted part of the array and places it at its correct position.

At every pass,

- Left side → Sorted
- Right side → Unsorted

Initially,

Sorted = Empty
Unsorted = Entire Array

Example:

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

---

## 2. Mental Model

Imagine arranging students by height.
Instead of swapping every time you see a shorter student, you first walk through the entire line,
find the shortest student,and only then place them at the front.
Repeat for the remaining students.

---

## 3. Algorithm

For every position i,

1. Assume current element is the minimum.
2. Search the remaining array.
3. Find the actual minimum.
4. Swap it with index i.

Repeat until only one element remains.

---

## 4. Iterative Code

```java
public static void selectionSort(int[] arr){
    int n = arr.length;
    for(int i=0;i<n-1;i++){
        int minIndex = i;
        for(int j=i+1;j<n;j++){
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

## 5. Recursive Code

```java
public static void selectionSort(int[] arr){
    helper(arr,0);
}
private static void helper(int[] arr,int start){
    if(start == arr.length-1)
        return;
    int minIndex = start;
    for(int i=start+1;i<arr.length;i++){
        if(arr[i] < arr[minIndex]){
            minIndex = i;
        }

    }

    int temp = arr[start];
    arr[start] = arr[minIndex];
    arr[minIndex] = temp;
    helper(arr,start+1);
}
```

---

## 6. How recursion replaces iteration

#### Iterative:

for every i → find minimum → swap → next i

#### Recursive:

helper(start) → find minimum → swap → helper(start+1)

Instead of the loop changing start,
the recursive call changes start.

---

## 7. Dry Run

Array: [29,10,14,37,13]

Pass 1
Minimum = 10

Swap with 29

10 29 14 37 13

Pass 2
Minimum = 13

Swap with 29

10 13 14 37 29

Pass 3
Minimum = 14

Already correct

10 13 14 37 29

Pass 4
Minimum = 29

Swap

10 13 14 29 37

Sorted.

---

## 8. Complexity

Best Case  : O(n²)

Average    : O(n²)

Worst      : O(n²)

Extra Space: O(1)

Reason:

Even if the array is already sorted,
Selection Sort still searches the entire unsorted part every pass.

---

## 9. Stable?

❌ No

Reason:

The minimum element may jump ahead of equal elements,
changing their original order.

---

## 10. In-place?

✅ Yes

Reason:

Only a few variables are used.
No extra array is created.

---

## 11. Adaptive?

❌ No

Reason:

Already sorted arrays still require all comparisons.

---

## 12. Number of Comparisons

Always

(n-1)+(n-2)+...+1  =  n(n-1)/2

Comparisons are fixed.

---

## 13. Number of Swaps

Maximum = n-1
One swap per pass.
This is much fewer than Bubble Sort.

---

## 14. When is Selection Sort useful?

- Memory is limited.
- Swapping is expensive.
- Small datasets.

---

## 15. Key Takeaways

✅ Finds minimum every pass.

✅ Swaps only once per pass.

✅ Comparisons never reduce.

✅ Not stable.

✅ In-place.

✅ Not adaptive.
