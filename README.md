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
  7.	Return new linked list

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

#### Description

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

#### My first solution:

1. Think about the base cases. There's only one way to climb 0 or 1 steps, so initialise an array to store all the different ways, with 1 and 1 as the first 2 vaules.
2. Loop through all remaining ways until n, starting from 2 since 0 and 1 positions are already there.
3. The number of steps it will take to reach i is the number of steps it took to reach i - 1 + number of steps it took to reach i - 2. This is because from i-1 and i-2, you can either take 1 or 2 steps to get there. Add the value of i-1 + i-2.
4. Return the n'th position in the array.

##### Code: 
```
 var climbStairs = function(n) {
    
    let arr = [1, 1];

    for (let i = 2; i <= n; i++){
        arr.push(arr[i-1] + arr[i-2])
    }

    return arr[n];

  };
  ```
#### Optimised solution
  
When I submitted my answer, I saw that memory was high compared with other solutions. I realised it's possible to have the solution without creating an array, and just storing the previous value and the value before in variables as only the end value matters. This uses O(1) instead of O(n).

1. Declare variables for the previous step and the step before, both initialised to 1.
2. Loop from 2 to n.
3. Define a variable to store the value of the current step, which will be assigned to value of previous + step before previous.
4. Increment the i-2 variable to i-1.
5. Increment the previous step to current.
5. Return previous step (since it's assigned to value of current at end of the for loop).

##### Code:
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
