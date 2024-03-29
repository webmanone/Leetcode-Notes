# Leetcode Notes

## About
<p>I will be updating this readme with my notes on different Leetcode problems and data structures. As well as working on my personal projects, I know that it's important to have a strong grasp on data structures and leetcode style interview questions when I'm applying for different companies. </p>

<p>I've been working on Leetcode problems sporadically throughout the last few months, but now I want to make sure that I fully understand each problem and the steps I'll need to take for a solution. By writing down the notes I take whilst completing a problem, I can quickly look back if I encounter the same problem or something similar, and the process of revision will help cement the problem solving process to common data structure problems in my mind and make me a better programmer overall.</p>

## Top 150 Interview Questions - Easy

### 1. Two Sum

Description:

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target. You may assume that each input would have exactly one solution, and you may not use the same element twice. You can return the answer in any order.

Solution:

The easy way would be to have 2 nested for loops, comparing avery possible pair, then returning the indexes as the answer. The best part about this solution is it has O(1) space complexity, however becasue of the nested for loops it has O(n^2) time complexity:

Code:
```
var twoSum = function(nums, target) {
let sum;

for (let i = 0; i < nums.length; i++){
    for (let j = i + 1; j < nums.length; j++){
        sum = nums[i] + nums[j];
        if (sum === target) {
            return [i, j];
        }
    }
}
};
```

However, this isn't very time efficient at O(n^2). A better way that has time & space complexity O(n) would be:
1. Create a hash map.
2. Loop through elements of the array.
3. Do (target - current) to find what the remainder would be.
4. If the remainder is already in the hash map, we have found the solution.
5. Return the index of the element in the hash map and current index.
6. If the remainder isn't in the hash map, add the element's value and index to the hash map.

Code:
```
var twoSum = function(nums, target) {
    const hash = new Map();
    for (let i = 0; i < nums.length; i++){
        const remainder = target - nums[i];
        if (hash.has(remainder)){
            return [hash.get(remainder), i];
        }
        hash.set(nums[i], i);
    }
};
```
### 13. Roman to Integer

Description:

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000

For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II. Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used: I can be placed before V (5) and X (10) to make 4 and 9. X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900. Given a roman numeral, convert it to an integer.

Solution:

1. Store the key/value pairs in a hash map.
2. Declare a result variable to hold the result.
3. Look through each letter in the string.
4. Declare variables for current value and next value of the string in the hash map.
5. If the current value is less than next, subtract the value from the result, else add the value to the result.
6. Return result.

Code:
```
var romanToInt = function(s) {
   const numerals = {
     "I":1,
     "V":5,
     "X":10,
     "L":50,
     "C":100,
     "D":500,
     "M":1000
};

let result = 0;

for (let i = 0; i < s.length; i++){
  let current = numerals[s[i]];
  let next = numerals[s[i+1]];

  if (current < next){
    result -= current;
  } else {
    result += current;
  }
}
return result;
}
```
### 14. Longest Common Prefix

Description:
Write a function to find the longest common prefix string amongst an array of strings. If there is no common prefix, return an empty string "".

Solution (O(nm), O(1)):
1. Define a variable to contain the first word, which will be used as the prefix we are comparing.
2. Loop through all the other words.
3. Create a while loop that runs as long as prefix isn't found in the current word. Can do this by using the indexOf() method if prefix isn't found at 0.
4. If current prefix isn't found, remove the last letter of prefix by creating a substring.
5. If the prefix is empty, return the empty string.
6. Return prefix at the end.

Code:
```
var longestCommonPrefix = function(strs) {

  let prefix = strs[0];

  for (let i = 1; i < strs.length; i++){
      while (strs[i].indexOf(prefix) !== 0){
          prefix = prefix.substring(0, prefix.length - 1);
          if (prefix === ""){
              return "";
          }
      }
  }
  return prefix;
};
```
### 20. Valid Parentheses

Description: Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
An input string is valid if: 
Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.

Solution:
1. Create a stack.
2. Loop through all characters in the string.
3. If the character is an opening bracket, push it to the stack.
4. If it's a closing bracket, check that the last element in the stack is it's corresponding opening bracket.
5. If it is, pop the element from the stack. Else, return false.
6. After all elements have been checked, if the stack still has any remaining characters, return false. Else return true.

Code:
(Time: O(n), Space: O(n))
```
var isValid = function(s) {
    
    const stack = [];

    for (let i = 0; i < s.length; i++){
        if (s[i] === "(" || s[i] === "{" || s[i] === "["){
            stack.push(s[i]);
        } else if (s[i] === ")"){
            if (stack[stack.length - 1] === "("){
                stack.pop();
            } else {
                return false;
            }
        } else if (s[i] === "]"){
            if (stack[stack.length - 1] === "["){
                stack.pop();
            } else {
                return false;
            }
        } else if (s[i] === "}"){
            if (stack[stack.length - 1] === "{"){
                stack.pop();
            } else {
                return false;
            }
        }
    }
    if (stack.length > 0){
        return false;
    }
    return true;
};
```
### 21. Merge Two Sorted Lists

  1.	Check if either list1 or list 2 are null, return the other if so.
  2.	Create a pointer for the head node.
  3.    Check which list value is smaller, if it's list 1 assign it to list1 and then increment list1 to list1.next. Vice versa if list 2.
  4.    Set a current pointer to the head node.
  5.    Loop while list 1 and list 2 aren't null.
  6.    If list 1 value is smaller, change current.next to list1, move list1 to next, vice versa for list 2. Don't need one if they're the same, as list2 will be added automatically.
  7.    Move current to current.next to continue at the end of the loop.
  8.    Set current.next to either of the lists if there's any ramaining nodes after the while loop.
  9.    Return head.

