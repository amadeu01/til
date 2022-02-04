---
title: 4Sum II
layout: post
categories: Algorithm Leetcode
---

## Daily Challenge for 03-02-2022

That is about the problem [4Sum II - LeetCode](https://leetcode.com/problems/4sum-ii/) on leetcode. 

## My initial 

The first thing I've done was terrible !

```swift
class Solution {
    func fourSumCount(
        _ nums1: [Int],
        _ nums2: [Int],
        _ nums3: [Int],
        _ nums4: [Int]
    ) -> Int {

        let permutes = permutations(
            nums1,
            nums2,
            nums3,
            nums4
        )
        
        return permutes
            .map { $0.reduce(0, +) }
            .filter({ $0 == 0}).count
    }
}

func permutations(
    _ nums1: [Int]
) -> [[Int]] {
    return nums1.map { [$0] }
}

func permutations(
    _ nums1: [Int],
    _ nums2: [Int]
) -> [[Int]] {
    var permutationsResult: [[Int]] = []
    let permutations2 = permutations(nums2)
    for nums in nums1 {
        permutationsResult.append(contentsOf:
            permutations2.map { [nums] + $0  }
        )
    }
    return permutationsResult
}

func permutations(
    _ nums1: [Int],
    _ nums2: [Int],
    _ nums3: [Int]
) -> [[Int]] {
    var permutationsResult: [[Int]] = []
    let permutations2 = permutations(nums2, nums3)
    for nums in nums1 {
        permutationsResult.append(contentsOf:
            permutations2.map { [nums] + $0  }
        )
    }
    return permutationsResult
}

func permutations(
    _ nums1: [Int],
    _ nums2: [Int],
    _ nums3: [Int],
    _ nums4: [Int]
) -> [[Int]] {
    var permutationsResult: [[Int]] = []
    let permutations2 = permutations(nums2, nums3, nums4)
    for nums in nums1 {
        permutationsResult.append(contentsOf:
            permutations2.map { [nums] + $0  }
        )
    }
    return permutationsResult
}
```

It got a timeout :(

## Accepted solution

Not the best solution, but I got passed on all tests.

```swift
typealias Sum = Int

class Solution {
    func fourSumCount(
        _ nums1: [Int],
        _ nums2: [Int],
        _ nums3: [Int],
        _ nums4: [Int]
    ) -> Int {
        return sums(
            [nums1, nums2, nums3, nums4]
        )[0] ?? 0
    }
}

func sums(
    _ numsArray: [[Int]]
) -> [Sum: Int] {

    if numsArray.count == 1 {
        var sums: [Sum: Int] = [:]
        for num in numsArray[0] {
            sums[num] = (sums[num] ?? 0) + 1
        }
        return sums
    }

    var resultSums: [Sum: Int] = [:]
    let nums2 = Array(numsArray.suffix(from: 1))

    for (sum, value) in sums(nums2) {
        for num in numsArray[0] {
            resultSums[num + sum] =
            (resultSums[num + sum] ?? 0) + value
        }
    }
    return resultSums
}
```

## Study

- Related resource [video](https://www.youtube.com/watch?v=57SUNQL4JFA&t)
- https://www.geeksforgeeks.org/find-all-distinct-quadruplets-in-an-array-that-sum-up-to-a-given-value/