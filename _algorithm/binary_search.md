---
title: Binary Search
layout: post
---

## Binary Search

- [Really helpful content](https://www.geeksforgeeks.org/the-ubiquitous-binary-search-set-1/)
- [Wiki](https://en.wikipedia.org/wiki/Binary_search_algorithm#:~:text=In%20computer%20science%2C%20binary%20search,middle%20element%20of%20the%20array.)

My first attempt to write binary search was something like below 🔽

```swift
extension Array where Element == Int {
    func binaryLastMinorSearch(searchItem: Int) -> Int? {
        var lowerIndex = 0
        var upperIndex = self.count - 1
        while (lowerIndex < upperIndex) {
            let currentIndex = Int(ceil(Double(lowerIndex + upperIndex)/2.0))
            
            if (self[currentIndex] > searchItem) {
                upperIndex = currentIndex - 1
            } else if (self[currentIndex] < searchItem) {
                lowerIndex = currentIndex
            } else {
                return currentIndex
            }
        }

        return lowerIndex
    }

    func binaryFirstMajorSearch(searchItem: Int) -> Int? {
        var lowerIndex = 0
        var upperIndex = self.count - 1
        while (lowerIndex < upperIndex) {
            let currentIndex = (lowerIndex + upperIndex)/2
            
            if (self[currentIndex] > searchItem) {
                upperIndex = currentIndex
            } else if (self[currentIndex] < searchItem) {
                lowerIndex = currentIndex + 1
            } else {
                return currentIndex
            }
        }

        return lowerIndex
    }
}
```

Then I started to wonder more about how I could be sure the algorithm was right.
Or even more, how I could think of something similar in a more processual thought.

So I read the content in [here](https://www.geeksforgeeks.org/the-ubiquitous-binary-search-set-1/)
it provided me with this implementation

```java
// Returns location of key, or -1 if not found 
int BinarySearch(int A[], int l, int r, int key) 
{ 
    int m; 
  
    while( l <= r ) 
    { 
        m = l + (r-l)/2; 
  
        if( A[m] == key ) // first comparison 
            return m; 
  
        if( A[m] < key ) // second comparison 
            l = m + 1; 
        else
            r = m - 1; 
    } 
  
    return -1; 
} 
```

And it was envolved to:

```java
// Invariant: A[l] <= key and A[r] > key 
// Boundary: |r - l| = 1 
// Input: A[l .... r-1] 
int BinarySearch(int A[], int l, int r, int key) 
{ 
    int m; 
  
    while( r - l > 1 ) 
    { 
        m = l + (r-l)/2; 
  
        if( A[m] <= key ) 
            l = m; 
        else
            r = m; 
    } 
  
    if( A[l] == key ) 
        return l; 
    else
        return -1; 
} 
```

Which was similar to something I was tring to do, but I couldn't prove that I would ended up with 2 element at
some point, which would be the `right` and the `left` elements.
Here in this piece of code is clear that the algorithm will converge to a set of `right` and `left`. 
That are adjacent elements.

We can see that :

```java
m = l + (r-l)/2
``` 

`m` is always changing its value until `(r-l)/2` be 0, what would only happen if `r - l < 2`
Therefore, if `r - l > 1` -- or `r > l + 1` -- the value of `m` will always be changing, and then, we will check
different values for `A[m]`