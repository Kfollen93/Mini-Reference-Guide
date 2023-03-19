# Mini-Reference-Guide :bookmark_tabs:
This repository represents a mini guide of topics I either searched for often throughout my studies, topics that I needed a clearer explanation on, and/or topics that I felt were just generally important to know. This mini guide is intended to be used as a central location for the reasons listed above; it is not intended to be a full reference guide. <br>
<br>
Since I have found it is best to learn by doing, this mini guide uses C# code examples that I have created myself in a way that make sense to me.
After shuffling through my notes and erasing/re-writing these topics numerous times on my whiteboard, I figured it would be worth making this public and sharing with others. <br>

# Table of Contents  
* [Big O Notation](#big-o-notation) :o:
* [Generic Collections](#generic-collections) :wrench:
* [Object Oriented Programming](#object-oriented-programming) :classical_building:
* [Common Sorting Algorithms](#common-searching-and-sorting-algorithms) :balance_scale:
* [Binary Trees and Binary Search Trees](#binary-trees-and-binary-search-trees) :evergreen_tree:
* [Class, Record, Struct, Abstract Class, Interface](#class-record-struct-abstract-class-interface) :scroll:
* [Dependency Injection](#dependency-injection) :pushpin:
* [Value Types & Reference Types](#value-types--reference-types) :point_right:
* [Delegates, Actions, and Events](#delegates-actions-and-events)
* Miscellaneous *(WIP)*
* [Tips](#tips) *(WIP)* :notebook:	

## Big O Notation
<details>
  <summary><b>Big-O Complexity Chart</b></summary>
<br>
<img src=https://github.com/Kfollen93/Mini-Reference-Guide/blob/main/Images/BigOChart.png alt="Big O Complexity Chart"> <br>
https://www.bigocheatsheet.com/
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
  // Binary Search
  private int SearchForTargetIndex(int[] arr, int target)
  {
      int leftPointer = 0;
      int rightPointer = arr.Length - 1;
    
      while (leftPointer <= rightPointer)
      {
          // Prevent integer overflow.
          int mid = leftPointer + (rightPointer - leftPointer) / 2;

          if (arr[mid] == target)
              return mid;
          else if (target < arr[mid])
              rightPointer = mid - 1;
          else
              leftPointer = mid + 1;
      }
      return false;
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
 It is worth noting in the example above, if the array was sorted already, we could apply Binary Search to solve this and reduce the time complexity down to O(log n), but assuming the array is not sorted, then this would be the most optimal solution, as sorting an array is slower than a linear search (as you will see in the next example).
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
  O(n<sup>2</sup>) is represented as "Quadratic Time". You will most typically see this with nested loops (although do not fall victim to thinking if it is a nested loop that it is automatically Quadratic Time; this is not how you calculate time complexity). If you are looping through the outer array <b><i> n </i></b> times, then the inner loop will also need to run <b><i> n </i></b> times during each iteration of the outer loop (the square of n).  <br>
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
  Stores items as a Key/Value pair. I find using a Dictionary can be useful for a lot of situations. Just remember keys must be unique! The Dictionary structure utilizes hashing, allowing for many methods (Add(), ContainsKey(), etc) to run in O(1) time (similiar to a HashSet).<br>
 <br>
 An example of using a Dictionary is:
  
  ```cs
    // Every element appears twice except for one element in nums.
    // (Example: nums = [4,1,2,1,2]). Find that single one.
    public int SingleNumber(int[] nums)
    {
        // Create a Dictionary
        Dictionary<int, int> dic = new Dictionary<int, int>();
        int value = 0;
        foreach (int i in nums)
        {
            if (!dic.ContainsKey(i))
            {
                dic.Add(i, 1);
            }
            else
            {
                dic[i]++;
            }
            
            // Alternatively this could be done using TryAdd()
            if (!dic.TryAdd(i, 1)) dic[i]++;
        }
        
        foreach (KeyValuePair<int, int> pair in dic)
        {
            if (pair.Value.Equals(1))
            {
                value = pair.Key; 
            }
        }
        return value;
        
        // The example above is a bit overkill too, there's many ways to do this, but I wanted to display a potential way of using an array.
        // You could solve this through LINQ's Group By, adding to HashSet, etc.
    }
```
</details>

<details>
  <summary><b>List</b></summary>
  Similar to an Array but able to add/remove items from it during run time. Array memory is static and continuous. List memory is dynamic and random. List has a .Count property and if .Count equals .Capacity, then the capacity of the List is increased by automatically reallocating the internal array, and the existing elements are copied to the new array before the new element is added. However, if .Count is less than .Capacity, this calling .Add() is an O(1) operation. If the capacity needs to be increased to accommodate the new element, this method becomes an O(n) operation, where n is Count. Remove() is a linear search. <br>
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
      Print out would look like:
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
  Stores items on First-in-First-out (FIFO) basis. You can remember how a Queue works by thinking of waiting in a line. The person at the front of the line, will be first the first person out of the line. Enqueue() is an O(1) operation if the Count is less than the capacity of the internal array (otherwise it is linear). Where as Dequeue() and Peek() are always an O(1) operation. Queues are often seen used in Breadth First Search (BFS) algorithms.<br>
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
  Stores items on a Last-in-First-out (LIFO) basis. Some important methods of a Stack are: Peek() which returns the object at the top of the Stack without removing it, Pop() which removes and returns the object at the top of the Stack, and Push() which inserts an object at the top of the Stack. A Stack is often used for Depth First Search (DFS) solutions and/or can be implemented as an iterative solution, as an alternative to a recursive solution.<br>
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
  Functions similar to a List by being able to add and remove items, however the main difference is that it can only store unordered unique elements (no duplicates). In addition to the above, a HashSet set has a O(1) lookup time, compared to a List or Array, which is O(n). A HashSet has similar functions to a List such as Count(), Add(), and Remove(). <br>
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
    
    // Note: Since a HashSet can only hold unique items, and .Add() is a boolean, you can simply call .Add(),
    // without checking .Contains() first. Example:
    
    if (hash.Add(5)) // If '5' isn't in the HashSet already, then the statement will be true and 5 will be added.
    
  }
```
</details>
  
<details>
  <summary><b>Linked List</b></summary>
  Linked list data items consists of nodes. A node is a combination of data and a pointer to the next node which is stored somewhere in a random memory location. Linked Lists are worth reading into further than this brief summary, as they are often used in interview questions and they can be a bit tricky to understand. Linked Lists provide O(1) insertion and removal (in a singly-linked list) if the pointer to the node right before the one you want to insert or remove is known, otherwise you will have an O(n) search to find that node. <br>
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
          
          // This could also be implemented with a Stack :) 
      }
  }
```
</details>
  
## Object Oriented Programming
 <details>
    <summary><b>Overview</b></summary>
  Object Oriented Programming (OOP) is a programming style that focuses on the use of classes and objects. You will commonly hear this structure used with examples such as the code classes being blueprints that build instances of objects. The class is often a broad category, that will then share attributes, but the objects themselves will have their own values. Imagine you have a class named <code>Car</code>, and it has the attributes of: <code>model</code>, <code>color</code>, <code>year</code>. We could then give the Car class a method as well, such as, <code>ChangeColor();</code>. After that we would make an instance of the class, which is the object, by using the <code>new</code> keyword, such as <code>Car myFirstCar = new Car();</code>. From here you could make several car instances from the <code>Car</code> class that all have different values for the attributes listed above. <br>
   <br>
  There are four pillars of OOP, which are: Encapsulation, Abstraction, Inheritance, and Polymorphism.
 </details>
  
 <details>
   <summary><b>Encapsulation</b></summary>
   Encapsulation uses public/private modifiers to restrict what attributes can (and can not) be accessed. Attributes of the class are often kept private and public get and set accessors are provided to manipulate these attributes. Proper use of Encapsulation will help us avoid breaking things that are not related to the change we are making, increase readability and maintainability, and reduce complexity by means of decoupling. <br>
   
 ```cs
 class Enemy
{
    private string name;
    public string Name // Property
    {
        get { return name; } // get accessor which returns the value of the Name property
        set { name = value; } // set accessor to set a new value. The value keyword represents the value being assigned to the property.
    }
    
    // This could also be simplified to:
    public string Name { get; set; } // Publicly get the value and pubicly set it.
    public string Name { get; private set; } // Publicly get the value, but it can only be set privately within the class.
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
   Abstraction only shows the essential information, and hides any unnecessary details from the user. Abstraction is often implemented through interfaces and by using abstract classes. Abstraction and Encapsulation relate in a lot of ways, but Abstraction's focus is around creating an interface through which classes can interact with, and keeping the code decoupled so that each object is its own entity.<br>
   <br>
We're abstracting away the actual implmentation of how something works.  We're not interested in those specifics. A great example can be seen with a car. There are few things we need to know about a car in order to use it: how much gas it has, how the steering wheel works, where the brake pedal and gas pedal are, and so forth. However, we don't need to know exactly how the car works when we press the gas pedal and how this actually causes the car to move, along with how the break pedal works to slow down the car. All we care about is that if we call the method <code>MoveCar()</code> that the car would move. Therefore, by using interfaces, sections of code can communicate with each other, but they don't depend on each other to work.
 </details>
  
 <details>
   <summary><b>Inheritance</b></summary>
   At the simplest level, Inheritance involves the child class inheriting data and behaviors from the parent class. Within OOP, inhertiance can be defined as a "is-a" relationship between child and parent class. Sticking with the car example, it would make sense to have a parent class named <code>Car</code>. This class would contain data that is shared among all cars such as: <code>type</code>, <code>model</code>, <code>year</code>, <code>color</code>, and so forth. The idea of this parent class is that it contains general data that our child classes can inherit when we create an object. For example, now that the parent class <code>Car</code> is complete, we could make a child class such as <code>BMW</code> that inherits from <code>Car</code> and create an object. This might look like:
   
   ```cs
    class Car
    {
        public string Type { get; set; }
        public int NumOfDoors { get; set; }
        public int Year { get; set; }
        public string Color { get; set; }

        public void PushHorn()
        {
            Console.WriteLine("Honk honk!");
        }
    }
```

```cs
    class Bmw : Car
    {
        public string bmwSeries = "Default Series";

        public void DisplayBMWLogo()
        {
            Console.WriteLine("Only BMW cars come with the BMW logo.");
        }
    }
```

```cs
class Program
    {
        static void Main(string[] args)
        {
            Bmw myBmw = new Bmw();
            {
                myBmw.Type = "gas";
                myBmw.NumOfDoors = 2;
                myBmw.Year = 2004;
                myBmw.Color = "red";
                myBmw.bmwSeries = "series 3";
            };

            myBmw.PushHorn();
            Console.WriteLine($"My BMW is a {myBmw.NumOfDoors} door car, and it is a {myBmw.bmwSeries}.");
            myBmw.DisplayBMWLogo();
   
            /* Output:
               Honk honk!
               My BMW is a 2 door car, and it is a series 3.
               Only BMW cars come with the BMW logo. */
        }
    }
```
   You can then create as many other car classes that you want like <code>Audi</code> for example, and then have it also inherit from the <code>Car</code> base class. This way you are able to reuse the fields created, and tailor them to the car you create, without having to re-write the same code each time.

 </details>
  
 <details>
   <summary><b>Polymorphism</b></summary>
   Polymorphism is the condition of occurring in several different forms. In programming, this means that Polymorphism provides a class with the ability to have multiple implementations with the same name. Polymorphism tends to expand on Inheritance by allowing us to use inherited methods from another class to perform different tasks. This gives us the opportunity to call a single method in many different ways (specifically by including the use of virtual and override methods). <br>
To make this more clear, I have posted an example below directly from the Microsoft documentation on Polymorphism:

```cs
// https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/polymorphism
public class Shape
{
    // A few example members
    public int X { get; private set; }
    public int Y { get; private set; }
    public int Height { get; set; }
    public int Width { get; set; }

    // Virtual method
    public virtual void Draw()
    {
        Console.WriteLine("Performing base class drawing tasks");
    }
}

public class Circle : Shape
{
    public override void Draw()
    {
        // Code to draw a circle...
        Console.WriteLine("Drawing a circle");
        base.Draw();
    }
}
public class Rectangle : Shape
{
    public override void Draw()
    {
        // Code to draw a rectangle...
        Console.WriteLine("Drawing a rectangle");
        base.Draw();
    }
}
public class Triangle : Shape
{
    public override void Draw()
    {
        // Code to draw a triangle...
        Console.WriteLine("Drawing a triangle");
        base.Draw();
    }
}

// Polymorphism at work #1: a Rectangle, Triangle and Circle
// can all be used whereever a Shape is expected. No cast is
// required because an implicit conversion exists from a derived
// class to its base class.
var shapes = new List<Shape>
{
    new Rectangle(),
    new Triangle(),
    new Circle()
};

// Polymorphism at work #2: the virtual method Draw is
// invoked on each of the derived classes, not the base class.
foreach (var shape in shapes)
{
    shape.Draw();
}
/* Output:
    Drawing a rectangle
    Performing base class drawing tasks
    Drawing a triangle
    Performing base class drawing tasks
    Drawing a circle
    Performing base class drawing tasks
*/
```

  </details>
  
## Common Searching and Sorting Algorithms
 <details>
   <summary><b>Binary Search</b></summary>
   Binary Search is a fast algorithm to find a value in a sorted array (or any sorted sequence). The algorithm works by initially searching the entire sequence. At each step, the algorithm compares the median value in the search space to the target value. Due to the sequence being sorted, it can then eliminate half of the search space. By doing this repeatedly, it will eventually be left with a search space consisting of a single element, the target value/index, or we can return out of the function if the target does not exist at this point. The time complexity of the Binary Search algorithm is <b>O(log n)</b>, and space complexity is <b>O(n)</b>.

```cs
// Binary Search in a sorted array
  private static int SearchForTargetIndex(int[] arr, int target)
  {
      int leftPointer = 0;
      int rightPointer = arr.Length - 1;
    
      while (leftPointer <= rightPointer)
      {
          int mid = leftPointer + (rightPointer - leftPointer) / 2;

          if (arr[mid] == target)
              return mid;
          else if (target < arr[mid])
              rightPointer = mid - 1;
          else
              leftPointer = mid + 1;
      }
      return false; // Target does not exist.
  }
```
 </details>
  
 <details>
   <summary><b>Merge Sort</b></summary>
   Merge Sort is a divide and conquer algorithm that works by breaking an array down into several smaller sub-arrays until each sub-array consists of a single element, and then merges them back into a final sorted array. It is important to note that although my example shows an array being sorted, you will often see Merge Sort being the preferred implementation when dealing with sorting linked-lists. The time complexity of the Merge Sort algorithm is <b>O(n log n)</b>, and space complexity is <b>O(n)</b>.
   
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
   <summary><b>Quick Sort</b></summary>
   Quick Sort is another divide and conquer algorithm. You will often see Quick Sort being the preferred implementation when dealing with sorting arrays. It works by choosing an element as a pivot point, and then partitioning the array around that pivot point so that elements that are smaller than the pivot point are before it, and elements larger than it are after it. Through recursion, we repeat this partition until the array is sorted. Although you could select any element as the pivot point, it is common to choose the median value from the first, last, and middle element of the array. A benefit of Quick Sort is that it sorts in place so it doesn’t require any additional storage. The worst case time complexity of Quick Sort is <b>O(n<sup>2</sup></b>), but the average time complexity (and best) is <b>O(n log n)</b>; however, the space complexity is only <b>O(log n)</b>.
   
   ```cs
public static void QuickSort(int[] arr, int low, int high)
{  
      if (low < high)
      {
          int pivotLocation = Partition(arr, low, high);
          QuickSort(arr, low, pivotLocation - 1);
          QuickSort(arr, pivotLocation + 1, high);
      }
}
        
public static int Partition(int[] arr, int low, int high)
{
      int pivot = arr[high];
      int index = (low - 1);

      for (int i = low; i <= high - 1; i++)
      {
          if (arr[i] < pivot)
          {
              index++;
              Swap(arr, index, i);
          }
      }

      Swap(arr, index + 1, high);
      return (index + 1);
}

public static void Swap(int[] arr, int index, int i)
{
      int temp = arr[index];
      arr[index] = arr[i];
      arr[i] = temp;
}

/*

Driver code to set up and print the sorted array

public static void Main(string[] args)
{

     int[] arr = {2, 6, 5, 3, 8, 7, 1, 0};
     int startOfArr = 0;
     int endOfArr = arr.Length - 1;

     QuickSort(arr, startOfArr, endOfArr);
     foreach (int i in arr)
     {
         Console.WriteLine(i); // 0 1 2 3 5 6 7 8
     }
}

*/ 
```
 </details>
  
## Binary Trees and Binary Search Trees
<details>
  <summary><b>Binary Trees vs Binary Search Trees</b></summary>
  The focus of this section will be specifically on Binary Search Trees, but I think it is important to have an understanding that Binary Trees and Binary Search Trees are different. <br>
  <br>
  I like to think of Binary Trees as a basic data structure that involves a collection of nodes. The only rule for a Binary Tree is that a parent node can never have more than two children (hence "Binary"). Hierarchically, a Binary Search Tree shares the same structure as a Binary Tree, but the Binary Search Tree nodes are organized in a way where the left child only contains nodes with values less than the parent node, and the right child only contains nodes with values greater than (or equal) to the parent node. <br>
  <br>
  You can think of the Binary Tree as a more basic/general data stucture of a Binary Search Tree, but keep in mind that they do differ. All Binary Search Trees are by definition a Binary Tree, but not all Binary Trees are Binary Search Trees. A Binary Search Tree is simply a variation of the Binary Tree, pertaining to how the nodes are organized. <br> With a BST, all nodes to the left of the root node must be less than the root node, and all nodes to the right of the root node must be greater than the root node.
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
      Breadth First Search (BFS), also known as Level-order Traversal, is when you traverse a tree starting from the root node and then explore all the nodes at the current depth, before moving on to explore all the nodes at the next depth. I like to think of this as going from left to right as you explore the nodes at each level of the tree. <br>
      <br>
      An example to return the level order traversal of a tree nodes' values. (i.e., from left to right, level by level) is: <br>
      
```cs
public IList<IList<int>> LevelOrder(TreeNode root)
{
    if (root == null) return new List<IList<int>>();

    var output = new List<IList<int>>();
    var q = new Queue<TreeNode>();
    q.Enqueue(root);

    while (q.Count != 0)
    {
        int size = q.Count;
        List<int> currentLevel = new List<int>();

        for (int i = 0; i < size; i++)
        {
            TreeNode current = q.Dequeue();
            currentLevel.Add(current.val);

            if (current.left != null)
                q.Enqueue(current.left);

            if (current.right != null)
                q.Enqueue(current.right);
        }

        output.Add(currentLevel);
    }    

    return output;
}
 ```  
 </details>

  
<details>
  
  <summary><b>Depth First Search</b></summary>
  Depth First Search (DFS) is an algorithm used to traverse tree or graph structures. The algorithm starts at the root node, and then traverses as far as possible before backtracking. There are three commons ways to traverse a tree (or graph) in a DFS manner: Pre-Order, In-Order, and Post-Order. All of these traversal methods can be implemented iteratively (utilizing a Stack) or recursively.
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
      Traverse the tree in Post-order (Left, Right, Root). If you know you will need to explore all the leaves before any root nodes, then Post-Order will be the fastest. <br>
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
      </details>
      
## Class, Record, Struct, Abstract Class, Interface
<details>
  <summary><b>Which Should I Use?</b></summary>
  As with most programming related things, the answer is "it depends". Generally you will be using a standard Class to create and structure your projects. However, you may encounter times when a Class doesn't quite fit.
</details>
<details>
   <summary><b>Class</b></summary>
   Classes are references types that are mutable by default (however it's possible to make an immutable class). Where a record can be viewed as simply a data structure, a class can hold data and behavior (defined by its methods). A class should be created when you want to create a specific object that has certain features, in order to define the data along with the functionality and behavior.
</details>
<details>
   <summary><b>Record</b></summary>
   Records are defaulted to immutable reference types. I've viewed records as simple sets of data. Where a class holds data with functionality, a record is just a data container. Records also use value-based equality, making them actually more similar to structs than classes in that sense.
</details>
<details>
   <summary><b>Struct</b></summary>
   Structs are value types that "Struct-ure" our data. A performance rule of thumb is that structs should be less than 16 bytes, which makes them typically fairly small. Since structs are passed by value, the size of the struct would be copied if it is passed to a function (compared to only a reference pointer (4 bytes) being passed). Therefore, if all the member fields are value types and it's less than 16 bytes, you should probably opt for using a struct. Two struct objects can also be checked for equality by value using ValueType.Equals(), doing this would not work for a class (as the default implementation of Object.Equals() checks for reference equality, and not the values within).
</details>
<details>
   <summary><b>Abstract Class</b></summary>
   Cannot be instantiated. In order to get access to it, it must be inherited from another class; therefore, it's often featured as a "base class" functionality. C# does not support multiple class inheritance. Abstract classes are great to define a template for a group of classes that will inherit from it, due to those classes sharing common features, but also restricting the (abstract) class itself from being instantiated.
</details>
<details>
   <summary><b>Interface</b></summary>
Interfaces must be implemented by another class in order to be used, but you can use several interfaces within the same class. Interfaces cannot be used to create objects (just like Abstract Classes). An Interface is often defined as a "contract". Any class that implements the interface, needs to implement all of the members defined in the interface. This is the big difference from an Abstract Class, in that data members are not defined in an interface; it is just a collection of properties/function declarations.
</details>
 
 ## Dependency Injection
 <details>
  <summary><b> Overview </b></summary>
  Dependency Injection deals with providing the objects that an object needs, instead of having it construct the objects themselves. It is a software design pattern which enables the development of loosely coupled code. This results in being able to more easily make future changes throughout the application. The last of the SOLID principles (the "D") stands for "Dependency Inversion Principle" which states: "that high-level modules/classes should not depend on low-level modules/classes. Both should depend upon abstractions. Secondly, abstractions should not depend upon details. Details should depend upon abstractions".
  </details>
  <details>
  <summary><b>How to Implement</b></summary>
Within Dependency Injection, there are three types: Constructor Injection, Property Injection, and Method Injection. <br>
<br>
Constructor Injection is what you are most likely to see being used. In this case, the dependency will be provided through the constructor when an instance of the class is created. <br>
<br>
Property Injection works by passing the dependent class object through a public property of the client class. It is not likely that you will see this often. <br>
<br>
Method Injection works by having the client class implement an interface which declares the method(s) to supply the dependency and the injector uses this interface to supply the dependency to the client class. <br>
<br>

Within .NET Core, Dependency Injection (Inversion of Control) can be set up through a built-in service container (*Note: While DI is integrated by default in .NET Core applications, it is not for older .NET Framework applications. For older applications you will need a nuget package, for example, StructureMap*). <br>
<br>
It is common to do this with the Database Context file such as:
```cs
  // Program.cs File:
  builder.Services.AddDbContext<ApiDbContext>(opt => opt.UseSqlServer(builder.Configuration.GetConnectionString("SqlServer")));

  // The file you want to use implementing with Constructor Injection:
  namespace TestAPI.Controllers
  {
      [Route("api/[controller]")]
      [ApiController]
      public class IssueController : ControllerBase
      {
          private readonly ApiDbContext _context;
          public IssueController(ApiDbContext context) => _context = context;

          [HttpGet]
          public async Task<IEnumerable<Issue>> Get()
          {
              return await _context.Issues.ToListAsync();
          }
       }
  }
```
The key here with the database context in specific, is that when the service is injected into the constructor of the class, the built in service takes on the responsibility of creating an instance of the dependency (the context) and disposing of it when it's no longer needed. You can configure the database lifetime in three ways: Transient, Scoped, and Singleton (the default lifetime is Scoped).
<br>
</details>


## Value Types & Reference Types
<details>
  <summary><b>Value Types</b></summary>
  Value Types directly contain their data.
  <br>
  Common Value Types consist of: bool, byte, char, decimal, double, enum, float, int, long, struct, and short. <br>
  Value Types all have a default value based on their type (ex: 0 for Integer, false for boolean, etc.)
 </details>
 
 <details>
 <summary><b>Reference Types</b></summary>
 Reference Types store references to their data.
 <br>
 Common Reference Types consist of: string, array (even if it consists of value types), class, delegate, record, interface, dynamic, and object. <br>
 Reference Types all have a default value of null.
</details>
  
 <details>
 <summary><b>Example of Value vs Reference</b></summary>
  An example below shows the difference between a Struct (Value Type) and a Class (Reference Type) when setting a field: 
  <br>
  
  ```cs
    public class MyClass
    {
        public int value;
        public MyClass(int num) => value = num;
    }
    
    // If a struct declares any field initializers, it must explicitly declare a constructor (otherwise there will be a compiler error).
    // Any explicitly declared constructor (with parameters, or parameterless) executes all field initializers for that struct.
    // All fields without a field initializer or an assignment in a constructor are set to the default value.
    public struct MyStruct
    {
        public int value;
        public MyStruct(int num) => value = num;
    }

    MyClass myClassOne = new MyClass(7);
    MyClass myClassTwo = myClassOne;
    myClassTwo.value = 5;
    Console.WriteLine("ClassOne has a value of: " + myClassOne.value);
    // ClassOne has a value of: 5

    MyStruct myStructOne = new MyStruct(7);
    MyStruct myStructTwo = myStructOne;
    myStructTwo.value = 5;
    Console.WriteLine("StructOne has a value of: " + myStructOne.value);
    // StructOne has a value of: 7
```

</details>

## Delegates, Actions, and Events
<details>
  <summary><b>Overview</b></summary>
  I have limited experience with creating and using Delegates, Actions, and Events within my day job as C#/.NET (mostly api/web) developer, yet I've found Unity to be an awesome place to learn how to implement and utilize this publisher/subscriber system. As an overview, you should do a quick search and read about the Observer Pattern (https://refactoring.guru/design-patterns/observer).
    </details>
    <details>
      <summary><b>Delegate</b></summary>
        As a basis, delegates enable you to store and call a function like it was a variable. For example, if you had your ordinary function:

  ```cs
  void HealthChangeHandler()
  {
    // Code
  }
  ```
  To create a delegate from this method example, it would look like:
  ```cs
  delegate void HealthChangeHandler();
  ```
  This states what kind of function can be stored in the delegate. You would then still need an instance of the delegate:
  ```cs
  delegate void HealthChangeHandler();
  HealthChangeHandler healthChangeHandler;
  ```
  Now a function could be assigned to this delegate:
  ```cs
  void ChangeHealth() { // Code to change health };
  healthChangeHandler = ChangeHealth;
  ```
  This may not look at that useful, but the real magic and help of delegates come into play when you set up your delegate to call different functions from different scripts. For example:

```cs
public class BaseEnemy : MonoBehaviour, IDamageable
{
    [SerializeField] private int _health;
    public delegate void OnBaseEnemyHealthChange();
    public OnBaseEnemyHealthChange onBaseEnemyHealthChange;

    void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.TryGetComponent(out IDealDamage iDealDamage))
        {
            int dmgAmount = TakeDamage(iDealDamage);
            onBaseEnemyHealthChange?.Invoke(dmgAmount);
            _health -= dmgAmount;
            if (_health <= 0) DestroyEnemy();
        }
    }
 }
 
public class HealthBarUI : MonoBehaviour
{
    [SerializeField] private Slider _enemyHealthBar;
    [SerializeField] private BaseEnemy _baseEnemyScript;
    private void OnEnable() => _baseEnemyScript.OnBaseEnemyHealthChange += UpdateEnemyHealthBar;
    private void OnDisable() => _baseEnemyScript.OnBaseEnemyHealthChange -= UpdateEnemyHealthBar;
    private void UpdateEnemyHealthBar(int amt) => _enemyHealthBar.value -= amt;
}
```
Although in this case I still have a direct reference to the baseEnemyScript, I am not reliant on the Update() function to constantly poll waiting to update the healthbar UI. Instead, it's separated by having the UI listen for the onBaseEnemyHealthChange event and then update the healthbar UI accordingly. <br>
Additionally, it's important to always unsubscribe to the function to prevent memory leaks. This is most commonly done in the OnDisable() function built in by Unity.

  </details>
      <details>
    <summary><b>Action</b></summary>
      Temp
  </details>
  <details>
   <summary><b>Event</b></summary>
     Events are similar to delegates. The key difference being that events can only be called from their own class. This is not to be confused with subscribing/unsubscribing from other classes which you can still do, but rather other classes would not be able to clear the event by setting it to null (or any other value). Therefore, events abstract and confine delegates. <br>
     <br>

The best explanation I've found is from Jon Skeet, the author of the "C# in Depth" books, from a StackOverFlow posts, where he commented: <br>

*An event is fundamentally like a property - it's a pair of add/remove methods (instead of the get/set of a property). When you declare a field-like event (i.e. one where you don't specify the add/remove bits yourself) a public event is created, and a private backing field. This lets you raise the event privately, but allow public subscription. With a public delegate field, anyone can remove other people's event handlers, raise the event themselves, etc - it's an encapsulation disaster.<br>
https://stackoverflow.com/questions/3028724/why-do-we-need-the-event-keyword-while-defining-events*
<br>
<br>
Events are then called and utilized in a similar fashion to the delegate section above:
```cs
public delegate void OnGameStart();
public static event OnGameStart onGameStart;
```
<br>
However, another defining difference between a Delegate vs an Event is that delegates typically hold data (like a variable that holds a function), that can then be a parameter within a method. Contrast to delegates, events are closer to having an "event system" with sub/pub events.
</details>

## Miscellaneous
<details>
  <summary><b>Delegates, Actions, Events</b></summary>
  I have limited experience with creating and using Delegates, Actions, and Events within my day job as C#/.NET (mostly api/web) developer, yet I've found Unity to be an awesome place to learn how to implement and utilize this publisher/subscriber system. As an overview, you should do a quick search and read about the Observer Pattern (https://refactoring.guru/design-patterns/observer).
    <details>
      <summary><b>Delegate</b></summary>
        As a basis, delegates enable you to store and call a function like it was a variable. For example, if you had your ordinary function:

  ```cs
  void HealthChangeHandler()
  {
    // Code
  }
  ```
  To create a delegate from this method example, it would look like:
  ```cs
  delegate void HealthChangeHandler();
  ```
  This states what kind of function can be stored in the delegate. You would then still need an instance of the delegate:
  ```cs
  delegate void HealthChangeHandler();
  HealthChangeHandler healthChangeHandler;
  ```
  Now a function could be assigned to this delegate:
  ```cs
  void ChangeHealth() { // Code to change health };
  healthChangeHandler = ChangeHealth;
  ```
  This may not look at that useful, but the real magic and help of delegates come into play when you set up your delegate to call different functions from different scripts. For example:

```cs
public class BaseEnemy : MonoBehaviour, IDamageable
{
    [SerializeField] private int _health;
    public delegate void OnBaseEnemyHealthChange();
    public OnBaseEnemyHealthChange onBaseEnemyHealthChange;

    void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.TryGetComponent(out IDealDamage iDealDamage))
        {
            int dmgAmount = TakeDamage(iDealDamage);
            onBaseEnemyHealthChange?.Invoke(dmgAmount);
            _health -= dmgAmount;
            if (_health <= 0) DestroyEnemy();
        }
    }
 }
 
public class HealthBarUI : MonoBehaviour
{
    [SerializeField] private Slider _enemyHealthBar;
    [SerializeField] private BaseEnemy _baseEnemyScript;
    private void OnEnable() => _baseEnemyScript.OnBaseEnemyHealthChange += UpdateEnemyHealthBar;
    private void OnDisable() => _baseEnemyScript.OnBaseEnemyHealthChange -= UpdateEnemyHealthBar;
    private void UpdateEnemyHealthBar(int amt) => _enemyHealthBar.value -= amt;
}
```
Although in this case I still have a direct reference to the baseEnemyScript, I am not reliant on the Update() function to constantly poll waiting to update the healthbar UI. Instead, it's separated by having the UI listen for the onBaseEnemyHealthChange event and then update the healthbar UI accordingly. <br>
Additionally, it's important to always unsubscribe to the function to prevent memory leaks. This is most commonly done in the OnDisable() function built in by Unity.

  </details>
      <details>
    <summary><b>Action</b></summary>
      Temp
  </details>
  <details>
   <summary><b>Event</b></summary>
     Events are similar to delegates. The key difference being that events can only be called from their own class. This is not to be confused with subscribing/unsubscribing from other classes which you can still do, but rather other classes would not be able to clear the event by setting it to null (or any other value). Therefore, events abstract and confine delegates. <br>
     <br>

The best explanation I've found is from Jon Skeet, the author of the "C# in Depth" books, from a StackOverFlow posts, where he commented: <br>

*An event is fundamentally like a property - it's a pair of add/remove methods (instead of the get/set of a property). When you declare a field-like event (i.e. one where you don't specify the add/remove bits yourself) a public event is created, and a private backing field. This lets you raise the event privately, but allow public subscription. With a public delegate field, anyone can remove other people's event handlers, raise the event themselves, etc - it's an encapsulation disaster.<br>
https://stackoverflow.com/questions/3028724/why-do-we-need-the-event-keyword-while-defining-events*
<br>
<br>
Events are then called and utilized in a similar fashion to the delegate section above:
```cs
public delegate void OnGameStart();
public static event OnGameStart onGameStart;
```
<br>
However, another defining difference between a Delegate vs an Event is that delegates typically hold data (like a variable that holds a function), that can then be a parameter within a method. Contrast to delegates, events are closer to having an "event system" with sub/pub events.
</details>
      
</details>

<details>
  <summary><b>Constructors</b></summary>
  To quote Microsoft: "Constructors enable the programmer to set default values, limit instantiation, and write code that is flexible and easy to read." A constructor is called whenever its class (or struct) is created. You will often utilize a constructor to initialize the private fields of the class while creating an instance for the class. It also common to limit instantiation with a constructor, in terms of providing a means of specifying the required data when the object is created. <br>
<br>
As an example, you may want to specify a required first name and last name when creating an Employee: <br>
<br>

```cs
// You can create a constructor with the shortcut by typing "ctor" and tab in Visual Studio.
class Employee
{
  public string FirstName { get; set; }
  public string LastName { get; set; }
  public string Salary { get; set; }
  
  // Employee constructor                                                   
  public Employee(string firstName, string lastName)                        
  {                                                                         
    FirstName = firstName;                                                  
    LastName = lastName;                                                    
  }                                                                         
}
```
By doing the above, now an Employee object can only be instantiated by defining the `FirstName` and `LastName`. You would get an error if you tried doing:
```cs
var employee = new Employee(); // Error.
var employee = new Employee("Bob", "Smith"); // Works.
```
<br>
There may be times where you want to create an object without any specifications, but also have the option to specify. In this case, you would need to define an empty constructor, along with a second constructor holding the fields you require. You are not restricted to only one constructor.
</details>

<details>
<summary><b>Better Null Checking</b></summary>
  Conditional statements and null checks can be cleaned up with null-coalescing and `??` checks, such as: <br>
  <br>
  
  ```cs
  Dog dog;
  string name = dog?.Name;
  // You could also chain further checks such as if you wanted the length.
  // Now if dog is null, it won't throw an error, and if Name is null, it won't throw an error for checking the Length.
  int lengthOfName = dog?.Name?.Length;
  ```
In this case you can check if the `.Name` property is null or not, and if it is, it will not throw an error. Furthermore, you could pair this up with a default value if `.Name` was null. You can do this by using the `??` operator:
```cs
Dog dog;
string name = dog?.Name ?? "Bruno";
```
If `.Name` was null, then it would return "Bruno", otherwise, it would return the `dog.Name` value. I've found this to be super helpful for minimizing the explicit conditional null checks that I would otherwise have littered everywhere.

</details>

## Tips
<details>
  <summary><b>Unorganized (WIP)</b></summary>
  
 - If a collection is already sorted, could you utilize Binary Search to perform whatever it is that you need on it? However, do not sort a collection (O n log n) purely in order to utilize Binary Search (O log n), as that largely defeats the advantages.
 - When working with Entity Framework think about when you are calling `_db.SaveChanges()` (i.e. don't Add() / SaveChanges() within a loop). There's an option for `AddRange()` and then calling `_db.SaveChanges()` once.
 - When working with Entity Framework, if you're querying the database and just need the result to perform something else but you're not actually needing to modify the entity, you can utilize `AsNoTracking()`. EF will not track the results of your query now, making it much more efficient.
 - If you need to concatenate strings more than a few times, use StringBuilder.
 - If start to get somewhere beyond 4+ parameters, consider consolidating it into its own object and passing that in instead.
 - Clarify your intent when when querying for (a) record(s); `SingleOrDefault()` states that the query should result in only one record, whereas `FirstOrDefault()` will return the first record even though there may be many.
 - Try to prevent deep level nesting of conditional statements. See if you can add a guard clause and return out early instead.
 - Bulk updating/inserting is slow in Entity Framework (particularly if you're not using Entity Framework Core). There are ways to speed this up with setting AutoDetectChangesEnabled to false. Better yet, there are nuget packages for bulk inserting/saving (or you can execute sql commands). Even better, EFC 7.0 added built in bulk operations.
</details>
