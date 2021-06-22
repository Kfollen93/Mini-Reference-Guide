# Mini-Reference-Guide :bookmark_tabs:
This repository represents a mini reference guide of topics I either search for often throughout my studies, topics that I needed a clearer explanation on, and/or topics that I felt were just generally important to know. This mini guide is intended to be used as a central location for the reasons listed above; it is not intended to be a full reference guide. <br>
<br>
Since I have found it is best to learn by doing, this mini guide uses C# code examples that I have created myself in a way that make sense to me.
After shuffling through my notes and erasing/re-writing these topics numerous times on my whiteboard, I figured it would be worth making this public and sharing with others. <br>

Therefore, if you found any of this useful, please consider giving it a star! :star:

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
  <summary><b>O(1) Constant Time</b></summary>
  O(1) is represented as "constant time". This means that regardless of the amount of data that is involved, whether it's 10,000 or 10, it will always require the same amount of time. <br>
  <br>
  An example of O(1) is:

  ```cs
  private int ReturnFirstElementInArray(int[] arr)
  {
    return arr[0];
  }
  ```
</details>

