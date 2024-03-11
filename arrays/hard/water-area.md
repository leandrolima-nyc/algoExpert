# Water Area

### Understanding the problem

You are given an array of non-negative integers where each non-zero integer represents the height of a pillar of width `1`. Imagine water being poured over all of the pillar; write a fucntion that returns the surface area of the water trapped between the pillars viewed from the front. Note that spilled water should be ignored.

## Solutions
1. <a href="#method-1">Array Manipulation</a> <kbd>sort + single loop</kbd> 💡
2. <a href="#method-2">Array Processing</a> <kbd>two pointers</kbd> 🎯

### Visual Representation

```bash
heights = [0, 8, 0, 0, 5, 0, 0, 10, 0, 0, 1, 1, 0, 3]

// Output: 48

Indexes
 0 1 2 3 4 5 6 0 8 9 1 1 1 1
               ┃     0 1 2 3
   ┃ . . . . . ┃
   ┃ . . . . . ┃
   ┃ . . . . . ┃
   ┃ . . ┃ . . ┃
   ┃ . . ┃ . . ┃
   ┃ . . ┃ . . ┃ . . . . . ┃
   ┃ . . ┃ . . ┃ . . . . . ┃
   ┃ . . ┃ . . ┃ . . ┃ ┃ . ┃
 0 8 0 0 5 0 0 9 0 0 1 1 0 3
Heights

```

# 

### Method 1
#### Array Manipulation <kbd>for...in</kbd>

Initially, I deemed the `problem straightforward`, so I adopted a `simple and direct approach`. 

```js
if (!heights.some((height) => height > 0) || heights.length === 0) return 0

//Initializers
let pillars = 0
let surfaceArea = 0
let space = 0
let currentMax = heights[0]
```
It starts by comparing each element of the <kbd>array</kbd> against the <kbd>currentMax</kbd>. If the <kbd>currentHeight</kbd> equals or exceeds the <kbd>currentMax</kbd>, then it employes a formula to compute the current <kbd>surfaceArea</kbd> and updates the new <kbd>currentMax</kbd> with the <kbd>currentHeight</kbd> value. If the <kbd>currentHeight</kbd> is less than <kbd>currentMax</kbd>, then it accumulates the number of <kbd>pillars</kbd> with the current height <kbd>currentHeight</kbd> and increments the number <kbd>voids</kbd> separately in their respective variables, proceeding to the next iteration. This approach ensured that the function iterated through the <kbd>array</kbd> just once, efficiently finding the area of trapped water. 

<table><tr><td><samp>⚠️ Spoiler alert: Although I managed to obtain the correct answer for the sample input, there were additional scenarios that I overlooked. Thus, the journey continues..</td></tr></table>

```js
for (const idx in heights) {
  let currentHeight = heights[idx]

  if (currentHeight >= currentMax || idx == heights.length - 1) {
    surfaceArea += space * Math.min(currentHeight, currentMax) - pillars
    space = 0
    pillars = 0
    currentMax = currentHeight
  } else {
    space++
    pillars += currentHeight
  }
}
return surfaceArea

// Output: 48 ✅
```

### Patching
In the current state of the solution, the calculation only happens when falls under two conditions.  The <kbd>currentHeight</kbd> must excceds <kbd>currentMax</kbd> or it reaches the end of the <kbd>array</kbd>.  Any water trapped above the last number of the <kbd>array</kbd>, it will be completelly ignored. In this instance, only surface up to and including 3 are considered.  `Output: 22` ❌  

#### Different Scenario

