# Leetcode Notes

## About
<p>I will be updating this readme with my notes on different Leetcode problems and data structures. As well as working on my personal projects, I know that it's important to have a strong grasp on data structures and leetcode style interview questions when I'm applying for different companies. </p>

<p>I've been working on Leetcode problems sporadically throughout the last few months, but now I want to make sure that I fully understand each problem and the steps I'll need to take for a solution. By writing down the notes I take whilst completing a problem, I can quickly look back if I encounter the same problem or something similar, and the process of revision will help cement the problem solving process to common data structure problems in my mind and make me a better programmer overall.</p>

## Top 150 Interview Questions - Easy

### 21. Merge Two Sorted Lists

  1.	Create new linked list, initialize to 0
  2.	Create variable to keep track of current node
  3.	While list 1 and list 2 aren’t empty, loop through all their nodes
  4.	If list 1 value is lower than list 2, add it to the new node. Vice versa. Then increment current
  5.	If they’re both the same, add the node from list 1, then add the node from list 2. Bring current forward an extra node to compensate.
  6.	Add remaining nodes from whichever list is left.
  7.	Return new linked list.

### 26. Remove Duplicates from Sorted Array

This problem wants you to remove duplicates from a sorted array without creating any new arrays, or removing and adding elements, so elements will have to be overwritten. After the function edits the array, it wants to return how long the list of sorted elements with no duplicates are (k). This is because editing the array without removing/adding elements, but keep it the same size, will end up with lots of duplicates at the end of the array. It asks for k because it wants to only know the list of sorted numbers and doesn’t care about any at the end.

1.	Create pointer (i) which will keep track of the correct order in the list (each unique element).
2.	Create pointer (j) which loops through every item in the array (including duplicates).
3.	When J reaches an item that isn’t a duplicate (different from i), increment i by 1 and change it to the value of J. For example. If array is [1, 2], it will just change 2 to 2, but if it’s [1, 1, 2], it will change the 1 to a 2. Since it doesn’t matter what’s at the end of the array that’s fine.
4.	Return i + 1. It wants to return the number of unique elements. This will be i + 1 because i only tracks unique elements, and we add +1 because it started at 0 since arrays are initialised at 0.

### 28. Find the Index of the First Occurrence in a String

Easy solution: return haystack.indexOf(needle);

Solution:
1.	Loop through each letter in the haystack. (only until haystack.length – needle.length because the first letter can’t occur later than the length of the haystack – length of the needle.)
2.	Loop through each letter of the needle (nested for loop with counter defined out of scope).
3.	If the letter in the needle is not the same as the letter in the haystack, break out of the inner for loop. – To do this we need to compare the index of haystack + index of needle, because at the start it will be 0, but then it needs to check if the second letter is the same.
4.	Create an if statement that checks if j = needle.length. If so, that means the complete word is in the haystack, and then return the index of the haystack.
5.	If none of that happens, return -1 as a default.

### 69. Sqrt(x)

Description

<p>Given a non-negative integer x, return the square root of x rounded down to the nearest integer. The returned integer should be non-negative as well. You must not use any built-in exponent function or operator. For example, do not use pow(x, 0.5) in c++ or x ** 0.5 in python.</p>
 
Binary search method:

1. Create 2 variables, left and right to show the lower and upper bounds of the search range. 
2. Create a while loop that finishes when left is higher than right.
3. Create variable that will calculate the midpoint using (left + right) / 2).
4. Calculate the square of the midpoint.
5. Compare square to x. 
6. If square is equal to x, that means the midpoint is the square root, so return mid.
7. Else, adjust search range. If square of midpoint os less than x, we know that the sqrt is to the right of the midpoint, so update left to mid + 1 to narrow range to right half.
8. If square of midpoint is more than x, the sqrt is to the left of the midpoint, so update right to mid - 1 to narrow search range to left half.
9. Once left is higher than right, return left - 1 at the end for the result.

