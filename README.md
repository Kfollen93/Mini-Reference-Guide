# Mini-Reference-Guide :bookmark_tabs:
This repository represents a mini reference guide of topics I either search for often throughout my studies, topics that I needed a clearer explanation on, and/or topics that I felt were just generally important to know. This mini guide is intended to be used as a central location for the reasons listed above; it is not intended to be a full reference guide. <br>
<br>
Since I have found it is best to learn by doing, this mini guide uses C# code examples that I have created myself in a way that make sense to me.
After shuffling through my notes and erasing/re-writing these topics numerous times on my whiteboard, I figured it would be worth making this public and sharing with others. <br>

Therefore, if you found any of this useful, please consider giving it a star! :star: <br>

# Table of Contents  
* Big O Notation :o:
* Generic Collections :wrench:
* Object Oriented Programming          --> <i>In progress</i> :hammer:
* Common Sorting Algorithms            --> <i>In progress</i> :hammer:
* Binary Trees and Binary Search Trees :evergreen_tree:

## Big O Notation
<details>
  <summary><b>Big O Complexity Chart</b></summary>
<br>
<img src=https://github.com/Kfollen93/Mini-Reference-Guide/blob/main/Images/BigOChart.png alt="Big O Complexity Chart">
</details>

<details>
  <summary><b>O(1)</b></summary>
  O(1) is represented as "Constant Time". This means that regardless of the amount of data that is involved, whether it's 10,000 or 10, it will always require the same amount of time. <br>
  <br>
  An example of O(1) is:

  ```cs
  private static int ReturnFirstElementInArray(int[] arr)
  {
      return arr[0];
  }
  ```
</details>

<details>
  <summary><b>O(log n)</b></summary>
  O(log n) is represented as "Logarithmic Time". This means that the time will increase linearly while n increases exponentially. It is most commonly seen with divide and conquer algorithms. You can think of it as when the input is being divided in half with each iteration, it is logarithmic time. <br>
  <br>
  An example of O(log n) is:

  ```cs
  // Binary Search in a sorted array
  private static int SearchForTargetIndex(int[] arr, int target)
  {
      int leftPointer = 0;
      int rightPointer = arr.Length - 1;
    
      while (leftPointer <= rightPointer)
      {
          int mid = (leftPointer + rightPointer) / 2;

          if (arr[mid] == target)
          {
              return mid;
          }
          else if (target < arr[mid])
          {
              rightPointer = mid - 1;
          }
          else
          {
              leftPointer = mid + 1;
          }
      }
      return false; // Target does not exist.
  }
  ```
  It is worth noting that in the best case here, the time complexity could actually be O(1) if the mid point matches the target at the start. However, it is typical to measure time complexity based upon the worst case scenario, which in this case would be O(log n).
</details>

<details>
  <summary><b>O(n)</b></summary>
  O(n) is represented as "Linear Time". This means that the amount of time it will take is directly proportional to the number (<b><i>n</i></b>) of elements. The larger the amount of data that is involved, the longer it will take to complete. <br>
  <br>
  An example of O(n) is:

  ```cs
  private static int SearchForTargetIndex(int[] arr, int target)
  {
      for (int i = 0; i < arr.Length; i++)
      {
          if (arr[i] == target)
          {
              return i;
          }
      }
  }
  ```
</details>
  
