---
title: Coin change (Second)
layout: post
date: 2022-02-02 22:40:00
categories: Algorithm Leetcode
---

## Daily Challenge for 02-02-2022

That is about the problem [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/) on leetcode. 

## Description 

Given two strings _s_ and _p_, return an array of all the start indices of `p's` _anagrams_ in s. You may return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

## My first thoughts

I start thinking about doing some permutation for the `p` String, so I could find all possible anagrams. Then, do a regex to find them out. But, that isn't a good solution.

## Second try 

```swift 
class Solution {
    func findAnagrams(_ s: String, _ p: String) -> [Int] {
        let sCharacters: [Character] = s.map { $0 }
        let pCharacters: [Character: Int] = p.reduce(into: [:]) {
            res, element in
            let counter = (res[element] ?? 0) + 1
            res[element] = counter
        }

        var res: [Int] = []

        for index in 0..<sCharacters.count {
            let start = index
            let end = index + p.count
            if end > sCharacters.count {
                break
            }
            if isAnagram(Array(sCharacters[start..<end]), pCharacters) {
                res.append(start)
            }
        }

        return res
    }
    
}

func isAnagram(_ s: [Character], _ p: [Character: Int]) -> Bool {
    var sCopy = s
    var currentSize = sCopy.count
    for element in p {
        sCopy.removeAll(where: { $0 == element.key })
        if (currentSize - sCopy.count) != element.value {
            return false
        }
        currentSize = sCopy.count
    }
    return true
}
```

### Final solution

I'm not happy with this solution, but it was the best I could get 

```swift
class Solution {
    func findAnagrams(_ s: String, _ p: String) -> [Int] {
        if s.count < p.count { return [] }
        
        let sCharacters: [Character] = s.map { $0 }
        let pCharacters: [Character: Int] = ocorrences(p.map { $0})

        print(pCharacters)
        var res: [Int] = []

        var slicedOcorrences = ocorrences(sCharacters[0..<p.count])

        for index in 0..<sCharacters.count {
            let start = index
            let end = index + p.count
            
            if isAnagram(slicedOcorrences, pCharacters) {
                res.append(start)
            }
            
            if end >= sCharacters.count { break }

            let nextCharacter = sCharacters[end]
            slicedOcorrences[nextCharacter] = (slicedOcorrences[nextCharacter] ?? 0) + 1

            let nextToBePopedOut = sCharacters[start]
            slicedOcorrences[nextToBePopedOut] = (slicedOcorrences[nextToBePopedOut] ?? 1) - 1
        }

        return res
    }
    
}

func ocorrences(_ array: [Character]) -> [Character: Int] {
    return array.reduce(into: [:]) {
        res, element in
        let counter = (res[element] ?? 0) + 1
        res[element] = counter
    }
}

func ocorrences(_ array: ArraySlice<Character>) -> [Character: Int] {
    return array.reduce(into: [:]) {
        res, element in
        let counter = (res[element] ?? 0) + 1
        res[element] = counter
    }
}

func isAnagram(_ s: [Character: Int], _ p: [Character: Int]) -> Bool {
    for element in p {
        if s[element.key] != element.value { return false }
    }
    return true
}
```