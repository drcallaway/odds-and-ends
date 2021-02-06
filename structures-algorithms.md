# Structures and Algorithms

Occasionally, I'll implement a data structure or algorithm as a simple programming exercise. Here are a few of the small programs I've written that demonstrate basic concepts in computer science. These programs include:

* Simple stack implementation
* Simple queue implementation
* Binary tree traversal

This code may be used, altered, and distributed without restriction. Of course, it comes without warranty of any kind.

## Simple Stack Implementation
```java
/**
 * Stack implementation.
 *
 * @author Dustin R. Callaway
 * @version January 19, 2006
 */
public class Stack
{
    private Node head;
   
    public void push(Object dataItem)
    {
        Node node = new Node(dataItem, head);
       
        head = node;
    }
   
    public Object pop()
    {
        if (head == null)
            return null;
       
        Object dataItem = head.dataItem;
        head = head.next;
       
        return dataItem;
    }
   
    private class Node
    {
        private Object dataItem;
        private Node next;
       
        private Node(Object dataItem, Node next)
        {
            this.dataItem = dataItem;
            this.next = next;
        }
    }
}
```

## Simple Queue Implementation
```java
/**
 * Queue implementation.
 *
 * @author Dustin R. Callaway
 * @version January 19, 2006
 */
public class Queue
{
    private Node head;
    private Node tail;
    
    public void add(Object dataItem)
    {
        Node node = new Node(dataItem, null);
        
        if (head == null)
        {
            head = node;
            tail = node;
        }
        else if (head.next == null)
        {
            head.next = node;
            tail = node;
        }
        else
        {
            tail.next = node;
            tail = node;
        }
    }
    
    public Object get()
    {
        if (head == null)
            return null;
        
        Object dataItem = head.dataItem;
        head = head.next;
        
        if (head == null)
            tail = null;
        
        return dataItem;
    }
    
    private class Node
    {
        Object dataItem;
        Node next;
        
        private Node(Object dataItem, Node next)
        {
            this.dataItem = dataItem;
            this.next = next;
        }
    }
}
```

## Binary Search Tree Traversal Algorithm
```java
/**
 * Binary search tree traversal.
 *
 * @author Dustin R. Callaway
 * @version January 21, 2006
 */
public class BinaryTreeTraversal
{
    public static void main(String[] args)
    {
        //construct binary search tree
        Node root = new Node(100);
        root.left = new Node(50);
        root.right = new Node(150);
        root.left.left = new Node(25);
        root.left.right = new Node(75);
        root.right.left = new Node(125);
        root.right.right = new Node(175);
        root.right.left.left = new Node(110);
        
        inorderTraversal(root);
    }

    public static void inorderTraversal(Node node)
    {
        //for reverse order traversal, traverse right child before left
        if (node.left != null)
        {
            inorderTraversal(node.left);
        }

        System.out.println(node.dataItem);

        if (node.right != null)
        {
            inorderTraversal(node.right);
        }
    }

    private static class Node
    {
        private int dataItem;
        private Node left;
        private Node right;
        
        private Node(int dataItem)
        {
            this.dataItem = dataItem;
        }
    }
}
```
