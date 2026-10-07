# LinkedIn Queens — Backtracking

## Problem

Given an `n × n` board divided into different **regions**, represented by integers, place exactly **one queen in every row, every column, and every region**.

A queen also **cannot be diagonally adjacent** to another queen.

### Rules

For every valid solution:

1. Exactly one queen in each **row**.
2. Exactly one queen in each **column**.
3. Exactly one queen in each **region**.
4. Two queens cannot be **diagonally adjacent**.
5. Print the **first valid solution**.
6. If no solution exists, print `NO`.

---

## Example

### Input

```text
5
1 2 3 4 5
2 3 4 5 1
3 4 5 1 2
4 5 1 2 3
5 1 2 3 4
```

### Output

```text
YES
Q....
..Q..
....Q
.Q...
...Q.
```

### Why is this valid?

Queen positions are:

```text
(0,0)
(1,2)
(2,4)
(3,1)
(4,3)
```

### Row check

There is exactly one queen in every row.

```text
Q....
..Q..
....Q
.Q...
...Q.
```

### Column check

Queen columns are:

```text
0, 2, 4, 1, 3
```

All columns are different.

### Region check

Using the input matrix:

```text
1 2 3 4 5
2 3 4 5 1
3 4 5 1 2
4 5 1 2 3
5 1 2 3 4
```

The queens are placed in regions:

```text
(0,0) → 1
(1,2) → 4
(2,4) → 2
(3,1) → 5
(4,3) → 3
```

So every region contains exactly one queen.

### Diagonal check

LinkedIn Queens only prevents **diagonally adjacent** queens.

For example:

```text
Q . .
. . Q
```

is allowed because the queens are not touching diagonally.

But:

```text
Q .
. Q
```

is invalid.

Because we place queens row-by-row, when checking `(row, col)` we only need to inspect the two diagonal cells in the **previous row**:

```text
↖   ↗
  Q
```

---

# Approach — Backtracking

We place exactly one queen in each row.

For every row:

```text
Try every column
      ↓
Is this position valid?
      ↓
Place queen
      ↓
Recursively solve next row
      ↓
If it fails → remove queen and try next column
```

### Step 1 — Start from row 0

```java
backtrack(0)
```

We try every column in row `0`.

---

### Step 2 — Check whether a position is valid

For a candidate `(row, col)`, we check:

#### 1. Row

```java
for(int j=0;j<n;j++){
    if(board[row][j]=='Q') return false;
}
```

Technically, because we process one row at a time, this check is redundant. The recursion itself guarantees one queen per row.

---

#### 2. Column

```java
for(int i=0;i<n;i++){
    if(board[i][col]=='Q') return false;
}
```

This prevents two queens from occupying the same column.

---

#### 3. Diagonally adjacent cells

Only the previous row needs to be checked:

```java
if(row-1>=0 && col-1>=0 && board[row-1][col-1]=='Q')
    return false;

if(row-1>=0 && col+1<n && board[row-1][col+1]=='Q')
    return false;
```

We don't check the entire diagonal because LinkedIn Queens only prohibits **touching diagonally**.

---

#### 4. Region

```java
if(!regionCheck(row,col,region))
    return false;
```

`regionCheck()` scans the board and makes sure that no previously placed queen belongs to the same region.

```java
if(board[i][j]=='Q' && grid[i][j]==region)
    return false;
```

---

# Backtracking

If the position is valid:

```java
board[row][col]='Q';
```

Then solve the next row:

```java
if(backtrack(row+1)) return true;
```

The important part is that we immediately return `true`.

This means we **stop as soon as the first solution is found**.

If the recursive call fails:

```java
board[row][col]='.';
```

We undo the choice and try another column.

This is the **backtracking** step.

---

# Base Case

```java
if(row==n){
    return true;
}
```

If we reached `row == n`, all `n` rows have successfully received a queen.

Therefore, a valid solution has been found.

---

# Early Region Check

Before starting backtracking:

```java
if(regions.size()<n){
    System.out.println("NO");
    return;
}
```

We need exactly `n` queens and every queen must belong to a different region.

Therefore, if there are fewer than `n` regions, a solution is impossible.

---

# Complete Java Code

```java
import java.io.*;
import java.util.*;

public class Solution {

    static int[][] grid;       // input region matrix
    static char[][] board;     // puzzle board

    static int n;              // board size
    static Set<Integer> regions;

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        n = sc.nextInt();

        grid = new int[n][n];
        regions = new HashSet<>();

        // Read input
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){
                grid[i][j] = sc.nextInt();
                regions.add(grid[i][j]);
            }
        }

        // Need at least n different regions
        if(regions.size() < n){
            System.out.println("NO");
            return;
        }

        // Initialize board
        board = new char[n][n];

        for(int i = 0; i < n; i++){
            Arrays.fill(board[i], '.');
        }

        // Try to find a solution
        if(backtrack(0)){
            print(board);
        }else{
            System.out.println("NO");
        }
    }

    // Print the first solution
    public static void print(char[][] board){

        System.out.println("YES");

        for(char[] ch : board){
            System.out.println(new String(ch));
        }
    }

    // Backtracking
    public static boolean backtrack(int row){

        // All rows successfully filled
        if(row == n){
            return true;
        }

        // Try every column in this row
        for(int col = 0; col < n; col++){

            if(isValid(row, col, grid[row][col])){

                // Place queen
                board[row][col] = 'Q';

                // Try next row
                if(backtrack(row + 1)){
                    return true;
                }

                // Undo choice
                board[row][col] = '.';
            }
        }

        // No valid position in this row
        return false;
    }

    // Check whether a queen can be placed
    public static boolean isValid(int row, int col, int region) {

        // Check row
        for (int j = 0; j < n; j++) {
            if (board[row][j] == 'Q') {
                return false;
            }
        }

        // Check column
        for (int i = 0; i < n; i++) {
            if (board[i][col] == 'Q') {
                return false;
            }
        }

        // Check upper-left diagonal
        if (row - 1 >= 0 &&
            col - 1 >= 0 &&
            board[row - 1][col - 1] == 'Q') {
            return false;
        }

        // Check upper-right diagonal
        if (row - 1 >= 0 &&
            col + 1 < n &&
            board[row - 1][col + 1] == 'Q') {
            return false;
        }

        // Check region
        if (!regionCheck(row, col, region)) {
            return false;
        }

        return true;
    }

    // Check whether another queen
    // already exists in this region.
    public static boolean regionCheck(int row, int col, int region) {

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {

                if (board[i][j] == 'Q' &&
                    grid[i][j] == region) {
                    return false;
                }
            }
        }

        return true;
    }
}
```



