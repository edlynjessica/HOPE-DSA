# BST Implementation

![Language](https://img.shields.io/badge/Language-Java-blueviolet?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Binary%20Search%20Tree-green?style=for-the-badge)

## Problem Statement

Implement a **Binary Search Tree (BST)** with the following operations:

1. **Insert** — Given an element, insert it into the BST at the correct position. If the element is equal to the data of the node, insert it in the **left subtree**.
2. **Delete** — Given an element, remove it from the BST. If the node has both children, replace it with the **minimum element from the right subtree**.
3. **Search** — Given an element, check whether it is present in the BST. Return `true` or `false`.
4. **Print Tree (Recursive)** — Print every node in the following format:

```text
N:L:x,R:y
```

where:

* `N` → data of the current node
* `x` → data of the left child
* `y` → data of the right child
* Print `L` and `R` only when the corresponding child is not `null`.
* There should be **no spaces** in the output.
* Each node should be printed on a separate line.

---

## Input Format

The first line contains the number of queries.

From the second line onwards, each query is given in the following format:

```text
1 data    → Insert
2 data    → Delete
3 data    → Search
4         → Print Tree
```

---

## Output Format

Print the result of each query on a separate line.

For a search query, print:

```text
true
```

or

```text
false
```

For a print query, print all nodes in the required recursive format.

---

## Example 1

### Input

```text
6
1 2
1 3
1 1
4
2 2
4
```

### Output

```text
2:L:1,R:3
1:
3:
3:L:1,
1:
```

---

## Example 2

### Input

```text
6
1 2
1 3
1 1
3 2
2 2
3 2
```

### Output

```text
true
false
```

# Approach

## 🌱 1. Insert

Use the BST property to decide where the new element should go.

```text
data < root.data  → go left
data > root.data  → go right
data == root.data → go left
```

The given implementation places duplicate values in the **left subtree**.

---

## 🌱 2. Delete

First, search for the node that needs to be deleted.

There are three cases:

### Case 1: No Children

The node is a leaf.

```text
return null
```

### Case 2: One Child

If the node has only one child, replace the node with that child.

```text
Left child only  → return left
Right child only → return right
```

### Case 3: Two Children

If the node has both children:

1. Find the **minimum element in the right subtree**.
2. Replace the current node's data with that value.
3. Delete the duplicate value from the right subtree.

```text
Current Node
     ↓
  [  5  ]
   /   \
  3     8
       /
      6

Minimum in right subtree = 6

      6
     / \
    3   8
       /
      6  ← delete this
```

---

## 🌱 3. Search

Use the BST property to eliminate half of the possible subtree in each step.

```text
data == root.data → found

data < root.data  → search left

data > root.data  → search right
```

---

## 🌱 4. Print Tree

Print the current node first, then recursively print its left and right subtrees.

```text
Root
 ↓
Print current node
 ↓
Print left subtree
 ↓
Print right subtree
```

This follows **Preorder Traversal**.

### Example

```text
      2
     / \
    1   3
```

Output:

```text
2:L:1,R:3
1:
3:
```

---

# ⭐ Key Observations

* BST property determines whether to move **left or right**.
* Duplicate values are inserted into the **left subtree** according to the problem statement.
* Deletion has **3 cases**: `0`, `1`, or `2` children.
* For a node with two children, use the **minimum value from the right subtree**.
* Search only needs to explore one path.
* `printTree()` uses **preorder traversal**.

---

# ⏱️ Complexity

Let `h` be the height of the BST.

| Operation  | Time     |
| ---------- | -------- |
| Insert     | **O(h)** |
| Delete     | **O(h)** |
| Search     | **O(h)** |
| Print Tree | **O(n)** |

For a balanced BST:

```text
h = O(log n)
```

For a completely skewed BST:

```text
h = O(n)
```

---

# 💻 Java Solution

```java
import java.io.*;
import java.util.*;


class Node{
        int data;
        Node left;
        Node right;
        Node(int data){
                this.data=data;
        }
}

public class Solution {
    
    static Node insert(Node root,int data){
        if(root==null){
                return new Node(data);
        }
        if(data<root.data){
                root.left=insert(root.left,data);
        }else if(data>root.data){
                root.right=insert(root.right,data);
        }
        return root;
    }
    
    static Node delete(Node root,int data){
        if(root==null) return null;
        
        // search for the data
        if(data<root.data){
                root.left=delete(root.left,data);
        }else if(data>root.data){
                root.right=delete(root.right,data);
        }
        else{ // node is found
                // 0 children 
                if(root.left==null && root.right==null) return null;
                // 1 children
                else if(root.left==null){
                        return root.right;
                }
                else if(root.right==null){
                        return root.left;
                }
                else{
                        Node temp=root.right;
                        while(temp.left!=null){
                                temp=temp.left;
                        }
                        root.data=temp.data;
                        root.right=delete(root.right,temp.data);
                }
        }
        return root;
    }
    
    static boolean search(Node root,int data){
        if(root==null) return false;
        if(root.data==data) return true;
        else if(data<root.data){
                return search(root.left, data);
        }else{
                return search(root.right,data);
        }
    }
    
    static void printTree(Node root){
        if(root==null) return;
        System.out.print(root.data+":");
        if(root.left!=null) System.out.print("L:"+root.left.data+",");
        if(root.right!=null) System.out.print("R:"+root.right.data);
        System.out.println();
        printTree(root.left);
        printTree(root.right);
    }
    
    public static void main(String[] args) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution. */
        Scanner sc=new Scanner(System.in);
        int n=sc.nextInt();
        Node root=null;
        int data;
        while(n-- >0){
                int ch=sc.nextInt();
                switch(ch){
                        case 1:
                                data=sc.nextInt();
                                root=insert(root,data);
                                break;
                        case 2:
                                data=sc.nextInt();
                                root=delete(root,data);
                                break;
                        case 3:
                                data=sc.nextInt();
                                System.out.println(search(root,data));
                                break;
                        case 4:
                                printTree(root);
                                break;
                }
        }
    }
}
```

# 📌 Key Takeaways

* 🌳 **BST:** `Left < Root < Right`
* ➕ **Insert:** Follow the BST property.
* ❌ **Delete:** Handle `0`, `1`, and `2` child cases.
* 🔎 **Search:** Follow only one path.
* 🔄 **Two-child deletion:** Replace with the **minimum of the right subtree**.
* 🖨️ **Print Tree:** Preorder traversal.
* ⚡ Balanced BST operations → **O(log n)**.
