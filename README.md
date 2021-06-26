# Mini-Reference-Guide :bookmark_tabs:
This repository represents a mini reference guide of topics I either search for often throughout my studies, topics that I needed a clearer explanation on, and/or topics that I felt were just generally important to know. This mini guide is intended to be used as a central location for the reasons listed above; it is not intended to be a full reference guide. <br>
<br>
Since I have found it is best to learn by doing, this mini guide uses C# code examples that I have created myself in a way that make sense to me.
After shuffling through my notes and erasing/re-writing these topics numerous times on my whiteboard, I figured it would be worth making this public and sharing with others. <br>

Therefore, if you found any of this useful, please consider giving it a star! :star: <br>
Furthermore, if you believe any example could be made clearer, please submit a pull request conforming with the style used below. :heavy_check_mark:

# Table of Contents  
* Big O Notation :o:                       --> <i>In progress</i> :hammer:
* Generic Collections                  --> <i>In progress</i> :hammer:
* Object-Oriented-Programming          --> <i>In progress</i> :hammer:
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
  <summary><b>O(n^2)</b></summary>
  O(n^2) is represented as "Quadratic Time". You will most typically see this with nested loops (although do not fall victim to thinking if it is a nested loop that it is automatically Quadratic Time; this is not how you calculate time complexity). If you are looping through the outer array <b><i> n </i></b> times, then the inner loop will also need to run <b><i> n </i></b> times during each iteration of the outer loop.  <br>
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
  <summary><b>O(2^n)</b></summary>
  O(2^n) is represented as "Exponential Time". You will most likely see this with recursive functions that make two recursive calls, in order to solve a problem of size <b><i> n</i></b>.  <br>
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

