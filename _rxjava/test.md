---
title: RxJava tests troubles and solutions
layout: default
---

# This is dedicated to test with RxJava

## How to test something async with JUnit?

I don't know, but I hope I get this response.

## Problems with [Room](https://developer.android.com/topic/libraries/architecture/room)

I had some problem such as an Sigle, like:

```java
    @Query("SELECT * FROM table_x WHERE something = 1") // True = 0
    Single<List<Item>> findItems();
```

I tried something, like:

```java
Single<List<Item>> single = mDb.ItemDao().findItems();
single.doOnSuccess(list -> somethingElse());
```

But my onOnSeccess was called with empty list. I do not know why.
Nevertheless, if I do something, like:

```java
long[] ItemIds = mDb.ItemDao().insert(Item);
Item item = mDb.ItemDao().findItemById(ItemIds[0])
```

The item returned is not null. And I got the item.

The same problem occoured when I tried with `.blockingGet()` function.


## Problem with `Schedulers`

I had some problems with `Schedulers` and threads where the things was going on.
So, I fixed but changing the default implementations of schedulers that I had used on the app.

```java
    @BeforeClass
    public static void setupClass() {
        Scheduler immediate = new Scheduler() {
            @Override
            public Disposable scheduleDirect(@NonNull Runnable run, long delay, @NonNull TimeUnit unit) {
                // this prevents StackOverflowErrors when scheduling with a delay
                return super.scheduleDirect(run, 0, unit);
            }

            @Override
            public Worker createWorker() {
                return new ExecutorScheduler.ExecutorWorker(Runnable::run);
            }
        };

        RxAndroidPlugins.setInitMainThreadSchedulerHandler(__ -> immediate);
        RxJavaPlugins.setInitIoSchedulerHandler(__ -> immediate);
        RxJavaPlugins.setInitComputationSchedulerHandler(__ -> immediate);
        RxJavaPlugins.setInitNewThreadSchedulerHandler(__ -> immediate);
        RxJavaPlugins.setInitSingleSchedulerHandler(__ -> immediate);
    }

    @AfterClass
    public static void tearDownClass() {
        RxJavaPlugins.reset();
        RxAndroidPlugins.reset();
    }

```


## Problem the test stop before the observable emits the stuff.

This also avoid silent errors inside the observable stream.

```java

        TestObserver<Boolean> testObserver = single.test();

        testObserver.awaitTerminalEvent();

        testObserver.assertNoErrors();
```