Code: 
(Time O(n + m), Space O(1))
```
var mergeTwoLists = function(list1, list2) {
    if (!list1){
        return list2;
    }
    if (!list2){
        return list1;
    }
    
    let head;

    if (list1.val < list2.val){
        head = list1;
        list1 = list1.next;
    }
    else {
        head = list2;
        list2 = list2.next;
    }

    let current = head;

    while (list1 && list2){
        if (list1.val < list2.val){
            current.next = list1;
            list1 = list1.next;
        } else {
            current.next = list2;
            list2 = list2.next;
        }
        current = current.next;
    }

    current.next = list1 || list2;

    return head;

};
```
There's also a recursive approach to this problem:
```
var mergeTwoLists = function(list1, list2) {
    if (!list1) {
    return list2;
  }
  if (!list2) {
    return list1;
  }

  if (list1.val < list2.val) {
    list1.next = mergeTwoLists(list1.next, list2);
    return list1;
  } else {
    list2.next = mergeTwoLists(list1, list2.next);
    return list2;
  }
};
```
### 26. Remove Duplicates from Sorted Array
Description:
Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in nums.

Consider the number of unique elements of nums to be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the unique elements in the order they were present in nums initially. The remaining elements of nums are not important as well as the size of nums.
Return k.

Solution:

This problem wants you to remove duplicates from a sorted array without creating any new arrays, or removing and adding elements, so elements will have to be overwritten. After the function edits the array, it wants to return how long the list of sorted elements with no duplicates are (k). This is because editing the array without removing/adding elements, but keep it the same size, will end up with lots of duplicates at the end of the array. It asks for k because it wants to only know the list of sorted numbers and doesn’t care about any at the end.

1.	Create pointer (i) which will keep track of the correct order in the list (each unique element).
2.	Create pointer (j) which loops through every item in the array (including duplicates).
3.	When J reaches an item that isn’t a duplicate (different from i), increment i by 1 and change it to the value of J. For example. If array is [1, 2], it will just change 2 to 2, but if it’s [1, 1, 2], it will change the 1 to a 2. Since it doesn’t matter what’s at the end of the array that’s fine.
4.	Return i + 1. It wants to return the number of unique elements. This will be i + 1 because i only tracks unique elements, and we add +1 because it started at 0 since arrays are initialised at 0.

Code:
(O(n), O(1))
```
var removeDuplicates = function(nums) {
    
let i = 0;

for (let j = 1; j < nums.length; j++){
  if (nums[i] !== nums[j]){
    i++;
    nums[i] = nums[j];
  }
}
return i + 1;
};
```

### 28. Find the Index of the First Occurrence in a String

Description
Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack. 

Solution:

Easy + most efficient solution:
```
return haystack.indexOf(needle);
```
More algorithmic solution:
1.	Loop through each letter in the haystack. (only until haystack.length – needle.length because the first letter can’t occur later than the length of the haystack – length of the needle.)
2.	Loop through each letter of the needle (nested for loop with counter defined out of scope).
3.	If the letter in the needle is not the same as the letter in the haystack, break out of the inner for loop. – To do this we need to compare the index of haystack + index of needle, because at the start it will be 0, but then it needs to check if the second letter is the same.
4.	Create an if statement that checks if j = needle.length. If so, that means the complete word is in the haystack, and then return the index of the haystack.
5.	If none of that happens, return -1 as a default.

```
var strStr = function(haystack, needle) {
  
    for (let h = 0; h <= haystack.length - needle.length; h++) {
      for (let n = 0; n < needle.length; n++){
        if (haystack[h + n] !== needle[n]){
          break;
        }
      }
      if (n === needle.length) {
        return h;
      }
    }
return -1;
};
```
### 66. Plus One

Description:
You are given a large integer represented as an integer array digits, where each digits[i] is the ith digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading 0's. Increment the large integer by one and return the resulting array of digits.

Solution:

1. Loop through the array backwards.
2. If the digit is 9 and the previous value doesn't exist, push a 0 to the array and change the current digit to 1. (For example, if the array is a [9], it needs to be changed to [1, 0]. Return the array.
3. Else, if it's a 9, change it to 0.
4. Else, increment the current digit and return the array.

Code:
```
var plusOne = function(digits) {
        for (let i = digits.length - 1; i >= 0; i--){
            if (digits[i] === 9 && !digits[i - 1]){
               digits.push(0);
               digits[i] = 1;
               return digits;
            }
            else if (digits[i] === 9){
                     digits[i] = 0;
            } else {
                digits[i]++;
                return digits;
            }
        }
};
```
The first condition in this can also be removed if we're simplifying the solution using unshift. The else statement returns the array but the first if statement doesn't. Therefore, the unshift will only happen if all digits were a 9:

```
var plusOne = function(digits) {
    for (let i = digits.length - 1; i >= 0; i--) {
        if (digits[i] === 9) {
            digits[i] = 0;
        } else {
            digits[i]++;
            return digits;
        }
    }
    digits.unshift(1);
    return digits;
};
```
You can also do it by converting the type, but this isn't very efficient:
```
let x = BigInt(digits.join(""));
        x += 1n;
        x = x.toString();
        let arr = x.split("");
    return arr;
```
### 69. Sqrt(x)

Description

Given a non-negative integer x, return the square root of x rounded down to the nearest integer. The returned integer should be non-negative as well. You must not use any built-in exponent function or operator. For example, do not use pow(x, 0.5) in c++ or x ** 0.5 in python.
 
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

