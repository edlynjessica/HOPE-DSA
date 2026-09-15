# Binary Tree Construction in Level order

## Problem

Given `n` node values, construct a **Binary Tree** using the values in level order.

* The first value becomes the root.
* Each parent node must have **at most 2 children**.
* Fill the left child first, followed by the right child.
* Continue until all `n` nodes are inserted.

### Example

**Input**

```text
1 2 3 4 5 6 7
```

**Constructed Binary Tree**

```text
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
```

**Output**

```text
1
2 3
4 5 6 7
```

## Approach

Use a **Queue** to keep track of nodes that still need children.

1. If the tree is empty, make the new node the root and add it to the queue.
2. Take the front node using `peek()` as the current parent.
3. If its left child is empty, insert the new node as the left child.
4. Otherwise, insert it as the right child.
5. Once both children are filled, remove the parent using `poll()`.
6. Continue until all `n` nodes are inserted.

### Key Idea

The queue stores nodes that still have an available child position.

```text
Queue → [nodes waiting for children]
```

A node is removed from the queue only after its **right child is filled**.

### Java code
```java
import java.util.*;


class Node{
  Node left;
  Node right;
  int data;
  Node(int data){
    this.data=data;
  }
}

class Tree{
    Queue<Node> q=new LinkedList<>();
    Node root;

    void insert(int data){
      Node newNode=new Node(data);
      if(root==null){
        root=newNode;
        q.offer(root);
      }else{
        Node parent=q.peek();
        if(parent.left==null){
          parent.left=newNode;
          q.offer(newNode);
        }else{
          parent.right=newNode;
          q.offer(newNode);
          q.poll();
        }
      }
    }
    void print(){
      List<List<Integer>> ans=new ArrayList<>();
      Queue<Node> q=new LinkedList<>();
      q.offer(root);
      while(!q.isEmpty()){
        int size=q.size();
        List<Integer> lvl=new ArrayList<>();
        for(int i=0;i<size;i++){
          Node curr=q.poll();
          lvl.add(curr.data);
          if(curr.left!=null) q.offer(curr.left);
          if(curr.right!=null) q.offer(curr.right);
        }
        ans.add(lvl);
      }
      for(List<Integer> list:ans){
        for(int i:list){
          System.out.print(i+" ");
        }
        System.out.println();
      }
    }

}

public class Main {
    public static void main(String[] args) {
      Scanner sc=new Scanner(System.in);
      Tree obj=new Tree();
      while(sc.hasNext()){
        obj.insert(sc.nextInt());
      }
      obj.print();
    }
}
```

## Complexity

* **Time:** `O(n)` — each node is inserted once.
* **Space:** `O(n)` — the queue can contain up to `O(n)` nodes.
