---
title: Queue Reconstruction by Height
layout: post
date: 2020-06-07 14:58:09
categories: Algorithm Leetcode
---

About the [problem](https://leetcode.com/explore/challenge/card/june-leetcoding-challenge/539/week-1-june-1st-june-7th/3352/) on 06/06/2020 on leetcode

## Adding from the highest to the shortest

```swift
func reconstructQueue(_ people: [[Int]]) -> [[Int]] {
    let sortedPeople = people.sorted { $0[0] == $1[0] ? $0[1] < $1[1] : $0[0] > $1[0] }
    var res = [[Int]]()

    for person in sortedPeople {
        res.insert(person, at: person[1])
    }

    return res
}
```

## Adding from the shortest to the highest

For each of the smallest number, I need to check:

1. If some people with same height was already placed before the current person.

```swift
if index > 0 {
    if sortedPeople[index - 1][0] == currentPersonHeight {
        numberOfPeopleWithSameHeightBeforeCurrent += 1
    } else {
        numberOfPeopleWithSameHeightBeforeCurrent = 0
    }
}
```

2. If I have enough spots for the next bigger person to be placed.

```swift
var numberOfEmptySpotsFound = 0
var insertionIndex = -1
while numberOfEmptySpotsFound <= currentPersonEmptyOnFront - numberOfPeopleWithSameHeightBeforeCurrent {
    insertionIndex += 1
    if res[insertionIndex].isEmpty {
        numberOfEmptySpotsFound += 1
    }
}
```

```swift
func reconstructQueue(_ people: [[Int]]) -> [[Int]] {
    let sortedPeople = people.sorted { $0[0] == $1[0] ? $0[1] < $1[1] : $0[0] < $1[0] }
    let peopleCount = people.count
    var res = [[Int]].init(repeating: [], count: peopleCount)

    var numberOfPeopleWithSameHeightBeforeCurrent = 0
    for (index, person) in sortedPeople.enumerated() {
        let currentPersonHeight = person[0]
        let currentPersonEmptyOnFront = person[1]

        if index > 0 {
            if sortedPeople[index - 1][0] == currentPersonHeight {
                numberOfPeopleWithSameHeightBeforeCurrent += 1
            } else {
                numberOfPeopleWithSameHeightBeforeCurrent = 0
            }
        }

        var numberOfEmptySpotsFound = 0
        var insertionIndex = -1
        while numberOfEmptySpotsFound <= currentPersonEmptyOnFront - numberOfPeopleWithSameHeightBeforeCurrent {
            insertionIndex += 1
            if res[insertionIndex].isEmpty {
                numberOfEmptySpotsFound += 1
            }
        }
        res[insertionIndex] = person
    }

    return res
}
```

#### Second Approach

We can follow the steps for the test case of ⤵️

- `let input = [[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]`

You can see the steps until the resolution and find the answer of 🎺

`[[5, 0], [7, 0], [5, 2], [6, 1], [4, 4], [7, 1]]`

```
currentPersonHeight 4
currentPersonEmptyOnFront 4
emptyStops 5
insertionIndex 5
current person [4, 4]
 [[], [], [], [], [], []]
 [[], [], [], [], [4, 4], []]
---

currentPersonHeight 5
currentPersonEmptyOnFront 0
emptyStops 1
insertionIndex 1
current person [5, 0]
 [[], [], [], [], [4, 4], []]
 [[5, 0], [], [], [], [4, 4], []]
---

currentPersonHeight 5
currentPersonEmptyOnFront 2
emptyStops 2
insertionIndex 3
current person [5, 2]
 [[5, 0], [], [], [], [4, 4], []]
 [[5, 0], [], [5, 2], [], [4, 4], []]
---

currentPersonHeight 6
currentPersonEmptyOnFront 1
emptyStops 2
insertionIndex 4
current person [6, 1]
 [[5, 0], [], [5, 2], [], [4, 4], []]
 [[5, 0], [], [5, 2], [6, 1], [4, 4], []]
---

currentPersonHeight 7
currentPersonEmptyOnFront 0
emptyStops 1
insertionIndex 2
current person [7, 0]
 [[5, 0], [], [5, 2], [6, 1], [4, 4], []]
 [[5, 0], [7, 0], [5, 2], [6, 1], [4, 4], []]
---

currentPersonHeight 7
currentPersonEmptyOnFront 1
emptyStops 1
insertionIndex 6
current person [7, 1]
 [[5, 0], [7, 0], [5, 2], [6, 1], [4, 4], []]
 [[5, 0], [7, 0], [5, 2], [6, 1], [4, 4], [7, 1]]
---
[[5, 0], [7, 0], [5, 2], [6, 1], [4, 4], [7, 1]]

```

The second approach is notably slower, but it is nice to think how we can start from the shortest one to the highest.
Since it was more difficult to find the solution.
