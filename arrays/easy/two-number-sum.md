# Two Number Sum

### Understanding the problem

Given an array of distinct integers and an integer representing the target sum, we are asked to implement a function that is going to find out whether there's a pair of numbers in the array that adds up to the target sum. If such pair exists, return the pair in any order; otherwise return an empty array. We cannot add a number to itself to get the target sum.


<div align="center">
  <a href="#approach-1-brute-force">Brute Force</a> •
  <a href="#approach-2-two-pass-with-hash-table">Two Pass with Hash Table</a> •
  <a href="#approach-3-one-pass-with-hash-table">One Pass with Hash Table</a> •
  <a href="#approach-4-sort--two-pointers">Sort + Two Pointers</a>
</div>

# 

### Approach 1: Brute Force

Iterate through the array. For each number, iterate through the rest of the array; if adding any number in the rest of the array to the number yields the target sum, return the pair.

<div align="right">Java Script <a href="#"><img src="../../icons/javascript.svg" width="12px"></a> </div>

```js
function twoNumberSum(array, targetSum) {
    for (let i = 0; i < array.length; i++) {
        let firstNum = array[i];
        for (let j = i + 1; j < array.length; j++) {
            let secondNum = array[j];
            if (firstNum + secondNum === targetSum) {
                return [firstNum, secondNum];
            }
        }
    }
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
    for i in 0..<array.count {
        let firstNum = array[i]
        for j in (i + 1)..<array.count {
            let secondNum = array[j]
            if firstNum + secondNum == targetSum {
                return [firstNum, secondNum]
            }
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

- Time Complexity: O(N^2), where N is the length of the input array.

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

#

### Approach 3: One Pass with Hash Table

We can check if the complement of the current number is present in the hash table while we are adding numbers into the hash table. At each step, we first look up current number's complement in the hash table. If it already exists, then we have found the pair; otherwise, add the current number into the hash table.

Suppose the input array is `[3, 5, 1, 7]` and the target sum is `10`.

We start at index `0`.

```
[ 3, 5, 1, 7 ]
  ^

hashTable = {}
```

The complement of `3` is `7`, we check if `7` is present in the hash table. It isn't, so we add `3` into the hash table.

```
[ 3, 5, 1, 7 ]
  ^

hashTable = { 3: 0 }
```

We move on to `5`, and check if its complement `5` exists in the hash table. It isn't, so we add `5` into the hash table.

```
[ 3, 5, 1, 7 ]
     ^

hashTable = { 3: 0,
              5: 1
            }
```

We move on to the next number `1`. Its complement `9` is not in the hash table. We add `1` into the hash table.

```
[ 3, 5, 1, 7 ]
        ^

hashTable = { 3: 0,
              5: 1,
              1: 2,
            }
```

We are now at `7`. Its complement `3` is present in the hash table, thus we have found the solution.

```
[ 3, 5, 1, 7 ]
           ^

hashTable = { 3: 0,
              5: 1,
              1: 2,
            }
```

We can also notice that with this approach we don't need to check if the complement is the current number itself. Therefore, it is not necessary to store each number's index as value in the hash table.

<div align="right">Java Script <a href="#"><img src="../../icons/javascript.svg" width="12px"></a> </div>

```js
function twoNumberSum(array, targetSum) {
    let hashTable = {};

    for (let num of array) {
        let complement = targetSum - num;

        if (hashTable[complement]) {
            return [complement, num];
        }

        hashTable[num] = true;
    }

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
    var hashTable = [Int: Bool]()

    for num in array {
        let complement = targetSum - num

        if hashTable[complement] != nil {
            return [complement, num]
        }

        hashTable[num] = true
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

#

### Approach 4: Sort + Two Pointers

We sort the input array in ascending order and use two pointers to find the pair in the array.

Suppose the input array is `[5, 1, -2, 6, -4, 9]` and the target sum is `3`. After sorting the array, we get `[-4, -2, 1, 5, 6, 9]`. We initialize two pointers `l` and `r` to point to the first and last elements in the array respectively.

```
[ -4, -2, 1, 5, 6, 9 ]
   ^               ^
   l               r
```

`-4 + 9 = 5`. `5` is greater than the target sum `3`. If we move `l` to right, then we would obtain an even greater sum (e.g. `-2 + 9 = 7`), since all the numbers after `-4` is going to be greater than `-4`. Therefore, we move `r` to left by one.

```
[ -4, -2, 1, 5, 6, 9 ]
   ^            ^
   l            r
```

`-4 + 6 = 2`. Now the sum is smaller than the target sum `3`, so we want to try a number that is greater than `-4`: move `l` to right by one.

```
[ -4, -2, 1, 5, 6, 9 ]
       ^        ^
       l        r
```

The sum is `4` and it is greater than `3`, so we move `r` to left by one.

```
[ -4, -2, 1, 5, 6, 9 ]
       ^     ^
       l     r
```

`-2 + 5 = 3`. We have found the pair.

<div align="right">Java Script <a href="#"><img src="../../icons/javascript.svg" width="12px"></a> </div>

```js
function twoNumberSum(array, targetSum) {
    let sortedArray = array.sort((a, b) => a - b);
    let left = 0;
    let right = sortedArray.length - 1;

    while (left < right) {
        let currSum = sortedArray[left] + sortedArray[right];
        if (currSum === targetSum) {
            return [sortedArray[left], sortedArray[right]];
        }
        if (currSum < targetSum) {
            left++;
        } else {
            right--;
        }
    }

    return [];
}

// Example usage:
let array = [3, 5, -4, 8, 11, 1, -1, 6];
let targetSum = 10;
let result = twoNumberSum(array, targetSum);
console.log(result); // Output: [-1, 11]
```

<div align="right">Swift <a href="#"><img src="../../icons/swift.svg" width="12px"></a> </div>

```swift
func twoNumberSum(array: [Int], targetSum: Int) -> [Int] {
    var sortedArray = array.sorted()
    var left = 0
    var right = sortedArray.count - 1

    while left < right {
        let currSum = sortedArray[left] + sortedArray[right]
        if currSum == targetSum {
            return [sortedArray[left], sortedArray[right]]
        }
        if currSum < targetSum {
            left += 1
        } else {
            right -= 1
        }
    }

    return []
}

// Example usage:
let array = [3, 5, -4, 8, 11, 1, -1, 6]
let targetSum = 10
let result = twoNumberSum(array: array, targetSum: targetSum)
print(result) // Output: [-1, 11]
```

### Complexity Analysis

- Time Complexity: O(N · log(N)), where N is the length of the input array.

- Space Complexity: O(log(N)) or O(N), depending on the implementation of the sorting algorithm.

[js-icon]:../../icons/javascript.svg
[swift-icon]:../../icons/swift.svg
