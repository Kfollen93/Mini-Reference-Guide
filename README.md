# Mini-Reference-Guide :bookmark_tabs:
This repository represents a mini reference guide of topics I either search for often throughout my studies, topics that I needed a clearer explanation on, and/or topics that I felt were just generally important to know. This mini guide is intended to be used as a central location for the reasons listed above; it is not intended to be a full reference guide. <br>
<br>
Since I have found it is best to learn by doing, this mini guide uses C# code examples that I have created myself in a way that make sense to me.
After shuffling through my notes and erasing/re-writing these topics numerous times on my whiteboard, I figured it would be worth making this public and sharing with others. <br>

Therefore, if you found any of this useful, please consider giving it a star! :star: <br>
Furthermore, if you believe any example could be made clearer, please submit a pull request conforming with the style used below. :heavy_check_mark:

# Table of Contents  
* Big O Notation :o:
* Generic Collections                  --> <i>In progress</i> :hammer:
* Object Oriented Programming          --> <i>In progress</i> :hammer:
* Common Sorting Algorithms            --> <i>In progress</i> :hammer:
* Linked Lists                         --> <i>In progress</i> :hammer:
* Binary Trees and Binary Search Trees --> <i>In progress</i> :hammer:

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
  An example of O(n^2) is:

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
  Similar to an Array but able to add/remove items from it during run time.
</details>
  
<details>
  <summary><b>Queue</b></summary>
  Stores items on First-in-First-out (FIFO) basis.
</details>
  
<details>
  <summary><b>Stack</b></summary>
  Stores items on a Last-in-First-out (LIFO) basis.
</details>
  
<details>
  <summary><b>HashSet</b></summary>
  Functions similar to a List by being able to add and remove items, however the main difference is that it can only store unique elements (no duplicates).
</details>
  
## Object Oriented Programming
  
## Common Sorting Algorithms
  
## Linked Lists
  
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
 
```
                                      Unbalanced BST         Balanced Binary Search Tree
                                         103                     102
                                        /                       /   \
                                      102                     100    103
                                      /
                                    103

```
  The reason for working with a balanced BST, rather than an unbalanced BST, is that we can get the time complexity for Searching, Inserting and Deleting a node, all down to O(log n) (as opposed to the slower O(n) time complexity for an unbalanced BST).
  <br>
    <details>
      <summary><b>Search</b></summary>
      Search for an element in the tree. <br>
      This could be solved iteratively or recursively. Below is my example of searching for an element in a BST iteratively: <br>
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
      Insert an element in the tree.
    </details>
    <details>
      <summary><b>Delete</b></summary>
      Delete an element in the tree.
    </details>
</details>
  
<details>
  <br>
  <summary><b>Traversing a Tree</b></summary>
    <details>
      <summary><b>Breadth First Search (Level-order Traversal)</b></summary>
      Breadth First Search, also known as Level-order Traversal, is...
    </details>

  
<details>
  
  <summary><b>Depth First Search</b></summary>
  Depth First Search (DFS) is an algorithm used to traverse tree or graph structures. The algorithm starts at the root node, and then traverses are far as possible before backtracking. There are three commons ways to traverse a tree (or graph) in a DFS manner: Pre-Order, In-Order, and Post-Order.
    <details>
      <summary><b>Pre-order Traversal</b></summary>
      Traverse the tree in pre-order. This means...
    </details>
 <details>
      <summary><b>In-order Traversal</b></summary>
      Traverse the tree in In-order (Left, Root, Right). <br> This means you will first visit the left child (which includes its entire subtree), then visit the root node, and lastly visit the right child (also including its entire subtree). When performing In-order Traversal on a BST, this will result in all nodes being visited in ascending order.
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
      Traverse the tree in post-order. This means...
    </details>
</details>