<details>
  <summary><b>O(n log n)</b></summary>
  O(n log n) is represented as "Linearithmic Time". I find it is easiest to understand this as an algorithm that performs O(log n) operations <b><i> n </i></b> times. <br>
  <br>
  An example of O(n log n) is:

  ```cs
private static int[] MergeSort(int[] nums)
{
      if (nums.Length <= 1) return nums; // Array is already sorted
      int[] left;
      int[] right;
      int[] sorted = new int[nums.Length];

      int mid = nums.Length / 2;

      left = new int[mid]; // Sets the size of left

      // if the array is even
      if (nums.Length % 2 == 0)
      {
          right = new int[mid];
      }
      else // if the array is odd, then add one extra element to the right array
      {
          right = new int[mid + 1];
      }

      // Populating the left array -> Going from 0 to the mid point.
      for (int i = 0; i < mid; i++)
      {
          left[i] = nums[i];
      }

      // Populating the right array -> Going from the mid point to the end of the array.
      int j = 0;
      for (int i = mid; i < nums.Length; i++)
      {
          right[j] = nums[i];
          j++;
      }

      // Use recursion to sort the arrays
      left = MergeSort(left);
      right = MergeSort(right);

      // Merge arrays - Call the Merge function
      sorted = Merge(left, right);

      return sorted;
}

public static int[] Merge(int[] left, int[] right)
{
      // Set the size of the sorted array
      int sortedLength = left.Length + right.Length;
      int[] sorted = new int[sortedLength];

      int leftIndex = 0;
      int rightIndex = 0;
      int indexSorted = 0;

      // While there's always at least one element in either array
      while (leftIndex < left.Length || rightIndex < right.Length)
      {
        // If there's at least one element in BOTH arrays
        if (leftIndex < left.Length && rightIndex < right.Length)
        {
            if (left[leftIndex] <= right[rightIndex])
            {
                sorted[indexSorted] = left[leftIndex];
                leftIndex++;
                indexSorted++;
            }
            else
            {
                sorted[indexSorted] = right[rightIndex];
                rightIndex++;
                indexSorted++;
            }
        }
        // If only the left array has elements
        else if (leftIndex < left.Length)
        {
            sorted[indexSorted] = left[leftIndex];
            leftIndex++;
            indexSorted++;
        }
        // If only the right array has elements
        else if (rightIndex < right.Length)
        {
            sorted[indexSorted] = right[rightIndex];
            rightIndex++;
            indexSorted++;
        }
    }
    return sorted;
}
  ```
</details>
  
<details>
  <summary><b>O(n<sup>2</sup></b>)</summary>
  O(n<sup>2</sup>) is represented as "Quadratic Time". You will most typically see this with nested loops (although do not fall victim to thinking if it is a nested loop that it is automatically Quadratic Time; this is not how you calculate time complexity). If you are looping through the outer array <b><i> n </i></b> times, then the inner loop will also need to run <b><i> n </i></b> times during each iteration of the outer loop.  <br>
  <br>
  An example of O(n<sup>2</sup></b>) is:

  ```cs
  private static void SortArrayInPlace(int[] arr)
  {
      int temp;
  
      for (int i = 0; i < arr.Length; i++)
      {
          for (int j = i + 1; j < arr.Length; j++)
          {
              if (arr[j] < arr[i])
              {
                  temp = nums[j];
                  nums[j] = nums[i];
                  nums[i] = temp;
              }
          }
      }
  }
  ```
</details>
  
<details>
  <summary><b>O(2<sup>n</sup></b>)</summary>
  O(2<sup>n</sup>) is represented as "Exponential Time". You will most likely see this with recursive functions that make two recursive calls, in order to solve a problem of size <b><i> n</i></b>.  <br>
  <br>
  An example of O(2^n) is:

  ```cs
  private static int Fibonacci(int n)
  {
      if (n < 2)
      {
          return n;
      }
      else
      {
         return Fibonacci(n - 1) + Fibonacci(n - 2);
      }
  }
  ```
</details>
  
<details>
  <summary><b>O(n!)</b></summary>
  O(n!) is represented as "Factorial Time". You are most likely to see this when writing algorithms that involve permutations. You should know that a factorial is the product of the sequence of <b><i> n</i></b> integers. <b><i>10!</i></b> is already 3,628,800.<br>
  <br>
  I do not have much experience solving common O(n!) algorithms, therefore instead of copying/pasting a solution I do not fully understand, I will share a couple common problems for you to review:

  ```cs
  Traveling Salesman
  Generate all the Permutations of a List
  ```
</details>
  
## Generic Collections
<details>
  <summary><b>Dictionary</b></summary>
  Stores items as a Key/Value pair. I find using a Dictionary can be useful for when you need to add keys (keys must be unique), and be able to track the value for each key. Since the elements are stored as KeyValuePair objects, you can easily loop through the Dictionary to find the value of a key. <br>
 <br>
 An example of using a Dictionary is:
  
  ```cs
    // Every element appears twice except for one element in nums (Example: nums = [4,1,2,1,2]). Find that single one.
    public int SingleNumber(int[] nums)
    {
        // Create a Dictionary
        Dictionary<int, int> dic = new Dictionary<int, int>();
  
        int value = 0;
        
        foreach (int i in nums)
        {
            // Loop through nums, and if the Dictionary does not already contain the key, then add it to the dictionary.
            if (!dic.ContainsKey(i))
            {
                dic.Add(i, 1);
            }
            else
            {   // Here, the key already exists, so instead I add 1 to the value for that key.
                dic[i]++;
            }
        }
        
        // Loop through the K/V pair in the Dictionary, and check for when the Value is equal to one.
        foreach (KeyValuePair<int, int> pair in dic)
        {
            if (pair.Value.Equals(1))
            {
                // Set value equal to the key that has a value of 1
                value = pair.Key; 
            }
        }
        // Returning value which is the key that had a value of 1.
        return value;
    }
```
</details>

