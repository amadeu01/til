---
title: Coin change (Second)
layout: post
date: 2020-06-07 14:58:09
categories: Algorithm Leetcode
---

About the [problem](https://leetcode.com/explore/featured/card/june-leetcoding-challenge/539/week-1-june-1st-june-7th/3353/) on 06/06/2020 on leetcode

## Content

- [Greedy vs Dynamic Programming - Geeks For Geeks](https://www.geeksforgeeks.org/greedy-approach-vs-dynamic-programming/#:~:text=In%20a%20greedy%20Algorithm%2C%20we,problem%20to%20calculate%20optimal%20solution%20.)

## Solution

```swift
class Solution {
    func change(_ amount: Int, _ coins: [Int]) -> Int {
        let numberOfCoins = coins.count
        var cache = [Int].init(repeating: 0, count: amount + 1)
        cache[0] = 1
        for i in 0..<numberOfCoins {
            if coins[i] > amount { continue }
            for j in coins[i]...amount {
                cache[j] += cache[j - coins[i]]
            }
        }
        return cache[amount]
    }
}
```

