- [Link](https://github.com/kaushikgopal/RxJava-Android-Samples#9-pseudo-caching--retrieve-data-first-from-a-cache-then-a-network-call-using-concat-concateager-merge-or-publish)

> We have two source Observables: a disk (fast) cache and a network (fresh) call. Typically the disk Observable is much faster than the network Observable. But in order to demonstrate the working, we've also used a fake "slower" disk cache just to see how the operators behave.
>
> This is demonstrated using 4 techniques:
>
> - .concat
> - .concatEager
> - .merge
> - .publish selector + merge + takeUntil
>
> The 4th technique is probably what you want to use eventually but it's interesting to go through the progression of techniques, to understand why.
>
> concat is great. It retrieves information from the first Observable (disk cache in our case) and then the subsequent network Observable. Since the disk cache is presumably faster, all appears well and the disk cache is loaded up fast, and once the network call finishes we swap out the "fresh" results.
>
> The problem with concat is that the subsequent observable doesn't even start until the first Observable completes. That can be a problem. We want all observables to start simultaneously but produce the results in a way we expect. Thankfully RxJava introduced concatEager which does exactly that. It starts both observables but buffers the result from the latter one until the former Observable finishes. This is a completely viable option.
>
> Sometimes though, you just want to start showing the results immediately. Assuming the first observable (for some strange reason) takes really long to run through all its items, even if the first few items from the second observable have come down the wire it will forcibly be queued. You don't necessarily want to "wait" on any Observable. In these situations, we could use the merge operator. It interleaves items as they are emitted. This works great and starts to spit out the results as soon as they're shown.
>
> Similar to the concat operator, if your first Observable is always faster than the second Observable you won't run into any problems. However the problem with merge is: if for some strange reason an item is emitted by the cache or slower observable after the newer/fresher observable, it will overwrite the newer content. Click the "MERGE (SLOWER DISK)" button in the example to see this problem in action. @JakeWharton and @swankjesse contributions go to 0! In the real world this could be bad, as it would mean the fresh data would get overridden by stale disk data.
>
> To solve this problem you can use merge in combination with the super nifty publish operator which takes in a "selector". I wrote about this usage in a blog post but I have Jedi JW to thank for reminding of this technique. We publish the network observable and provide it a selector which starts emitting from the disk cache, up until the point that the network observable starts emitting. Once the network observable starts emitting, it ignores all results from the disk observable. This is perfect and handles any problems we might have.
>
> Previously, I was using the merge operator but overcoming the problem of results being overwritten by monitoring the "resultAge". See the old PseudoCacheMergeFragment example if you're curious to see this old implementation.


## Why did I find that content?

I had a problem where I wanted to retrieve data from source network, but I wanted to try to retrieve from localDataStore 
if that was unavailable by my network provider. So, I found that [question](https://stackoverflow.com/questions/40188325/rxjava-database-and-remote-server)
which leads me to that [repository](https://github.com/kaushikgopal/RxJava-Android-Samples) where they have very nice real world
use-cases exemples for rxjava.

One of then, which helped me today:

```java

 private Observable<Contributor> getFreshNetworkData() {
    String githubToken = getResources().getString(R.string.github_oauth_token);
    GithubApi githubService = GithubService.createGithubService(githubToken);

    return githubService
        .contributors("square", "retrofit")
        .flatMap(Observable::fromIterable)
        .doOnSubscribe(
            (data) ->
                new Handler(Looper.getMainLooper()) //
                    .post(() -> adapterSubscriptionInfo.add("(network) subscribed"))) //
        .doOnComplete(
            () ->
                new Handler(Looper.getMainLooper()) //
                    .post(() -> adapterSubscriptionInfo.add("(network) completed")));
  }
            
 @OnClick(R.id.btn_pseudoCache_mergeOptimized)
  public void onMergeOptimizedBtnClicked() {
    infoText.setText(R.string.msg_pseudoCache_demoInfo_mergeOptimized);
    wireupDemo();

    getFreshNetworkData() //
        .publish(
            network -> //
            Observable.merge(
                    network, //
                    getCachedDiskData().takeUntil(network)))
        .subscribeOn(Schedulers.io()) // we want to add a list item at time of subscription
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(
            new DisposableObserver<Contributor>() {
              @Override
              public void onComplete() {
                Timber.d("done loading all data");
              }

              @Override
              public void onError(Throwable e) {
                Timber.e(e, "arr something went wrong");
              }

              @Override
              public void onNext(Contributor contributor) {
                contributionMap.put(contributor.login, contributor.contributions);
                adapterDetail.clear();
                adapterDetail.addAll(mapAsList(contributionMap));
              }
            });
  }

  @OnClick(R.id.btn_pseudoCache_mergeOptimizedSlowDisk)
  public void onMergeOptimizedWithSlowDiskBtnClicked() {
    infoText.setText(R.string.msg_pseudoCache_demoInfo_mergeOptimizedSlowDisk);
    wireupDemo();

    getFreshNetworkData() //
        .publish(
            network -> //
            Observable.merge(
                    network, //
                    getSlowCachedDiskData().takeUntil(network)))
        .subscribeOn(Schedulers.io()) // we want to add a list item at time of subscription
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(
            new DisposableObserver<Contributor>() {
              @Override
              public void onComplete() {
                Timber.d("done loading all data");
              }

              @Override
              public void onError(Throwable e) {
                Timber.e(e, "arr something went wrong");
              }

              @Override
              public void onNext(Contributor contributor) {
                contributionMap.put(contributor.login, contributor.contributions);
                adapterDetail.clear();
                adapterDetail.addAll(mapAsList(contributionMap));
              }
            });
  }
```