```bash
heights = [0, 6, 0, 0, 5, 0, 0, 4, 0, 0, 1, 1, 0, 3]

// Output: 31

Indexes
 0 1 2 3 4 5 6 0 8 9 1 1 1 1                0 1 2 3 4 5 6 0 8 9 1 1 1 1
                     0 1 2 3                                    0 1 2 3
   ┃                                          ┃ 
   ┃ . . ┃                                    ┃ Ⅹ Ⅹ ┃
   ┃ . . ┃ . . ┃                              ┃ Ⅹ Ⅹ ┃ Ⅹ Ⅹ ┃
   ┃ . . ┃ . . ┃ . . . . . ┃                  ┃ . . ┃ . . ┃ . . . . . ┃
   ┃ . . ┃ . . ┃ . . . . . ┃                  ┃ . . ┃ . . ┃ . . . . . ┃
   ┃ . . ┃ . . ┃ . . ┃ ┃ . ┃                  ┃ . . ┃ . . ┃ . . ┃ ┃ . ┃ 
 0 6 0 0 5 0 0 4 0 0 1 1 0 3                0 6 0 0 5 0 0 4 0 0 1 1 0 3
Heights

```
At this point, I had to determine the highest number in the <kbd>array</kbd> from the current pointer <kbd>idx</kbd> to the end of the <kbd>array</kbd> and adjust the <kbd>currentMax</kbd> accordingly.  However, iterating through the entire <kbd>array</kbd> at each iteration was not pratical. I had to come up with a clever solution. 

To improve efficiency, I created a copy of the <kbd>array</kbd> called <kbd>maxList</kbd>, sorted in ascending order. During each iteration, elements are removed from <kbd>maxList</kbd> using the indexOf method along with <kbd>currentHeight</kbd> value. As a result, <kbd>maxList</kbd> retains only unprocessed values from the original <kbd>array</kbd>, with its first position always representing the highest value for subsequent iterations. Using this approach it allows to create of a variable <kbd>maxHeight</kbd>, which consistently updates to reflect the first element of <kbd>maxList</kbd>.

At last, we can correctly assign the smaller value between <kbd>currentHeight</kbd> and <kbd>maxHeight</kbd> to the variable <kbd>currentMax</kbd>.

```js
// ADDED
let maxList = [...heights].sort((a, b) => b - a)
```
```js
for (const idx in heights) {
// ADDED
  maxList.splice(maxList.indexOf(currentHeight), 1)
  const maxHeight = maxList[0]

  // code...
}
```
```js
// REMOVED:  if ( ... || idx == heights.length - 1)
if (currentHeight >= currentMax) {

  //...code

// REPLACED:  currentMax = currentHeight
  currentMax = Math.min(currentHeight, maxHeight)
}
```
### Final Solution

<div align="right">Java Script <a href="#"><img src="../../icons/javascript.svg" width="12px"></a> </div>

```js
function waterArea(heights) {
  if (!heights.some((height) => height > 0) || heights.length === 0) return 0

  let pillars = 0
  let surfaceArea = 0
  let voids = 0
  let currentMax = heights[0]
  let maxList = [...heights].sort((a, b) => b - a)

  for (const idx in heights) {
    const currentHeight = heights[idx]

    maxList.splice(maxList.indexOf(currentHeight), 1) 
    const maxHeight = maxList[0] 

    if (currentHeight >= currentMax) {
      surfaceArea += voids * Math.min(currentHeight, currentMax) - pillars
      currentMax = Math.min(currentHeight, maxHeight)
      voids = 0
      pillars = 0

    } else {
      voids++
      pillars += currentHeight
    }
  }
  return surfaceArea
}


// Example usage:
const heights = [0, 8, 0, 0, 5, 0, 0, 10, 0, 0, 1, 1, 0, 3]
//const heights = [0, 8, 0, 0, 5, 0, 0, 10, 0, 0, 1, 1, 0, 3] //sencond scenario Output: 31
const result = waterArea(heights)

console.log(result) // Output: 48
```

<div align="right">Swift <a href="#"><img src="../../icons/swift.svg" width="12px"></a> </div>