<details>
  <summary><b>List</b></summary>
  Similar to an Array but able to add/remove items from it during run time. <br>
  <br>
  An example of using a List is:
  
  ```cs
      public static void Main(string[] args)
      {
          List<int> myList = new List<int>();

          myList.Add(1);
          myList.Add(2);
          myList.Add(3);
          foreach (int i in myList)
          {
              Console.WriteLine(i);
          }
          Console.WriteLine("--------");

          myList.Reverse();
          foreach (int i in myList)
          {
              Console.WriteLine(i);
          }
          Console.WriteLine("--------");

          myList.Remove(2);
          foreach (int i in myList)
          {
              Console.WriteLine(i);
          }
          Console.WriteLine("--------");

          Console.WriteLine("The index of number 3 is: " + myList.IndexOf(3));

          Console.WriteLine(myList.ToArray());
      }
  
      /*
      1
      2
      3
      --------
      3
      2
      1
      --------
      3
      1
      --------
      The index of number 3 is: 0
      System.Int32[]
      */
```
</details>
  
<details>
  <summary><b>Queue</b></summary>
  Stores items on First-in-First-out (FIFO) basis. You can remember how a Queue works by thinking of waiting in a line. The person at the front of the line, will be first the first person out of the line. <br>
  <br>
  An example of a using a Queue is:
  
```cs
    static void Main(string[] args)
    {
        Queue<int> myQueue = new Queue<int>();
  
        myQueue.Enqueue(1);
        myQueue.Enqueue(2);
        myQueue.Enqueue(3);
        myQueue.Enqueue(4);
        myQueue.Dequeue();
        myQueue.Peek();
  
        foreach(int i in myQueue)
        {
            Console.WriteLine(i);
        }
  
        Console.WriteLine(myQueue.Peek());
  
      /* Output:
         2
         3
         4
         
         2 // Peek() returns the object at the beginning of the Queue without removing it
      */
    }
  ```
</details>
  
<details>
  <summary><b>Stack</b></summary>
  Stores items on a Last-in-First-out (LIFO) basis. Some important methods of a Stack are: Peek() which returns the object at the top of the Stack without removing it, Pop() which removes and returns the object at the top of the Stack, and Push() which inserts an object at the top of the Stack. <br>
  <br>
  I find myself frequently using Stacks when implementing an iterative approach during a tree traversal, an example being:
  
  ```cs
  /**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution
{
    public IList<int> InorderTraversal(TreeNode root) 
    {
        List<int> list = new List<int>();
        if (root == null) return list;
        
        Stack<TreeNode> stack = new Stack<TreeNode>();
        
        while (stack.Count != 0 || root != null)
        {
            if (root != null)
            {
                stack.Push(root);
                root = root.left;
            }
            else
            {
                root = stack.Pop();
                list.Add(root.val);
                root = root.right;
            }
        }
        
        return list;
    }
}
```
</details>
  
<details>
  <summary><b>HashSet</b></summary>
  Functions similar to a List by being able to add and remove items, however the main difference is that it can only store unordered unique elements (no duplicates). You can also use similar properties and functions with a HashSet that you can with a List such as Count(), Add(), and Remove(). <br>
  <br>
An example of using a HashSet is:
  
```cs
  public static void Main(string[] args)
  {

    HashSet<int> hash = new HashSet<int>();

    hash.Add(1);
    hash.Add(2);
    hash.Add(3);
    hash.Add(3); // NOTE: This 3 is not added since it's duplicative.
    foreach (int i in hash)
    {
        Console.WriteLine(i);
    }
    Console.WriteLine("--------");

    hash.Remove(2);
    foreach (int i in hash)
    {
        Console.WriteLine(i);
    }

    /*
    1
    2
    3
    --------
    1
    3
    */
  }
```
</details>
  
<details>
  <summary><b>Linked List</b></summary>
  Linked Lists are worth reading into further than this brief summary, as they are often used in interview questions and they can be a bit tricky to understand. Linked Lists provide O(1) insertion and removal operations. <br>
  You can visualize a Linked List as a list that contains nodes and each node contains a value and pointer (the link) to the next node within the list. <br>
  <br>
  
  ```
          [5] -> [4] -> [9] -> [6]
  ```
  <br>
