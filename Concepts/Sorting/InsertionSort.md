# Insertion Sort

## 1. Idea

Insertion Sort builds the sorted array **one element at a time**.

At every pass,

- Left side → Sorted
- Right side → Unsorted

Each new element is picked from the unsorted part and inserted into its correct position in the sorted part.

Initially,

Sorted = First Element

Unsorted = Remaining Elements

Example:

64 | 25 12 22 11

↓

25 64 | 12 22 11

↓

12 25 64 | 22 11

↓

12 22 25 64 | 11

↓

11 12 22 25 64

---

## 2. Mental Model

Imagine arranging playing cards in your hand.
You pick one new card at a time and insert it into its correct position among the cards already arranged.

---

## 3. Algorithm

For every element starting from index 1,

1. Store the current element (key).
2. Compare it with elements on its left.
3. Shift all larger elements one position to the right.
4. Insert the key into its correct position.

Repeat until the array is sorted.

---

## 4. Iterative Code

```java
public static void insertionSort(int[] arr){
    int n = arr.length;
    for(int i=1;i<n;i++){
        int key = arr[i];
        int j = i-1;
        while(j>=0 && arr[j] > key){
            arr[j+1] = arr[j];
            j--;
        }
        arr[j+1] = key;
    }
}
```

---

## 5. Recursive Code

```java
public static void insertionSort(int[] arr){
    helper(arr,1);
}
private static void helper(int[] arr,int index){
    if(index == arr.length)
        return;
    int key = arr[index];
    int j = index-1;
    while(j>=0 && arr[j] > key){
        arr[j+1] = arr[j];
        j--;
    }
    arr[j+1] = key;
    helper(arr,index+1);
}
```

---

## 6. How recursion replaces iteration

#### Iterative:

for every index → pick key → shift larger elements → insert key → next index

#### Recursive:

helper(index) → insert current element → helper(index+1)

Instead of the loop increasing the index,
the recursive call increases it.

---

## 7. Dry Run

Array: [5,2,4,6,1]

Pass 1 | Key = 2

5 shifts right

2 5 4 6 1


Pass 2 | Key = 4

5 shifts right

2 4 5 6 1


Pass 3 | Key = 6

Already correct

2 4 5 6 1


Pass 4 | Key = 1

6,5,4,2 shift right

1 2 4 5 6

Sorted.

---

## 8. Complexity

Best Case  : O(n)

Average    : O(n²)

Worst      : O(n²)

Extra Space: O(1)

Reason:

If the array is already sorted,
only one comparison is needed per pass.
In the worst case (reverse sorted),
every element shifts through the sorted portion.

---

## 9. Stable?

✅ Yes

Reason:

Only elements **greater than** the key are shifted.
Equal elements are never shifted past each other.
Their relative order remains unchanged.

---

## 10. In-place?

✅ Yes

Reason:

Only a few variables are used.
No extra array is created.

---

## 11. Adaptive?

✅ Yes

Reason:

Already sorted arrays require very few comparisons and no shifting.
Hence, the algorithm performs in O(n).

---

## 12. Number of Comparisons

Best Case n-1

Worst Case

n(n-1)/2

---

## 13. Number of Shifts

Best Case -→ 0

Worst Case → n(n-1)/2

(Reverse sorted array)

---

## 14. When is Insertion Sort useful?

- Small datasets.
- Nearly sorted arrays.
- Online sorting (elements arrive one by one).
- Used inside advanced algorithms like TimSort.

---

## 15. Key Takeaways

✅ Builds the sorted portion one element at a time.

✅ Inserts each element into its correct position.

✅ Stable.

✅ In-place.

✅ Adaptive.

✅ Very efficient for nearly sorted arrays.