Description:

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
### 118. Pascal's Triangle

Problem:

Given an integer numRows, return the first numRows of Pascal's triangle. In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

![PascalTriangleAnimated2](https://user-images.githubusercontent.com/66835665/230636824-4818a993-60ce-4dac-be19-30bfd623ca37.gif)

Solution I got first try:

1. Create a matrix to contain the arrays which will act as the rows of the triangle. 
2. Initialise the matrix to [1] for the base case that rows = 1. If rows = 1, return the matrix as is.
3. Update matrix to [1], [1, 1] for the case that rows = 2 and return the matrix if it's 2.
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
### 121. Best Time to Buy and Sell Stock

Problem:
You are given an array prices where prices[i] is the price of a given stock on the ith day. You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock. Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

Solution:

In my first attempt, I came up with this solution. It works, however it has a time complexity of O(n^2) as it compares every pair of prices, making it inefficient for large input:

```
var maxProfit = function(prices) {
    
    let profit = 0;

    for (let i = prices.length - 1; i > 0; i--) {
       for (let j = i - 1; j > -1; j--){
           if (prices[i] < prices[j]){
               continue;
           }
           let calculation = prices[i] - prices[j];
           if (calculation > profit){
           profit = calculation;
           }
       }
    }

return profit;

};
```

A more efficient solution would be to keep track of the lowest price, and then compare it against future values and keep track of maximum profit:

1. Define variables to contain the minimum price and maximum profit.
2. Loop through all elements in the array.
3. If the price at current index is lower than the minimum price, update minimum price to hold that value.
4. Else, if it's larger, define a variable to keep track of that profit.
5. If the profit is bigger than the maximum profit, assign that value to max profit.
6. Return the maximum profit.

Code:
```
var maxProfit = function(prices) {
    
    let minPrice = Infinity;
    let maxProfit = 0;

    for (let i = 0; i < prices.length; i++) {
        if (prices[i] < minPrice) {
            minPrice = prices[i];
        } else {
            let thisProfit = prices[i] - minPrice;
            if (thisProfit > maxProfit){
                maxProfit = thisProfit;
            }
        }
    }
    return maxProfit;
    
};
```

### 125. Valid Palindrome

Problem:

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers. Given a string s, return true if it is a palindrome, or false otherwise.

My first solution:

1. Convert the string to lowercase and remove non-alphanumeric characters. Can use the toLowerCase() and replace() functions.
2. Create an array of the characters in the clean string using split() function.
3. Reverse array using reverse() function.
4. Store reversed array as a string in a variable using join() function.
5. Return if the clean string is the same as reversed string.

Code:
```
var isPalindrome = function(s) {
    const sClean = s.toLowerCase().replace(/[^a-z0-9]/g, '');
    
    let arr = sClean.split('');
    arr.reverse();
    const reversed = arr.join('');
    
    return sClean === reversed;
};
```
Time = O(n), Space = O(n).

This solution is simple and works well, but the 2 pointer solution is most efficient since it has space complexity of O(1).

2 pointer solution:

1. Convert string to a clean version using toLowerCase() and replace() functions.
2. Define 2 pointers, left and right for the start and end of the string.
3. Loop through the string, comparing the left and right pointers while left is smaller than right (when they meet in the middle).
4. Return false if letter at left index in the string doesn't equal right index.
5. Increment left, decrement right.
6. Return true.

Code:

```
var isPalindrome = function(s) {
    const sClean = s.toLowerCase().replace(/[^a-z0-9]/g, '');
    
    let left = 0;
    let right = sClean.length - 1;

    while (left < right){
        if (sClean[left] !== sClean[right]) {
            return false;
        }
        left++;
        right--;
    }
    
    return true;
};
```
### 136. Single Number

Description:

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one. You must implement a solution with a linear runtime complexity and use only constant extra space.

Solution:

1. Set result variable to 0.
2. Create a for loop that loops through all numbers in the array.
3. Use XOR operator(^) to compare result to the number at current iteration, and assign the result of that to the result variable.
4. Return result.

This works because when 2 binary numbers are compared with XOR, if they are not the same, a different number is produced. If the same number appears again in the array, it will cancel out the binary number. Therefore, the only number left will be the single number in the array at the end.

Code:
```
var singleNumber = function(nums) {

    let result = 0;

    for (let i = 0; i < nums.length; i++) {
        result ^= nums[i];
    }
    
    return result;
};
```
### 141. Linked List Cycle
Description:
Given head, the head of a linked list, determine if the linked list has a cycle in it. There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter. Return true if there is a cycle in the linked list. Otherwise, return false.

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
```    
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
```

### 160. Intersection of Two Linked Lists

Problem:

Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.

For example, the following two linked lists begin to intersect at node c1:
![160_statement](https://user-images.githubusercontent.com/66835665/230890978-addad37c-d5b5-4f91-9aa8-eadfb6fb46e5.png)

My first solution:

1. Define an array to store the nodes visited by the first linked list.
2. Define a pointer and initialise it to headA.
3. Loop through all nodes of the first linked list and push each node to the array.
4. Change current to head of the second linked list.
5. Loop though all nodes, if the array includes current, return that, else go to the next node
6. Return null if nothing has been found.

Code:

```
var getIntersectionNode = function(headA, headB) {
    
    let nodes = [];

    let current = headA;

    while (current){
        nodes.push(current);
        current = current.next;
    }

    current = headB;

    while (current){
        if (nodes.includes(current)) {
            return current;
        } else {
            current = current.next;
        }
    }

return null;

};
```

However, this isn't as memory efficient as it could be. A more efficient solution would be:

1. Initialise 2 pointers to point to head A and head B.
2. While pointer A doesn't equal pointer B, loop through nodes.
3. If pointer A isn't null, move it to the next node.
4. If it is null, change it to head B so it loops through the second list. (They'll eventually meet if they intersect, even if one is shorter because by looping through the other list, they will catch up at the intersection.
5. Do the same for pointer B.
6. Return pointer A. This is because if they are the same, the loop will exit so you could return either A or B. If they on't intersect, A will have reached the end of head B and will be null.

Code:
```
    var getIntersectionNode = function(headA, headB) {
    
    let pA = headA;
    let pB = headB;

    while (pA !== pB) {
        if (pA !== null) {
            pA = pA.next;
        } else {
            pA = headB;
        }

        if (pB !== null) {
            pB = pB.next;
        } else {
            pB = headA;
        }
    }

    return pA;

};
```
This can also be written more consicely with ternary operators:
```
var getIntersectionNode = function(headA, headB) {
    
    let pA = headA;
    let pB = headB;

    while (pA !== pB) {
        pA = pA ? pA.next : headB;
        pB = pB ? pB.next : headA;
    }

    return pA;
};
```
### 163. Missing Ranges
PREMIUM

### 169. Majority Element

Problem:
Given an array nums of size n, return the majority element. The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

My first solution:
1. Declare variable for midpoint of the length of the array.
2. Set a counter and a variable to track the position which element is going to be tested to 0.
3. Set a variable to the current element being tested e.g. (nums[position]).
4. Create a for loop that loops from start to finish of array.
5. If nums[i] is the same as the current element we are testing, increment counter by 1.
6. If counter gets higher than mid, return element.
7. Else, if i gets to the end of the array, reset the counter, change the position in the array we're testing by 1 and initialise i back to 0.

Code:
```
var majorityElement = function(nums) {
    
    const mid = nums.length/2;
    let counter = 0;
    let init = 0;
    let element = nums[init];
    
    for (let i = 0; i < nums.length; i++){
        if (nums[i] === element){
            counter++;
        }
        if (counter > mid){
            return element;
        } else if (i === nums.length - 1){
            counter = 0;
            init++;
            element = nums[init];
            i = 0;
        }
    }
    
};
```
It works but is horribly inefficient for time since it uses a nested for loop using init.

A more efficient solution would be:

1. Set midpoint to half of array length.
2. Set counter to 0;
3. Set element we are testing to null.
4. Create for loop that loops through all elements in the array.
5. Create variable num that will store current number.
6. If the counter is 0, set element to num.
7. If num = element, increment counter, else decrement counter.
8. Return the element.

This works better by removing the nested loop, and incrementing and decrementing the counter, and defining the current element within the loop. By incrementing and decrementing the counter when an element appears or doesn't appear, I don't even need to put a check in to see if the counter is higher than mid. This is because if an element isn't majority, it will eventually go back to 0, which will reset the chosen element to the current number in the array. If an element is majority, it won't reset back to 0 and will be output when the for loop finishes.

Note: I can only skip this step because a majority is guaranteed.

Code:

```
const mid = nums.length/2;
    let counter = 0;
    let element = null;
    
    for (let i = 0; i < nums.length; i++){
        let num = nums[i];
        if (counter === 0){
            element = num;
        }
        if (num === element) {
            counter++;
        } else{
            counter--;
        }
    }
    return element;
};
```

### 171. Excel Sheet column Number

Problem: Given a string columnTitle that represents the column title as appears in an Excel sheet, return its corresponding column number.

Solution:
1. Create a hash map containing vaules e.g. ["A", 1], ["B", 2].
2. Initialise variable total to 0.
3. Initialise variable n to length of column title.
4. Loop through each char in column title.
5. Declare variable to get the value of the letter in column title, starting at the end (lowest) which will be n - i - 1.
6. Add the letter value of lettervalue * Math.pow(26, i) to the total.
7. Return total.

Code:
```
var titleToNumber = function(columnTitle) {
    const map = new Map([
        ["A", 1],
        ["B", 2],
        ["C", 3],
        ["D", 4],
        ["E", 5],
        ["F", 6],
        ["G", 7],
        ["H", 8],
        ["I", 9],
        ["J", 10],
        ["K", 11],
        ["L", 12],
        ["M", 13],
        ["N", 14],
        ["O", 15],
        ["P", 16],
        ["Q", 17],
        ["R", 18],
        ["S", 19],
        ["T", 20],
        ["U", 21],
        ["V", 22],
        ["W", 23],
        ["X", 24],
        ["Y", 25],
        ["Z", 26],
    ]);

    let total = 0;
    let n = columnTitle.length;

    for (let i = 0; i < n; i++){
        let letterValue = map.get(columnTitle.charAt(n - i - 1));
        total += letterValue * Math.pow(26, i);
    }

    return total;

};
```
### 190. Reverse Bits

Problem: Reverse bits of a given 32 bits unsigned integer.

Solution:
1. Define a variable result and set it to 0. This will contain the reversed integer.
2. Create a for loop to loop through all 32 bits.
3. Shift result to the left with << 1.
4. Use an OR operation on result: result | (n&1).
5. Shift n to the right.
6. Return result, converting it to an unsigned integer using >>>.

Code:
```
var reverseBits = function(n) {
    let result = 0;
    for (let i = 0; i < 32; i++) {
        result = result << 1;
        result = result | (n & 1);
        n = n >> 1;
    }
  return result >>> 0; 
};
```
### 191. Number of 1 Bits

Problem:
Write a function that takes the binary representation of an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).

Solution:
1. Define a count variable to keep track of the number of 1 bits.
2. Create a while loop that continues as long as n doesn't equal 0.
3. Define a variable isOne that we will later use to compare if the bit is 1. This uses the and operator, n & 1, which will look at the bit at the end of n and return true if it's 1.
4. If isOne and 1 are the same, increase count.
5. Shift n to compare the next end bit using n = n >>> 1.
6. Return count.

Code:
```
var hammingWeight = function(n) {
    let count = 0;

    while (n !== 0){
      const isOne = n & 1;
      if (isOne === 1){
        count++;
      }
      n = n >>> 1;
    }
    
    return count;
};
```
### 202. Happy Number

Problem:

Write an algorithm to determine if a number n is happy.A happy number is a number defined by the following process:
Starting with any positive integer, replace the number by the sum of the squares of its digits. Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy. Return true if n is a happy number, and false if not.

My first solution:

1. Define an array of results so that the results of squaring all the digits can be stored for future reference.
2. The solution will require recursion, so define another function that will recur.
3. Declare an array of digits that changes the int to a string and splits the number into seperate elements in an array.
4. Now the numbers are stored, reset n to 0;
5. Loop through all elements of the array, squaring them and adding them to n.
6. After this, if n is 1, return true.
7. Else, if the results array already includes the number n, return false.
8. Push n into the results array. 
9. Return result of the inner function.
10. Return result of inner function in outer function.

Code:
```
var isHappy = function(n) {
    let results = [];
    var isHappyCheck = function(n) {
        let digits = n.toString().split('');
        n = 0;
       
        for (let i = 0; i < digits.length; i++){
            n += digits[i] * digits[i];
        } 

        if (n === 1){
            return true;
        } else if (results.includes(n)){
            return false;
        }

        results.push(n);
        return isHappyCheck(n);
    };
    return isHappyCheck(n);
};
```
Another solution that would be a bit more efficient would use the hare and tortoise approach:

```
var isHappy = function(n) {
    function sumOfSquares(n) {
        let sum = 0;
        while (n > 0) {
            let digit = n % 10;
            sum += digit * digit;
            n = Math.floor(n / 10);
        }
        return sum;
    }

    let slow = n;
    let fast = sumOfSquares(n);
    while (fast !== 1 && slow !== fast) {
        slow = sumOfSquares(slow);
        fast = sumOfSquares(sumOfSquares(fast));
    }
    return fast === 1;
};
```

### 206. Reverse Linked List

Problem:

Given the head of a singly linked list, reverse the list, and return the reversed list.

Solution:
1. Set 3 pointers: current, previous and next.
2. Set current to head, then previous and next to null.
3. While current is not null, loop through the nodes.
4. First, deal with the next pointer and move it to the next node with current.next.
5. Then, since we now have the next pointer stored, change the current.next pointer to previous, which will initially be null since it will be the new end of the list.
6. Then, move previous up to current.
7. Finally, move current to next.
8. Return previous. Don't return current because it wil have exited the while loop and therefore previous will be set to the current head.

Tip: Remember that in the while loop, the order you need to deal with is dealing with the assigned value each time. E.g. starting with next = current.next, you then do current.next = previous, then previous=...

Code:
```
var reverseList = function(head) {
    let current = head;
    let previous = null;
    let next = null;

    while (current){
        next = current.next;
        current.next = previous;
        previous = current;
        current = next;
    }

    return previous;
};
```
### 217. Contains Duplicate
Problem:

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

My first solution:

1. Create for loop through nums - 1.
2. Create for loop setting j to i+1.
3. If nums[i] === nums[j] return true.
4. Else return false.

Code:
```
var containsDuplicate = function(nums) {
   for (let i = 0; i < nums.length - 1; i++){
        for (let j = i+1; j < nums.length; j++){
            if (nums[i] === nums[j]){
                return true;
            }
        }
   }
    return false;
};
```
It was good memory wise but terrible for time complexity because of the nested for loops.

A more time efficient solution would be to use a hash set, wich is a data structure that stores a collection of unique elements in no particular order (which is faster than an array):

1. Create new hash set.
2. Loop through all elements of the array.
3. If the set has nums[i], return true.
4. Else add nums[i] to the set.
5. Return false when all elements of array are checked.

Code:
```
var containsDuplicate = function(nums) {
    const set = new Set();
    for (let i = 0; i < nums.length; i++) {
        if (set.has(nums[i])) {
            return true;
        }
        set.add(nums[i]);
    }
    return false;
};
```
### 234. Palindrome Linked List
Description: Given the head of a singly linked list, return true if it is a palindrome or false otherwise.

Solution:
1. Use hare and tortoise method to find the middle of the linked list. Define a fast and slow pointer.
2. While fast & slow aren't null, increment slow by one and fast by 2.
3. Where slow finishes will be the centre of the linked list, so define current as slow, and set previous and next pointers to null.
4. Reverse the second half of the linked list by assigning next to current.next, current.next to previous, previous to current, and current to next.
5. Assign 2 pointers to the heads of each half, first will be head and second will be previous as that's where the list ends after reversing.
6. While not null, if the value of p1 doesn't equal value of p2, return false. Then check for the next value.
7. Return true.

Code:
```
var isPalindrome = function(head) {
    let slow = head;
    let fast = head.next;

    while (fast && fast.next){
        slow = slow.next;
        fast = fast.next.next;
    }

    let current = slow;
    let previous = null;
    let next = null;

    while (current){
        next = current.next;
        current.next = previous;
        previous = current;
        current = next;
    }

    let p1 = head;
    let p2 = previous;

    while (p1){
        if (p1.val !== p2.val){
            return false
        }
        p1 = p1.next;
        p2 = p2.next;
    }
    return true;
};
```

### 242. Valid Anagram

Problem:

Given two strings s and t, return true if t is an anagram of s, and false otherwise. An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

My first attempt code:
```
var isAnagram = function(s, t) {
    let arr = s.split("");

    for(let i = 0; i < s.length; i++){
        if (arr.includes(t[i])){
            let index = arr.indexOf(t[i]);
            arr.splice(index, 1);
        }
    }
    
    if (arr.length === 0 && s.length === t.length){
        return true;
    } else {

    return false;
    
    }
};
```
Better solution that has time and space complexity of O(n), which involves creating a hash table then looping through each string:

1. Check if the length of both strings are the same. If so, return false.
2. Create a hash map.
3. Loop through all characters in the s string. 
4. Declare a constant variable of char for the index of the string.
5. If char is in the hash table, set it's value to hash char + 1. Else, set it to 1.
6. Create another for loop to loop through the characters in the second string.
7. Declare a const variable to keep track of the current character.
8. If it doesn't exist in the hash table, return false.
9. Reduce the quantity of the char in the hash table by one.
10. Return true.

Code:
```
var isAnagram = function(s, t) {
    if (s.length !== t.length) {
        return false;
    }
    
    const hash = {};
    
    for (let i = 0; i < s.length; i++) {
        const char = s[i];
        hash[char] = hash[char] + 1 || 1;

    }
    
    for (let i = 0; i < t.length; i++) {
        const char = t[i];
        if (!hash[char]) {
            return false;
        }
        hash[char]--;
    }

    return true;
};
```
### 268. Missing Number

Problem:

Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

There's a lot of ways to do this, but my first solution was:

```
var missingNumber = function(nums) {
    for (let i = 0; i < nums.length; i++){
        if (!nums.includes(i)){
            return i;
        }
    }
    return nums.length;
};
```
This isn't great time efficiency since it uses includes. 

A better solution would be:
1. Create a set of the array. This is more efficient because sets have constant time lookup, meaning that it's quicker to check if an element exists compared to an array.
2. Loop through all the numbers that should be in the array, from 0 to <= nums.length.
3. If the set doesn't have i, return i.

Code:
```
var missingNumber = function(nums) {
    const set = new Set(nums);
    for (let i = 0; i <= nums.length; i++){
        if (!set.has(i)) {
            return i;
        }
    }
};
```
There are also funky solutions like this, using the formula based on the sum of an arithmetic progression. This is slightly more efficient, however it can be confusing to read:

```
function missingNumber(nums) {
    const n = nums.length;
    const expectedSum = (n * (n + 1)) / 2;
    const actualSum = nums.reduce((sum, num) => sum + num, 0);
    return expectedSum - actualSum;
}
```
### 283. Move Zeroes

Problem:

Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements. Note that you must do this in-place without making a copy of the array.

Solution:

Use 2 pointers. One to keep track of where the next non-zero integer should be, and one for where it actually is.

1. Define first pointer to 0. All the 0s are supposed to be at the end of the array, so the first element in the array should definitely be a non-zero if there are any.
2. Loop through all elements in the array with the second pointer (i).
3. If the second pointer reaches an array that isn't 0, we need to bring it to the front of the array. Therefore:
4. Store the element at the index of the first pointer in a temporary variable, since it's being overwritten. 
5. Set the value at the first pointer to the value at the second pointer.
6. Set the value of the second pointer to the value at the first pointer, which is now stored in the temporary variable.
7. Increment the first pointer by one, as there should be a number there.
8. Since it's in-place, no need to return anything.

Code:
```
var moveZeroes = function(nums) {
    let nonZeroIndex = 0;

    for (let i = 0; i < nums.length; i++){
        if (nums[i] !== 0){
            const storage = nums[nonZeroIndex];
            nums[nonZeroIndex] = nums[i];
            nums[i] = storage;
            nonZeroIndex++;
        }
    }
};
```
### 326. Power of 3

Problem:
Given an integer n, return true if it is a power of three. Otherwise, return false. An integer n is a power of three, if there exists an integer x such that n == 3^x.

Solution:

The point is, if n is equal to 3^x, it doesn't matter what x is, if it's true, n will still be a multiple of 3.

1. If n is 0, return false.
2. While n mod 3 is 0 (while n is a multiple of 3), divide n by 3.
3. If n is a multiple of 3, it will eventually return 1 since 3/3 is 1. Therefore, return if n equals 1 or not.

Code:
```
var isPowerOfThree = function(n) {
    if (n === 0) {
    return false;
  }
  while (n % 3 === 0) {
    n /= 3;
  }
  return n === 1;
};
```

### 344. Reverse String

Problem:

Write a function that reverses a string. The input string is given as an array of characters s.

You must do this by modifying the input array in-place with O(1) extra memory.

Solution:
There are plenty of ways to do it, I first did it with a for loop, then with a while loop, and in JS you can even do this in 1 line with s.reverse();

However, the general idea is:

1. Define 2 pointers, start and end. Start being initialised to the 0th element in the array, end being initialised to the last.
2. While start is smaller than end, assign the value of s[end] to s[start] and vice versa.
3. This can be done by having a storage variable to store the value of start before it's overwritten, or by using destructuring assignment, which reassigns the variables without the need for an extra storage variable.
4. If using a while loop, be sure to increment and decrement start and end respectively at the end. In the for loop this will be done automatically.
5. Return s.

Code:
```
var reverseString = function(s) {
    let start = 0;
    let end = s.length - 1;
   
    while (start < end){
        [s[start], s[end]] = [s[end], s[start]];
        start++;
        end--;
    }
    return s;
};
```
### 350. Intersection of Two Arrays II

Problem:
Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must appear as many times as it shows in both arrays and you may return the result in any order.

Solution:

1. Create a hash table.
2. Create an array to store the result.
3. Loop through all elements in the first array. Add each element to the hash table, increasing its frequency for each time it appears.
4. Loop through all elements in the second array. If the element is in the hash table, push the element to the result array, and decrease its frequency in the hash table.
5. Return the resulting array.

Code:
```
var intersect = function(nums1, nums2) {
    const hash = {};
    const result = [];

    for (let i = 0; i < nums1.length; i++) {
        const num = nums1[i];
        hash[num] = hash[num] + 1 || 1;
    }

    for (let i = 0; i < nums2.length; i++) {
        const num = nums2[i];
        
        if (hash[num] > 0){
            result.push(num);
            hash[num]--;
        }
    }
    
    return result;
};
```
### 387. First Unique Character in a String

Problem:
Given a string s, find the first non-repeating character in it and return its index. If it does not exist, return -1. 

Solution:
1. Create hash map.
2. Create a for loop to loop through each character in the array.
3. Each time the character appears, increment it's number by 1.
4. Create another for loop to loop through all the characters. If it's number is 1, return the index.
5. Return -1 at the end as if it's reached that point there are no non-repeating characters.

Code:
```
var firstUniqChar = function(s) {
    let hash = {};

    for (let i = 0; i < s.length; i++){
        let character = s[i];
        hash[character] = hash[character] + 1 || 1;
    }

    for (let i = 0; i < s.length; i++){
        let character = s[i];
        if (hash[character] === 1){
            return i;
        }
    }

    return -1;
};
```
### 412. Fizz Buzz

Description: 
Given an integer n, return a string array answer (1-indexed) where:
answer[i] == "FizzBuzz" if i is divisible by 3 and 5.
answer[i] == "Fizz" if i is divisible by 3.
answer[i] == "Buzz" if i is divisible by 5.
answer[i] == i (as a string) if none of the above conditions are true.

Solution:
1. Create an array to hold the solution.
2. Loop through all elements, since it's 1 indexed, start from 1 and end at n+1.
3. Create a string (or array) to store the current element.
4. If i is divisible by 3, add Fizz, if 5 then add Buzz, if neither add i.
5. Push the string (or array) to the result array.
6. Return array.

Code:
```
var fizzBuzz = function(n) {
    let arr = [];
    for (let i = 1; i < n + 1; i++){
       let str = "";
        if (i % 3 === 0){
            str += "Fizz";
        } 
        if (i % 5 === 0){
            str += "Buzz";
        } 
        if (i % 3 !== 0 && i % 5 !== 0) {
            str += i;
        }
        arr.push(str);
    }
    return arr;
};
```
In C#:
```
public class Solution {
    public IList<string> FizzBuzz(int n) {
        
        string[] arr = new string[n];

        for (int i = 1; i < n + 1; i++){
            string str = String.Empty;
            if (i % 3 == 0){
                str += "Fizz";
            }
            if (i % 5 == 0){
                str += "Buzz";
            }
            if (i % 3 != 0 && i % 5 != 0){
                str += i;
            }
            arr[i - 1] = str;
        }
    return arr;
    }
}
```
## Leftover easies

### 27. Remove Element

Description:

Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
Return k.

My first try:
```
var removeElement = function(nums, val) {
    let k = 0;
    for (let i = nums.length-1; i >= 0; i--){
        if (nums[i] === val){
            k++;
            nums[i] = nums[nums.length - k];
        }
        }
return nums.length - k;
};
```
Other solution:
```
var removeElement = function(nums, val) {
    let left = 0;
    let right = nums.length - 1;

    while (left <= right) {
        if (nums[left] === val) {
            nums[left] = nums[right];
            right--;
        } else {
            left++;
        }
    }
    return left;
};
```
Both have same time complexity but the second solution is slightly faster.

### 35. Search Insert Position

Description:

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order. You must write an algorithm with O(log n) runtime complexity.

My first try (without thinking about time complexity):
```
var searchInsert = function(nums, target) {
    let result = nums.indexOf(target);

    if (result === -1){
        for (let i = 0; i < nums.length; i++){
            if (target < nums[i]){
                result = i;
                break;
            }
            if (i === nums.length - 1){
                result = i + 1;
            }
        }
    }

    return result;
};
```
Although this was faster than 97.5% of solutions on Leetcode, it unfortunately is O(n) because it uses a for loop and uses indexOf.

Solution:
1. Use a binary search algorithm. Any algorithm having O(logn) time complexity means that it likely reduces the input size by half with each iteration.
2. Define a left and right pointer of 0 and the end of the array.
3. While left is smaller than right:
4. Find the mid point by adding left and right and dividing by 2.
5. If the index of mid is the same as the target, return mid.
6. Else, if mid is smaller than the target, that means the target is larger, so move left to be mid + 1.
7. Else, if mid is bigger than the target, move right to 1 less than mid to narrow the search.
8. Return left at the end, as this is where the target should be if it was in the array.

```
var searchInsert = function(nums, target) {
    let left = 0;
    let right = nums.length - 1;

    while (left <= right){
        let mid = Math.floor((left + right)/2);

        if (nums[mid] === target){
            return mid;
        } else if (nums[mid] < target){
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return left;
};
```

### 226. Invert Binary Tree
Description: Given the root of a binary tree, invert the tree, and return its root.

Solution:
1. Use a recursive approach. Start by returning null if root is equal to null.
2. Create a temporary variable to store the left node, then sway it with the right and replace right with temp.
3. Call the function again on root.left and root.right.
4. Return the root.
 
Code:
```
var invertTree = function(root) {
    if (root === null){
        return null;
    }
    
    let temp = root.left;
    root.left = root.right;
    root.right = temp;

    invertTree(root.left);
    invertTree(root.right);

    return root;
};
```
## Top 150 Interview Questions - Medium

### 2. Add Two Numbers

Problem:
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list. You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Solution:
1. Initialize a new linked list l3 and a current node to point to the head of l3. This will be used to construct the result linked list.
2. Initialize two pointers p1 and p2 to point to the head of the input linked lists l1 and l2, respectively.
3. Initialize a carry variable to 0.
4. Loop through both input linked lists l1 and l2 simultaneously until both pointers p1 and p2, and carry become null:
5. 
  a. Calculate the sum of the current digits, along with the carry, by adding the values of p1.val, p2.val, and carry.
  b. Update the carry variable by dividing the sum by 10 and taking the integer quotient.
  c. Update the current digit of the result linked list by setting the value of the current node's next pointer to a new ListNode object with the value of the remainder of the sum (i.e., the sum modulo 10).
  d. Move the current pointer to the next node of the result linked list and move the input pointers p1 and p2 to their respective next nodes.
6. After the loop, if the carry variable is still non-zero, add a new node to the result linked list with the value of the carry.
7. Return l3.next, which is the second node, as the first node does not contain a meaningful value.

Code:
```
var addTwoNumbers = function(l1, l2) {
    let carry = 0;
    let l3 = new ListNode();
    let current = l3;
    let p1 = l1, p2 = l2;

    while (p1 || p2 || carry) {
        let sum = carry;
        if (p1) {
            sum += p1.val;
            p1 = p1.next;
        }
        if (p2) {
            sum += p2.val;
            p2 = p2.next;
        }
        carry = Math.floor(sum / 10);
        sum = sum % 10;
        current.next = new ListNode(sum);
        current = current.next;
    }

    return l3.next;
};
```
## Other Useful Stuff

### Common Sorting Algorithms

#### Bubble Sort
Bubble sort loops through all ements of the array from first to last and swaps adjacent items if they are in the wrong order.

![Bubble-sort-example-300px](https://github.com/webmanone/Leetcode-Notes/assets/66835665/d3ca08b8-9e9f-46ec-928d-26408aef7ca1)

Example code C#:
```
public int[] SortArray() 
{
    var n = NumArray.Length;
    for (int i = 0; i < n - 1; i++)
        for (int j = 0; j < n - i - 1; j++)
            if (NumArray[j] > NumArray[j + 1])
            {
                var tempVar = NumArray[j];
                NumArray[j] = NumArray[j + 1];
                NumArray[j + 1] = tempVar;
            }
    return NumArray;
}
```
How it works:
The code uses 2 for loops to compare adjacent items in the array. If elements are in the wrong order, they are swapped using a temporary variable to store the value that's being swapped.

Advantages:
- Simple to learn and easy to implement.
- Sorting done in place so it uses constant memory O(1).
- Adaptabele - can be changed to detect if portions are already sorted.
Disadvantages:
- Inefficient as it has time complexity O(n^2).

#### Selection Sort
Searches through each element to find the smallest value, comparing the current minimum value with the current element. If it's smaller, then a swap happens.

![Selection-Sort-Gif gif_zoom=1](https://github.com/webmanone/Leetcode-Notes/assets/66835665/4ec7b6d0-737c-408e-964c-7644ea64d83f)

Example code in C#:
```
public int[] SortArray()
{
    var arrayLength = NumArray.Length;
    for (int i = 0; i < arrayLength - 1; i++)
    {
        var smallestVal = i;
        for (int j = i + 1; j < arrayLength; j++)
        {
            if (NumArray[j] < NumArray[smallestVal])
            {
                smallestVal = j;
            }
        }
        var tempVar = NumArray[smallestVal];
        NumArray[smallestVal] = NumArray[i];
        NumArray[i] = tempVar;
    }
    return NumArray;
}
```
How it works:
The code uses 2 for loops, 1 to loop through all elements of the array aside from the last. A variable is created for the index of the smallest value. For the second loop, looping through all other elements from i, it compares the current smallest value to the current element value. If it's smaller, then it updates the index of the smallest value. Then, once all elements have been searched, a swap happens by using a temporary variable.

Advantages:
- Simple to learn and easy to implement.
- Sorting done in place so it uses constant memory O(1).
- Fewer swaps than bubble sort so may perform better in terms of time complexity.
Disadvantages:
- Inefficient as it has time complexity O(n^2).

- Insertion Sort
- Merge Sort
- Quick Sort
- Heap Sort
- Counting Sort
- Radix Sort
- Bucket Sort
- Shell sort

### Common Search Algorithms

- Linear Search
- Binary Search
- Jump Search
- Interpolation Search
- Exponential Search
- Sublist Search
- Fibonacci Search
- The Ubiquitous Binary Search
- String search algorithms.

### Questions to research
How to check if all words in an input line are in a body of larger text and then output a hash table not just with the ones that are found but the ones that aren't found as well and put -1.

### Update
Thanks to all my hard work practicing these kinds of problems, I performed well in a recent coding assessment centre and got an offer which I'm going to accept!!