An example of using a Linked List is:
  
```cs
  /**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int val=0, ListNode next=null) {
 *         this.val = val;
 *         this.next = next;
 *     }
 * }
 */
  public class Solution 
  {
      public ListNode ReverseList(ListNode head)
      {
          ListNode prev = null;

          while (head != null)
          {
              ListNode temp = head;
              head = head.next;
              temp.next = prev; // Turns the pointer backwards
              prev = temp;
          } 
          return prev;
      }
  }
```
</details>
  
## Object Oriented Programming
 <details>
    <summary><b>Overview</b></summary>
  Object Oriented Programming (OOP) is a programming style that focuses on the use of classes and objects. You will commonly hear this structure used with examples such as the code classes being blueprints that build instances of objects. The class is often a broad category, that will then share attributes, but the objects themselves will have their own values. An example that I originally learned this from was a car program. Imagine you have a class named <code>Car</code>, and it has the attributes of: <code>model</code>, <code>color</code>, <code>year</code>. We could then give the Car class a method as well, such as, <code>ChangeColor();</code>. After that we would make an instance of the class, which is the object, by using the <code>new</code> keyword, such as <code>Car myFirstCar = new Car();</code>. From here you could make several car instances from the <code>Car</code> class that all have different values for the attributes listed above. <br>
   <br>
  There are four pillars of OOP, which are: Encapsulation, Abstraction, Inheritance, and Polymorphism.
 </details>
  
 <details>
   <summary><b>Encapsulation</b></summary>
   Encapsulation uses public/private modifiers to restrict what attributes can (and can not) be accessed. Attributes of the class are often kept private and public get and set accessors are provided to manipulate these attributes. Proper use of Encapsulation will help us avoid breaking things that are not related to the change we are making, increase readability and maintainability, and reduce complexity by means of decoupling.
   
 ```cs
 class Enemy
{
    private string name;
    public string Name // Property
    {
        get { return name; } // get accessor which returns the value of the Name property
        set { name = value; } // set accessor to set a new value. The value keyword represents the value being assigned to the property.
    }
}
```
```cs
  class Program
 {
    static void Main(string[] args)
    {
        Enemy enemy = new Enemy();
        enemy.Name = "Bowser"; // set accessor will invoke
        Console.WriteLine(enemy.Name); // get accessor will invoke
    }
 }
```
   
 </details>
  
 <details>
   <summary><b>Abstraction</b></summary>
 </details>
  
 <details>
   <summary><b>Inheritance</b></summary>
   Child classes inherit data and behaviors from parent class.
 </details>
  
 <details>
   <summary><b>Polymorphism</b></summary>
 </details>
  
## Common Searching/Sorting Algorithms
 <details>
   <summary><b>Binary Search</b></summary>
   Binary Search is a fast algorithm to find a value in a sorted array (or any sorted sequence). The algorithm works by initially searching entire sequence. At each step, the algorithm compares the median value in the search space to the target value. Due to the sequence being sorted, it can then eliminate half of the search space. By doing this repeatedly, it will eventually be left with a search space consisting of a single element, the target value/index, or we can return out of the function if the target does not exist at this point.

```cs
// Binary Search in a sorted array
  private static int SearchForTargetIndex(int[] arr, int target)
  {
      int leftPointer = 0;
      int rightPointer = arr.Length - 1;
    
      while (leftPointer <= rightPointer)
      {
          int mid = (leftPointer + rightPointer) / 2;

          if (arr[mid] == target)
          {
              return mid;
          }
          else if (target < arr[mid])
          {
              rightPointer = mid - 1;
          }
          else
          {
              leftPointer = mid + 1;
          }
      }
      return false; // Target does not exist.
  }
```
 </details>
  
 <details>
   <summary><b>Merge Sort</b></summary>
 </details>
  
 <details>
   <summary><b>Quick Sort</b></summary>
 </details>
  
