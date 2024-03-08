# Water Area

### Understanding the problem

You are given an array of non-negative integers where each non-zero integer represents the height of a pillar of width `1`. Imagine water being poured over all of the pillar; write a fucntion that returns the surface area of the water trapped between the pillars viewed from the front. Note that spilled water should be ignored.

<h3>Sample Input</h3>
<pre>
  heights =  [0, 8, 0, 0, 5, 0, 0, 10, 0, 0, 1, 1, 0, 3]
</pre>
<h3>Sample Output</h3>
<pre>
  48
</pre>
<h3>Visual Representation</h3>
<pre>
       |
       |
 |.....|
 |.....|
 |.....|
 |..|..|
 |..|..|
 |..|..|.....|
 |..|..|.....|
_|..|..|..||.|

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
function waterArea(heights) {
  let pillars = 0
  let surfaceArea = 0
  let space = 0
  let start = heights[0]

  let arr = [...heights]

  while (arr[arr.length - 1] === 0) {
    arr.pop()
  }

  if (arr.length === 0) return 0

  var sortedArray = [...heights].sort((a, b) => b - a)
  if (Math.max(...heights) === heights[0]) sortedArray.shift()

  for (i = 1; i < arr.length; i++) {
    // console.log(`Position = ${i}`)
    // console.log(`Space = ${space}\nPillar = ${pillars}\n---`)

    if (start > sortedArray[0]) start = sortedArray[0]
    // console.log(start)

    if (heights[i] >= start || i == arr.length - 1) {
      // console.log(`calculate`)
      surfaceArea += space * Math.min(heights[i], start) - pillars
      space = 0
      pillars = 0
      start = heights[i]
    }

    if (heights[i] < start) {
      // console.log(`accumulate`)
      space++
      pillars += heights[i]
    }
    // console.log(`Space = ${space}\nPillar = ${pillars}\n`)

    if (sortedArray[0] === heights[i]) sortedArray.shift()
    // console.log(heightSorted)
  }
  return surfaceArea
}

// Example usage:

const heights = [0, 8, 0, 0, 5, 0, 0, 10, 0, 0, 1, 1, 0, 3];
console.log(waterArea(heights)); // Output: 48
```

<div align="right">Swift <a href="#"><img src="../../icons/swift.svg" width="12px"></a> </div>

```swift
func waterArea(_ heights: [Int]) -> Int {
    guard !heights.isEmpty else { return 0 }

    var pillars = 0
    var area = 0
    var space = 0
    var start = heights[0]

    var mutableHeights = heights
    while mutableHeights.last == 0 {
        _ = mutableHeights.popLast()
    }

    var heightSorted = mutableHeights.sorted(by: >)
    if let maxHeight = heights.max(), maxHeight == heights.first {
        _ = heightSorted.removeFirst()
    }

    for x in 1..<mutableHeights.count {
        if start > heightSorted[0] { start = heightSorted[0] }

        if heights[x] >= start || x == mutableHeights.count - 1 {
            surfaceArea += space * min(heights[x], start) - pillars
            space = 0
            pillars = 0
            start = heights[x]
        }

        if heights[x] < start {
            space += 1
            pillars += heights[x]
        }

        if heightSorted[0] == heights[x] { _ = heightSorted.removeFirst() }
    }
    return surfaceArea
}

// Example usage:
let heights = [0, 8, 0, 0, 5, 0, 0, 10, 0, 0, 1, 1, 0, 3]
//let heights = [3, 1, 0, 2, 0, 1, 0, 0] //Special case Output: 4

let result = isValidSubsequence(array, sequence);
print(waterArea(heights)) // Output: 48
```

### Complexity Analysis

- Time Complexity: O(n log n) due to sorting, where n is the number of elements in the heights array.

- Space Complexity: O(n) for storing mutable heights and sorted heights arrays.

#

### Approach 2
#### ...

Explanation

<div align="right">Java Script <a href="#"><img src="../../icons/javascript.svg" width="12px"></a> </div>

```js
// code here
```

<div align="right">Swift <a href="#"><img src="../../icons/swift.svg" width="12px"></a> </div>

```swift
// code here
```

### Complexity Analysis

- Time Complexity: 

- Space Complexity: 



[js-icon]:../../icons/javascript.svg
[swift-icon]:../../icons/swift.svg
