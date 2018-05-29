---
title: Error Handling
layout: default
---

# Error Handling Operators

There are a variety of operators that you can use to react to or recover from onError notifications from Observables. For example, you might:

- swallow the error and switch over to a backup Observable to continue the sequence
- swallow the error and emit a default item
- swallow the error and immediately try to restart the failed Observable
- swallow the error and try to restart the failed Observable after some back-off interval
- The following pages explain these operators.

1. onErrorResumeNext( ) — instructs an Observable to emit a sequence of items if it encounters an error
2. onErrorReturn( ) — instructs an Observable to emit a particular item when it encounters an error
3. onExceptionResumeNext( ) — instructs an Observable to continue emitting items after it encounters an exception (but not another variety of throwable)
4. retry( ) — if a source Observable emits an error, resubscribe to it in the hopes that it will complete without error
5. retryWhen( ) — if a source Observable emits an error, pass that error to another Observable to determine whether to resubscribe to the source


## Usage

- With `onErrorReturn`, you will change an exception for another item on your stream.
So, it might be useful to flag that item as useless and then use filter to ignore that item that was a replace for your
exception.

```java
.onErrorReturn(throwable -> new EmptyObject())
```


## Why did I find that content?

I was looking up a way to handle retrofit error without interrupt my stream of data. Then, I found that [question](https://stackoverflow.com/questions/40188325/rxjava-database-and-remote-server).
Which lead me to that [repository](https://github.com/ReactiveX/RxJava/wiki/Error-Handling-Operators)
which I had already seen, but never given the right attention to it.