## Binary Trees and Binary Search Trees
<details>
  <summary><b>Binary Trees vs Binary Search Trees</b></summary>
  The focus of this section will be specifically on Binary Search Trees, but I think it is important to have an understanding that Binary Trees and Binary Search Trees are different. <br>
  <br>
  I like to think of Binary Trees as a basic data structure, that involves a collection of nodes. The only rule for a Binary Tree is that a parent node can never have more than two children (hence "Binary"). Hierarchically, a Binary Search Tree shares the same structure as a Binary Tree, but the Binary Search Tree nodes are organized in a way where the left child only contains nodes with values less than the parent node, and the right child only contains nodes with values greater than (or equal) to the parent node. <br>
  <br>
  You can think of the Binary Tree as a more basic/general data stucture of a Binary Search Tree, but keeping in mind that they do differ. All Binary Search Trees are by definition a Binary Tree, but not all Binary Trees are Binary Search Trees. A Binary Search Tree is simply a variation of the Binary Tree, pertaining to how the nodes are organized. <br>
  <br>
  
  ```
                                      Binary Tree           Binary Search Tree
                                         100                      102
                                        /   \                    /   \
                                      101    102               100   103

```
</details>
  
<details>
  
  <summary><b>Basic Operations of a Binary Search Tree</b></summary>
  To my understanding, when working with BSTs, it is best to try to work with "Balanced" trees. A balanced BST is when the left and right subtrees only differ in height by at most one from every node. <br>
  <br>
 
```
                                    Unbalanced BST         Balanced Binary Search Tree
                                       103                     102
                                      /                       /   \
                                    102                     100    103
                                    /
                                  100

```
  The reason for working with a balanced BST, rather than an unbalanced BST, is that we can get the time complexity for Searching, Inserting and Deleting a node, all down to O(log n) (as opposed to the slower O(n) time complexity for an unbalanced BST).
  <br>
    <details>
      <summary><b>Search</b></summary>
      Search for an element in the tree. <br>
      This could be solved iteratively or recursively. Below is my example of searching for an element in a BST iteratively:
```cs
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution 
{
    public TreeNode SearchBST(TreeNode root, int val) 
    {
        while (root != null)
        {
            if (root.val == val)
            {
                return root;
            }
            
            if (val < root.val)
            {
                root = root.left;
            }
            else
            {
                root = root.right;
            }
        }
        
        return null;
    }
}
```
  </details>
    <details>
      <summary><b>Insert</b></summary>
      Insert an element in the tree. <br>
      This could be solved iteratively or recursively. Below is my example of inserting an element in a BST recursively & iteratively: <br>
      
```cs
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution 
{
    public TreeNode InsertIntoBST(TreeNode root, int val)
    {
        if (root == null) return new TreeNode(val);
        
        if (root.val > val)
        {
            root.left = InsertIntoBST(root.left, val);
        }
        else
        {
            root.right = InsertIntoBST(root.right, val);
        }
        
        return root;
    }
}
      
// Iterative solution below:
      
public class Solution
{
    public TreeNode InsertIntoBST(TreeNode root, int val)
    {
        if (root == null) return new TreeNode(val);
        TreeNode current = root;
        
        while (true)
        {
            if (current.val <= val)
            {
                if (current.right != null)
                {
                    current = current.right;
                }
                else
                {
                    current.right = new TreeNode(val);
                    break;
                }
            }
            else
            {
                if (current.left != null)
                {
                    current = current.left;
                }
                else
                {
                    current.left = new TreeNode(val);
                    break;
                }
            }
        }
        
        return root;
    }
}
```
      
</details>
    <details>
      <summary><b>Delete</b></summary>
      Delete an element in the tree. <br>
      This is by far the most complex operation to do when it comes to the three (Search, Insert, and Delete). This is because there are three conditions that you must check for when wanting to delete a node. <br>
      <br>
      First condition to check: If the node doesn't have any children. <br>
      Second condition to check: If the node has one child (left or right). <br>
      Third condition to check: If the node has two children. <br>
      <br>
      An example of deleting a node from a BST can be seen below: <br>
      
```cs
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
public class Solution 
{ 
    public TreeNode DeleteNode(TreeNode root, int key) 
    {
        if (root == null) return null;
        
        if (key < root.val)
        {
            root.left =  DeleteNode(root.left, key);
        }  
        else if (key > root.val )
        {
            root.right =  DeleteNode(root.right, key);
        }
        else // Here we've found the node that we need to delete.
        {
            root = DeleteFoundNode(root,key); 
        }
        
        return root;
    }
    
    public TreeNode DeleteFoundNode(TreeNode root, int key) 
    {
        if (root.right == null && root.left == null) // 1. Node has no children.
        {
            root = null;
        }
        else if (root.right == null) // 2. Node has one child either right or left.
        {
            root = root.left;
        }
        else if (root.left == null)
        {
            root = root.right;
        }   
        else // 3. Node has two children.
        {
            TreeNode minNode = FindMinValue(root.right);
            root.val = minNode.val;
            root.right = DeleteNode(root.right, minNode.val);
        }
        
        return root;        
    }
    
    public TreeNode FindMinValue(TreeNode root)
    {
        while (root.left != null)
        {
            root = root.left;
        }
        
        return root;
    }
}
```
   </details>
