---
title: Core Data
layout: default
---

# Core Data

## Upsert in Core Data.

Based on that [question](https://stackoverflow.com/questions/12374132/coredata-is-there-a-good-way-to-upsert-items/47740579#47740579)
on stackoverflow. I found that [article](https://www.upbeat.it/upsert-in-core-data/) which explains that is possible to make an upsert
on *Core Data*.

Basically, better than

```swift

if (record.exists?) {
	// update code here
} else {
	// insert code here
}

```

To make an upsert you have to:

1. Create a constraint on the name attribute. If you want that the `attribute x` be unique, you put it as an constraint.
Like, if you have an object of **Products** which have the `productBarCode` as something unique. You can put it as an constraint.

2. Specify the TrumpMergePolicy in the Core Data context. Like:

```swift

managedObjectContext.mergePolicy = NSMergeByPropertyObjectTrumpMergePolicy

```
## Core data Unit Tests

TODO: Write about core data unit test

- How to write a test using coredata?
- How can use coredata on xtests?
