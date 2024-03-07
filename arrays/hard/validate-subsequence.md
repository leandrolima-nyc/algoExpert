# Validate Subsequence

### Understanding the problem
<p>
  Given two non-empty arrays of integers, write a function that determines
  whether the second array is a subsequence of the first one.
</p>
<p>
  A subsequence of an array is a set of numbers that aren't necessarily adjacent
  in the array but that are in the same order as they appear in the array. For
  instance, the numbers <code>[1, 3, 4]n</code> form a subsequence of the array
  <code>[1, 2, 3, 4]</code>, and so do the numbers <code>[2, 4]</code>. Note
  that a single number in an array and the array itself are both valid
  subsequences of the array.
</p>
<h3>Sample Input</h3>
<pre>
  array = [5, 1, 22, 25, 6, -1, 8, 10]
  sequence = [1, 6, -1, 10]
</pre>
<h3>Sample Output</h3>
<pre>
  true
</pre>

## Solutions
1. <a href="#approach-1-Subsequence-Validation">Subsequence Validation</a>
2. <a href="#approach-2-two-pass-with-hash-table">Two Pass with Hash Table</a>

# 

### Approach 1: Subsequence Validation

This function `isValidSubsequence` checks if the elements in the `sequence` array appear in the `array` array in the same order. It iterates through both arrays, updating a `counter` variable whenever a matching element is found in the `array` compared to the `sequence`. Finally, it checks if the `counter` is equal to the length of the `sequence` array to determine if all elements in the `sequence` were found in the `array`.

<div align="right">Java Script <a href="#"><img src="../../icons/javascript.svg" width="12px"></a> </div>

```js
function isValidSubsequence(array, sequence) {
    let counter = 0;
    for (let element of array) {
        if (counter < sequence.length && element === sequence[counter]) {
            counter++;
        }
    }
    return counter === sequence.length;
}

// Example usage:
let array = [5, 1, 22, 25, 6, -1, 8, 10];
let sequence = [1, 6, -1, 10];
let result = isValidSubsequence(array, sequence);
console.log(result); // Output: true
```

<div align="right">Swift <a href="#"><img src="../../icons/swift.svg" width="12px"></a> </div>

```swift
func isValidSubsequence(array: [Int], sequence: [Int]) -> Bool {
    var counter = 0
    for element in array {
        if counter < sequence.count && element == sequence[counter] {
            counter += 1
        }
    }
    return counter == sequence.count
}

// Example usage:
let array = [5, 1, 22, 25, 6, -1, 8, 10]
let sequence = [1, 6, -1, 10]
let result = isValidSubsequence(array: array, sequence: sequence)
print(result) // Output: true
```

### Complexity Analysis

- Time Complexity: O(n), where n is the length of the input array.

- Space Complexity: O(1).

#

### Approach 2: Two Pass with Hash Table

In the brute force approach above, the repeated search for each number's complement (`target sum - array[i]`) slows down the overall runtime. We can reduce the search from O(N) to amortized O(1) by throwing all the number in the array into a hash table, trading space for time. We need to ensure that the complement is not the current number itself though. For instance, suppose the input array is `[5, 2]` and the target sum is `10`, the hash table is going to be `{5: value, 2: value}`. The complement of `5` is `5` (`10 - 5 = 5`) and `5` does exist in the hash table, however, the complement is the number itself, thus it is not a valid answer. To handle this we can store each number's index as value in the hash table and compare the complement's index to the current number's index.

**Algorithm**

- Go through the input array. Add each number as key and its index as value to the hash table.

- Loop through the input array again. Check if each number's complement is present in the hash table. If it is present and is not the current number itself, return the current number and its complement.

<div align="right">Java Script <a href="#"><img src="../../icons/javascript.svg" width="12px"></a> </div>

```js
function twoNumberSum(array, targetSum) {
    let hashTable = {};

    // Populate the hash table with the array elements and their indices
    for (let i = 0; i < array.length; i++) {
        hashTable[array[i]] = i;
    }

    // Iterate through the array to find the complement of each element
    for (let i = 0; i < array.length; i++) {
        let value = array[i];
        let complement = targetSum - value;

        // Check if the complement exists in the hash table and it is not the current element
        if (hashTable.hasOwnProperty(complement) && hashTable[complement] !== i) {
            return [value, complement];
        }
    }

    // If no pair is found, return an empty array
    return [];
}

// Example usage:
let array = [3, 5, -4, 8, 11, 1, -1, 6];
let targetSum = 10;
let result = twoNumberSum(array, targetSum);
console.log(result); // Output: [11, -1]
```

<div align="right">Swift <a href="#"><img src="../../icons/swift.svg" width="12px"></a> </div>

```swift
func twoNumberSum(array: [Int], targetSum: Int) -> [Int] {
    var hashTable = [Int: Int]()

    for (index, value) in array.enumerated() {
        hashTable[value] = index
    }

    for (index, value) in array.enumerated() {
        let complement = targetSum - value

        if let complementIndex = hashTable[complement], complementIndex != index {
            return [value, complement]
        }
    }

    return []
}

// Example usage:
let array = [3, 5, -4, 8, 11, 1, -1, 6]
let targetSum = 10
let result = twoNumberSum(array: array, targetSum: targetSum)
print(result) // Output: [11, -1]
```

### Complexity Analysis

Let N be the length of the input array.

- Time Complexity: O(N).

- Space Complexity: O(N).



[js-icon]:../../icons/javascript.svg
[swift-icon]:../../icons/swift.svg