</details>
  
<details>
  <br>
  <summary><b>Traversing a Tree</b></summary>
    <details>
      <summary><b>Breadth First Search (Level-order Traversal)</b></summary>
      Breadth First Search (BFS), also known as Level-order Traversal, is when you a traverse a tree starting from the root node and then explore all the nodes at the current depth, before moving on to explore all the nodes at the next depth. I like to think of this as going from left to right as you explore the nodes at each level of the tree. <br>
      <br>
      An example to print out nodes in a Level-order traversal is: <br>
      
```cs
private void LevelOrderTraversal(Node root)
{
    // Base Case
    if (root == null) return;

    Queue<Node> q = new Queue<Node>();
    q.Enqueue(root);

    while(true)
    {
        int nodeCount = q.Count;
        if (nodeCount == 0) break;

        while(nodeCount > 0)
        {
            Node node = q.Peek();
            // Now remove the current node
            q.Dequeue();

            // Check for children nodes and Enqueue
            if(node.left != null)
              q.Enqueue(node.left);
            if(node.right != null)
                q.Enqueue(node.right);

            nodeCount--;
        }
    }
}   
 ```  
 </details>

  
<details>
  
  <summary><b>Depth First Search</b></summary>
  Depth First Search (DFS) is an algorithm used to traverse tree or graph structures. The algorithm starts at the root node, and then traverses are far as possible before backtracking. There are three commons ways to traverse a tree (or graph) in a DFS manner: Pre-Order, In-Order, and Post-Order.
    <details>
      <summary><b>Pre-order Traversal</b></summary>
      Traverse the tree in Pre-order (Root, Left, Right). You could use this traversal when you want to create a copy of a binary search tree. <br>
      <br>
      This means you will first visit the root node, then visit the left child (which includes its entire subtree), and lastly visit the right child (also including its entire subtree). A recursive example can be seen below when combining two binary trees into one: <br>
      
```cs
      /**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
      
public class Solution 
{
    public TreeNode MergeTrees(TreeNode root1, TreeNode root2)
    {
        if (root1 == null) return root2;
        if (root2 == null) return root1;
        
        root1.val += root2.val;
        root1.left = MergeTrees(root1.left, root2.left);
        root1.right = MergeTrees(root1.right, root2.right);
        
        return root1;
    }
}
```
</details>
  
 <details>
      <summary><b>In-order Traversal</b></summary>
      Traverse the tree in In-order (Left, Root, Right). Often used to get the values of nodes from a tree in ascending order (with a BST). <br>
   <br>
   This means you will first visit the left child (which includes its entire subtree), then visit the root node, and lastly visit the right child (also including its entire subtree). When performing In-order Traversal on a BST, this will result in all nodes being visited in ascending order.
A recursive example can be seen below: <br>
   
```cs
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
   
public class Solution 
{
    // Create a list to store the nodes
    List<int> list = new List<int>();
    
    public IList<int> InorderTraversal(TreeNode root) 
    {
        InOrder(root);
        return list;
    }
    
    public void InOrder(TreeNode root)
    {
        // Base case for recursive call
        if (root == null)
        {
            return;
        }

        InOrder(root.left);
        list.Add(root.val);
        InOrder(root.right);
    }
}
```
</details>
    <details>
      <summary><b>Post-order Traversal</b></summary>
      Traverse the tree in Post-order (Left, Right, Root). If you know you will need to explore all the leaves before any nodes, then Post-Order will be the fastest. <br>
      <br>
      An example of traversing and outputting the nodes of a tree in Post-order is:
      
```cs
/*
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left;
 *     public TreeNode right;
 *     public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
      
public class Solution
{
    List<int> list = new List<int>();

    public IList<int> PostorderTraversal(TreeNode root)
    {
        PostOrder(root);
        return list;
    }

    private void PostOrder(TreeNode root)
    {
        // Base case
        if (root == null) return;

        PostOrder(root.left);
        PostOrder(root.right);
        list.Add(root);
    }
}
```
  </details>
</details>

