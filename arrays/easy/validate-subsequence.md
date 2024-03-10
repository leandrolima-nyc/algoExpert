# Validate Subsequence

### Understanding the problem
<p>
  Given two non-empty arrays of integers, write a function that determines
  whether the second array is a subsequence of the first one.
</p>
<p>
  A subsequence of an array is a set of numbers that aren't necessarily adjacent
  in the array but that are in the same order as they appear in the array. For
  instance, the numbers <code>[1, 3, 4]</code> form a subsequence of the array
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
1. <a href="#approach-1">Subsequence Validation</a> <kbd>.forEach</kbd>
2. <a href="#approach-2">Subsequence Validation</a> <kbd>for...of</kbd>

# 

### Approach 1
#### Subsequence Validation <kbd>.forEach</kbd>

It iterates through both arrays, updating a `counter` variable whenever a matching element is found in the `array` compared to the `sequence`. Finally, it checks if the `counter` is equal to the length of the `sequence` array to determine if all elements in the `sequence` were found in the `array`.

<div align="right">Java Script <a href="#"><img src="../../icons/javascript.svg" width="12px"></a> </div>

```js
function isValidSubsequence(array, sequence) {
    let counter = 0
    array.forEach(element => element == sequence[counter] && counter++)
    return counter == sequence.length
}

// Example usage:
let array = [5, 1, 22, 25, 6, -1, 8, 10];
let sequence = [1, 6, -1, 10];
let result = isValidSubsequence(array, sequence);
console.log(result); // Output: true
```

<div align="right">Swift <a href="#"><img src="../../icons/swift.svg" width="12px"></a> </div>

```swift
func isValidSubsequence(_ array: [Int], _ sequence: [Int]) -> Bool {
    var counter = 0
    array.forEach { number in
      if counter < sequence.count && sequence[counter] == number { counter += 1 }
    }
    return counter == sequence.count
  }
}

// Example usage:
let array = [5, 1, 22, 25, 6, -1, 8, 10]
let sequence = [1, 6, -1, 10]
let result = isValidSubsequence(array: array, sequence: sequence)
print(result) // Output: true
```

### Complexity Analysis

- Time Complexity: O(n), where n is the length of the input array.

- Space Complexity: O(1), as it only uses a constant amount of extra space regardless of the size of the input.

#

### Approach 2
#### Subsequence Validation <kbd>for...of</kbd>

It iterates through each element of the `array` and compares it with the corresponding element in the `sequence`. If a match is found, it increments the `counter`. If the `counter` reaches the length of the `sequence`, it means the `sequence` is a valid subsequence.

<div align="right">Java Script <a href="#"><img src="../../icons/javascript.svg" width="12px"></a> </div>

```js
function isValidSubsequence(array, sequence) {
    let counter = 0;
    for (let element of array) {
        if (counter == sequence.length || array.length < sequence.length) break
        element == sequence[counter] && counter++
    }
    return counter === sequence.length;
}

const array = [5, 1, 22, 25, 6, -1, 8, 10];
const sequence = [1, 6, -1, 10];

console.log(isValidSubsequence(array, sequence)); // Output: true
```

<div align="right">Swift <a href="#"><img src="../../icons/swift.svg" width="12px"></a> </div>

```swift
func isValidSubsequence(_ array: [Int], _ sequence: [Int]) -> Bool {
    var counter = 0
    for element in array {
        if counter == sequence.count || array.count < sequence.count { break }
        if element == sequence[counter] { counter += 1 }
    }
    return counter == sequence.count
}

let array = [5, 1, 22, 25, 6, -1, 8, 10]
let sequence = [1, 6, -1, 10]

print(isValidSubsequence(array, sequence)) // Output: true
```

### Complexity Analysis

Let N be the length of the input array.

- Time Complexity: O(n), where n is the length of the array.

- Space Complexity: O(1), as it uses a constant amount of extra space regardless of the size of the input arrays.



[js-icon]:../../icons/javascript.svg
[swift-icon]:../../icons/swift.svg
