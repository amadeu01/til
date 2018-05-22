Do not break chain.

Use compose() operators. Create a new transformer.

```Java
<T> Transformer<T, T> applySchedulers() {
  return new Transformer<T, T>() {
    public Observable<T> call(Observable<T> observable) {
      return observable.subscribeOn(Schedulers.io()).observeOn(AndroidSchedulers.mainThread());
    }
  }
}
```

[Source](http://blog.danlew.net/2015/03/02/dont-break-the-chain/)