```swift
func waterArea(_ heights: [Int]) -> Int {
    guard !heights.isEmpty && heights.contains(where: { $0 > 0 }) else { return 0 }
    
    var pillars = 0
    var surfaceArea = 0
    var voids = 0
    var currentMax = heights[0]
    var maxList = heights.sorted(by: >)
    
    for (idx, currentHeight) in heights.enumerated() {
        maxList.remove(at: maxList.firstIndex(of: currentHeight)!)
        let maxHeight = maxList.first ?? 0
        
        if currentHeight >= currentMax {
            surfaceArea += voids * min(currentHeight, currentMax) - pillars
            currentMax = min(currentHeight, maxHeight)
            voids = 0
            pillars = 0
        } else {
            voids += 1
            pillars += currentHeight
        }
    }
    return surfaceArea
}

// Example usage:
let heights = [0, 8, 0, 0, 5, 0, 0, 10, 0, 0, 1, 1, 0, 3]
//let heights = [0, 8, 0, 0, 5, 0, 0, 10, 0, 0, 1, 1, 0, 3] //sencond scenario Output: 31

let result = waterArea(heights);
print(result) // Output: 48
```

### Complexity Analysis

- Time Complexity: O(n) the time complexity of the function is linear with respect to the size of the input array.

- Space Complexity: O(n log n) due to the sorting step, but the space complexity is O(1). sSince we’re not using any additional data structures.This function uses a fixed number of variables to perform its computations, and it does not create any additional data structures or allocate memory dynamically. Therefore, its space complexity is constant and does not depend on the size of the input array.

#

### Method 2
#### Array Processing <kbd>two pointers</kbd>

Explanation

// work in progress...

<div align="right">Java Script <a href="#"><img src="../../icons/javascript.svg" width="12px"></a> </div>

```js
function waterArea(heights) {
  if (heights.length === 0 || !heights.some((height) => height > 0)) return 0

  let leftPointer = 0
  let rightPointer = heights.length - 1

  let maxLeft = heights[leftPointer]
  let maxRight = heights[rightPointer]

  let surfaceArea = 0

  while (leftPointer < rightPointer) {
    if (heights[leftPointer] < heights[rightPointer]) {
      leftPointer++
      maxLeft = Math.max(maxLeft, heights[leftPointer])
      surfaceArea += maxLeft - heights[leftPointer]
    } else {
      rightPointer--
      maxRight = Math.max(maxRight, heights[rightPointer])
      surfaceArea += maxRight - heights[rightPointer]
    }
  }

  return surfaceArea
}

const heights = [0, 8, 0, 0, 5, 0, 0, 10, 0, 0, 1, 1, 0, 3]
result = waterArea(heights)
console.log(result)
```

<div align="right">Swift <a href="#"><img src="../../icons/swift.svg" width="12px"></a> </div>

```swift
func waterArea(_ heights: [Int]) -> Int {
    guard !heights.isEmpty && heights.contains(where: { $0 > 0 }) else { return 0 }
    
    var leftPointer = 0
    var rightPointer = heights.count - 1
    
    var maxLeft = heights[leftPointer]
    var maxRight = heights[rightPointer]
    
    var surfaceArea = 0
    
    while leftPointer < rightPointer {
        if heights[leftPointer] < heights[rightPointer] {
            leftPointer += 1
            maxLeft = max(maxLeft, heights[leftPointer])
            surfaceArea += maxLeft - heights[leftPointer]
        } else {
            rightPointer -= 1
            maxRight = max(maxRight, heights[rightPointer])
            surfaceArea += maxRight - heights[rightPointer]
        }
    }
    
    return surfaceArea
}
```

### Complexity Analysis

- Time Complexity: O(n), where n is the number of elements in the heights array. The function iterates through the array once using two pointers, performing constant-time operations within each iteration. Therefore, the overall time complexity is linear with respect to the size of the input array.

- Space Complexity: O(1), the function uses a fixed amount of additional memory to store variables such as pointers, maximum heights, and the surface area, which does not increase with the size of the input array. Therefore, the space complexity is constant.




[js-icon]:../../icons/javascript.svg
[swift-icon]:../../icons/swift.svg
