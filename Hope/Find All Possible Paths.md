# Find All Possible Paths — Backtracking

A variation of the **Unique Paths** problem from LeetCode.

Instead of finding only the **number of possible paths**, this problem requires us to **generate and store every possible path** from the top-left corner to the bottom-right corner of an `r × c` grid.

## Problem

Given an `r × c` grid, start at `(0, 0)` and reach `(r-1, c-1)`.

At every cell, you can move in only two directions:

* `h` → move horizontally/right
* `v` → move vertically/down

Find **all possible paths** from the start to the destination.

### Example

For a `2 × 3` grid:

```text
Start → → 
        ↓
        → Destination
```

The possible paths are:

```text
[h, h, v]
[h, v, h]
[v, h, h]
```

So the output is:

```text
[[h, h, v], [h, v, h], [v, h, h]]
```

## Approach

This solution uses **backtracking**.

We maintain a temporary `ArrayList<Character>` called `b` that represents the path currently being explored.

At every cell, we have two choices:

```text
             Current
             /     \
          'h'       'v'
           /         \
       right         down
```

For each choice:

1. Add the move to the current path.
2. Recursively explore that path.
3. Remove the move after returning from recursion.

This is the important backtracking step:

```java
b.add('h');
backtrack(a, i, j + 1, b);
b.remove(b.size() - 1);

b.add('v');
backtrack(a, i + 1, j, b);
b.remove(b.size() - 1);
```

The `remove()` operation restores the previous state before trying the next possibility.

## Why `new ArrayList<>(b)`?

At the destination, we store the current path:

```java
a.add(new ArrayList<>(b));
```

We create a **copy** of `b` because `b` is a mutable `ArrayList`.

If we directly stored `b`:

```java
a.add(b); // ❌
```

all entries in `a` would refer to the same list. Since backtracking keeps modifying `b`, the stored paths would also change.

Using:

```java
new ArrayList<>(b)
```

creates an independent copy of the current path.

## Base Cases

### Out of bounds

```java
if(i == r || j == c) return;
```

If we move outside the grid, that path is invalid.

### Destination

```java
if(i == r-1 && j == c-1) {
    a.add(new ArrayList<>(b));
    return;
}
```

When we reach the bottom-right cell, the current path is complete, so we store it.

## Complete Code

```java
import java.util.*;

public class Main {

    static int r;
    static int c;

    public static void backtrack(
            ArrayList<ArrayList<Character>> a,
            int i,
            int j,
            ArrayList<Character> b) {

        if (i == r || j == c)
            return;

        if (i == r - 1 && j == c - 1) {
            a.add(new ArrayList<>(b));
            return;
        }

        // Horizontal move
        b.add('h');
        backtrack(a, i, j + 1, b);
        b.remove(b.size() - 1);

        // Vertical move
        b.add('v');
        backtrack(a, i + 1, j, b);
        b.remove(b.size() - 1);
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        r = sc.nextInt();
        c = sc.nextInt();

        ArrayList<ArrayList<Character>> a = new ArrayList<>();
        ArrayList<Character> b = new ArrayList<>();

        backtrack(a, 0, 0, b);

        System.out.println(a);
    }
}
```

## Complexity

To reach the destination, we need exactly:

* `r - 1` vertical moves
* `c - 1` horizontal moves

Therefore, the number of possible paths is:

```text
C((r-1) + (c-1), r-1)
```

Since we are **generating every path**, the time complexity is proportional to the total size of all generated paths.

The space complexity includes:

* The recursion stack
* The current path
* The collection containing all generated paths

## Key Takeaway

This problem is useful for understanding an important backtracking pattern:

```text
choose
→ explore
→ undo
```

And when working with mutable objects such as `ArrayList`, remember:

> **Store a copy when you need to preserve the current state.**