Code:

    let left = 0;
    let right = x;

    while (left <= right) {
        let mid = Math.floor((left + right)/2);
        let square = mid * mid;

        if (square === x) {
            return mid;
        } else if (square < x){
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return left - 1;

After looking through other answers, there's also this pretty simple method:

    if (x === 0 || x === 1) {
        return x;
    }
   
    for (let i = 0; i <= x; i++) {
        if (i * i > x) {
            return i - 1
        }
    }

### 70. Climbing Stairs

You are climbing a staircase. It takes n steps to reach the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

My first solution:

1. Think about the base cases. There's only one way to climb 0 or 1 steps, so initialise an array to store all the different ways, with 1 and 1 as the first 2 vaules.
2. Loop through all remaining ways until n, starting from 2 since 0 and 1 positions are already there.
3. The number of steps it will take to reach i is the number of steps it took to reach i - 1 + number of steps it took to reach i - 2. This is because from i-1 and i-2, you can either take 1 or 2 steps to get there. Add the value of i-1 + i-2.
4. Return the n'th position in the array.

Code: 
```
 var climbStairs = function(n) {
    
    let arr = [1, 1];

    for (let i = 2; i <= n; i++){
        arr.push(arr[i-1] + arr[i-2])
    }

    return arr[n];

  };
  ```
Optimised solution:
  
When I submitted my answer, I saw that memory was high compared with other solutions. I realised it's possible to have the solution without creating an array, and just storing the previous value and the value before in variables as only the end value matters. This uses O(1) instead of O(n).

1. Declare variables for the previous step and the step before, both initialised to 1.
2. Loop from 2 to n.
3. Define a variable to store the value of the current step, which will be assigned to value of previous + step before previous.
4. Increment the i-2 variable to i-1.
5. Increment the previous step to current.
5. Return previous step (since it's assigned to value of current at end of the for loop).

Code:
  ```
  var climbStairs = function(n) {
    
    let backstep = 1;
    let doubleBackstep = 1;

    for (let i = 2; i <= n; i++){
        let currentStep = backstep + doubleBackstep;
        doubleBackstep = backstep;
        backstep = currentStep;
    }

    return backstep;
  };
```

### 88. Merge Sorted Array

Description:

You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

My First Solution:

1. Make copy of nums 1.
2. Loop through each element of the arrays whilst i and j are less than m and n.
3. Add remaining elements of the array if there's any left over once one is finished.

Notes:

This is very similar to question 21. Merge 2 sorted lists. I did a solution in a similar way, with a few variations to cater for the different data structure.

Code:
```
var merge = function(nums1, m, nums2, n) {
    
    let nums1Clone = [...nums1];

    let i = 0;
    let j = 0;
    let x = 0;

    while (i < m && j < n) {

        if (nums1Clone[i] > nums2[j]){
            nums1[x] = nums2[j];
            j++;
            x++;
        } else if (nums1Clone[i] < nums2[j]){
            nums1[x] = nums1Clone[i];
            i++;
            x++;
        } else {
            nums1[x] = nums1Clone[i];
            i++;
            x++;
            nums1[x] = nums2[j];
            j++;
            x++;
        }

    }

    while (i < m) {
        nums1[x] = nums1Clone[i];
        i++;
        x++;
    }

    while (j < n) {
        nums1[x] = nums2[j];
        j++;
        x++;
    }

};
```

On reflection, an optimisation could be remoing the need to clone nums1. I clone it in my solution so that numbers that need to be included aren't overwritten, however by iterating backwards through the arrays, it will start by filling the 0's. Even if all the nums2 numbers are bigger than nums1, it won't overwrite anything since there are n 0's at the end of nums1.

```
let i = m - 1;
    let j = n - 1;
    let x = m + n - 1;

    while (i >= 0 && j >= 0) {
        if (nums1[i] > nums2[j]) {
            nums1[x] = nums1[i];
            i--;
        } else {
            nums1[x] = nums2[j];
            j--;
        }
        x--;
    }

    while (j >= 0) {
        nums1[x] = nums2[j];
        j--;
        x--;
    }
```
### 94. Binary Tree Inorder Traversal

Description:

Given the root of a binary tree, return the inorder traversal of its nodes' values.

Recursive approach:

1. Create array to contain result.
2. Define a function to traverse the tree, defining (node) as a paremeter.
3. If a node isn't null, call the function recursively, for the left node. This is because it's getting the values in order, and the leftmost will be the lowest.
4. Once the leftmost item has been called, it's left value will be null, so push the current node's value to the array.
5. Then, call the function for the right node. This will look for the right node on the leftmost node, and then work it's way up.
6. Call the traverse function, passing root as an argument.
7. Return the resulting array.

Code: 
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
var inorderTraversal = function(root) {
   let result = [];

   function traverse(node) {
       if (node) {
           traverse(node.left);
           result.push(node.val);
           traverse(node.right);
       }
   }

   traverse(root);
   return result;
};
```
You can also use an iterative approach:

1. Create array to contain the result.
2. Create a stack that will be used throughout the code.
3. Define node as root node.
4. Create while loop that keeps looping as long as node isn't null or if the stack contains any elements.
5. Create another while loop inside that loops as long as node isn't null, that pushes the current node and then assigns node to node.left. This will make it so the smallest node is on top of the stack.
6. When there aren't any more leftmost nodes, change node to the top element on the stack by popping the top item.
7. Push this to the result array.
8. Change node to node.right. The outer while loop will reset and then it will push that to the stack, and then look at it's left node. This will continue until all nodes have been iterated through.
9. Return result array.

```
    let result = [];
    let stack = [];
    let node = root;
    
    while (node || stack.length > 0) {
        while (node) {
            stack.push(node);
            node = node.left;
        }
        node = stack.pop();
        result.push(node.val);
        node = node.right;
    }
    
    return result;
```

### 101. Symmetric Tree

Description:

Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

Solution (recursive):

1. Create a function that will traverse the left side and right side of the tree, taking in node1 and node2 as parameters.
2. Check base cases. For example if both sides are null, they are symmetric, if one node is null and other isn't, or if the values of node1 and node2 are different then they are not symmetric, so return false.
3. When returning the additional function, check if the left and right of each node is the same. e.g. comparing node1 left and node2 right and comparing node1 right and node2 left.
4. Return the function and pass the left and right node of the root.

Code:

```
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isSymmetric = function(root) {
   
    function isMirror(node1, node2) {

        if (!node1 && !node2) {
            return true;
        }

        if (!node1 || !node2) {
            return false;
        }
        if (node1.val !== node2.val) {
            return false;
        }
        ```
        return isMirror(node1.left, node2.right) && isMirror(node1.right, node2.left);
    }
    
    return isMirror(root.left, root.right);
};
```

### 104. Maximum Depth of Binary Tree

Given the root of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Solution:

1. Define variable to contain maximum depth. Initialise to 0.
2. Define recursive function that will traverse left and right nodes, passing node and current depth as parameters.
3. (First check that node isn't null) If there is a left node, call the traverse function, passing node.left and currentDepth + 1 as a parameter.
4. If there's a right node, do the same.
5. If the left node and right node aren't null (if there's no nodes left on left or right), update depth to the max of depth and current depth.
6. Call the traverse function for first time, passing in root and 1 as default values.
7. Return depth variable for answer.

Code:


```
var maxDepth = function(root) {
    
    let depth = 0;
    function traverse(node, currentDepth){
        if (node){
          if (node.left) {
          traverse(node.left, currentDepth + 1);
          }
          if (node.right) {
              traverse(node.right, currentDepth + 1);
          }

          if (!node.left && !node.right){
              depth = Math.max(depth, currentDepth);
          }
        }
    }

    traverse(root, 1);

return depth;

};
```
There's also a simpler solution:

```
if (root === null) {
        return 0;
    }
    else {
        let leftDepth = maxDepth(root.left);
        let rightDepth = maxDepth(root.right);
        return 1 + Math.max(leftDepth, rightDepth);
    }
```
### 108. Convert Sorted Array to Binary Search Tree

Problem:

Given an integer array nums where the elements are sorted in ascending order, convert it to a 
height-balanced binary search tree.

Solution:

1. The function will be called recursively. Aim is to find the midpoint of array, then define that as a node, and then go to each left and right node until both are null. Therefore, start with the end case, which will be if there are no elements left in the array (or sub-array), return null. 
2. Define midpoint as the middle of the array (or sub-array).
3. Define the current node (which could be the root or a leaf on the tree) as the value in the array of the current midpoint.
4. Make the function recur for the left node, but using the first half of the array (using slice).
5. Do the same fir the right node.
6. Return the head node.

Code:

```
var sortedArrayToBST = function(nums) {
    
  if (nums.length === 0) {
    return null;
  }

  const mid = Math.floor(nums.length / 2);
  const node = new TreeNode(nums[mid]);

  node.left = sortedArrayToBST(nums.slice(0, mid));
  node.right = sortedArrayToBST(nums.slice(mid + 1));

  return node;
};
```
#118. Pascal's Triangle

Problem:

Given an integer numRows, return the first numRows of Pascal's triangle. In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

![PascalTriangleAnimated2](https://user-images.githubusercontent.com/66835665/230636824-4818a993-60ce-4dac-be19-30bfd623ca37.gif)

Solution I got first try:

1. Create a matrix to contain the arrays which will act as the rows of the triangle. 
2. Initialise the matrix to [1] for the base case that rows = 1. If rows = 1, return the matrix as is.
3. Update matric to [1], [1, 1] for the case that rows = 2 and return the matrix if it's 2.
4. Create a for loop that cycles through all the rows from row 3 to max row.
5. Create an array that will reset every loop, and contain the numbers for each row.
6. Push 1 to the start, since every row will start with 1.
7. Create for loop nested inside that cycles through the length of the previous entry in the matrix. (Starting from index 1 since index 0 is already 1.
8. Inside this loop, push the value of the previous index + current index.
9. Push 1 at the end to finish the row.
10. Push the array to the matrix.
11. Return the matrix.

Code:
```
var generate = function(numRows) {
    let matrix = [[1]];
    if (numRows === 1) {
        return matrix;
    }
    matrix = [[1], [1, 1]];
    if (numRows === 2) {
        return matrix;
    }

    for (let i = 2; i < numRows; i++){
        let arr = [];
        arr.push(1);
      for (let j = 1; j < matrix[i-1].length; j++){
          arr.push(matrix[i-1][j-1] + matrix[i-1][j]);
       }
       arr.push(1);
       matrix.push(arr);
    }
    
 return matrix;
};
```

### 141. Linked List Cycle

Since I found this problem by searching by hash map, I thought the best solution would be hash map and came up this this solution:

1. Create new hash map.
2. Define a pointer starting at the head of the linked list.
3. Loop through all nodes in the linked list.
4. Check if map contains the current node's pointer (current.next). If so, return true.
5. Add the node to the hash table, using it's pointer as the key and true as the value, since the value doesn't really matter.
6. Set the current node to the next node.
7. Return false at the end, since if it made it through the while loop, it won't contain any cycles.

Code:
    
    const map = new Map([])

    let current = head;

    while (current !== null) {
        if (map.has(current.next)){
            return true;
        }
        map.set(current.next, true);
        current = current.next;
    }

    return false;

However, I now realise that you can also do this with hare and tortoise method, and it's much better for memory:

1. Define a slow pointer and fast pointer, initialise both to the head.
2. Loop through all nodes in the linked list by checking if the fast pointer and the next value of fast's pointer aren't null.
3. For each loop, increment slow by 1 and fast by 2.
4. If at any point slow and fast are the same, return true. Because if there is a cycle, slow and fast will meet.
5. Return false if there are no cycles.

Code:
    
    let slow = head;
    let fast = head;
    
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (slow === fast) {
            return true;
        }
    }
    
    return false;
