# 📘 Binary Search

![Topic](https://img.shields.io/badge/Topic-Binary%20Search-blue?style=for-the-badge)
![Language](https://img.shields.io/badge/Language-Java-orange?style=for-the-badge)
![Paradigm](https://img.shields.io/badge/Paradigm-Decrease%20%26%20Conquer-green?style=for-the-badge)

---

## 📖 About

**Binary Search** is an efficient searching algorithm used to find an element in a **sorted** array or search space.

Instead of checking every element one by one, Binary Search repeatedly halves the search space and continues searching in only the half that can contain the answer.

This reduces the search space by **50% after every comparison**, making it much faster than Linear Search.

---

## 🧠 How Binary Search Works

Maintain two pointers:

```text
left = 0
right = n - 1
```

Find the middle index:

```java
mid = left + (right - left) / 2;
```

Compare the middle element with the target.

- If equal → Answer found.
- If target < middle → Search the left half.
- If target > middle → Search the right half.

Continue until:

```text
left > right
```

---

## 🔍 Why Does It Work?

Since the array is **sorted**, one comparison tells us which half **cannot** contain the answer.

Therefore, we discard half of the search space after every step.

---

## 📈 Search Space Reduction

```text
n
↓

n/2
↓

n/4
↓

n/8
↓

...

↓

1
```

Hence,

```text
Time Complexity = O(log n)
```

---

## 🔄 Iterative Binary Search

Uses a loop to repeatedly reduce the search space.

### Time Complexity

```text
O(log n)
```

### Space Complexity

```text
O(1)
```

```java
class Solution {
    public int search(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while(left <= right){

            int mid = left + (right - left) / 2;

            if(nums[mid] == target)
                return mid;

            if(nums[mid] < target)
                left = mid + 1;

            else
                right = mid - 1;
        }

        return -1;
    }
}
```

---

## 🔄 Recursive Binary Search

Uses recursion to reduce the search space.

### Time Complexity

```text
O(log n)
```

### Space Complexity

```text
O(log n)
```

(Recursion Call Stack)

```java
class Solution {

    public int search(int[] nums, int target) {
        return binarySearch(nums, 0, nums.length - 1, target);
    }

    int binarySearch(int[] nums, int left, int right, int target){

        if(left > right)
            return -1;

        int mid = left + (right - left) / 2;

        if(nums[mid] == target)
            return mid;

        if(nums[mid] < target)
            return binarySearch(nums, mid + 1, right, target);

        return binarySearch(nums, left, mid - 1, target);
    }
}
```

---

## 🎯 When to Use Binary Search

Use Binary Search when:

- The array is sorted.
- The search space is sorted or monotonic.
- The answer can be determined by checking a condition.
- You need a faster alternative to Linear Search.

---

## 📚 Common Variations

- Classic Binary Search
- Lower Bound
- Upper Bound
- First & Last Occurrence
- Search Insert Position
- Rotated Sorted Array
- Peak Element
- Binary Search on Answer
- Search in 2D Matrix

---

## 📊 Complexity

| Approach | Time | Space |
|----------|------|-------|
| Iterative | **O(log n)** | **O(1)** |
| Recursive | **O(log n)** | **O(log n)** |

---

## 🎯 Key Takeaways

- Works only on a **sorted** search space.
- Eliminates **half of the remaining search space** every iteration.
- Follows the **Decrease and Conquer** paradigm.
- Prefer the iterative solution since it avoids recursion stack overhead.
- Always compute the middle index as:

```java
mid = left + (right - left) / 2;
```

to prevent integer overflow.
