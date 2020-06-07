---
title: Networking with Retrofit
layout: post
date: 2020-06-07 15:10:06
categories: Small Android Network
---

# Retrofit

- [Github](https://github.com/square/retrofit)

If you want to know the the status code of the request response. And you are using the [RxJava2 adpater](https://github.com/square/retrofit/tree/master/retrofit-adapters/rxjava2)
you could do something like:

```java
import io.reactivex.Single;
import okhttp3.ResponseBody;
import retrofit2.Response;
import retrofit2.http.Body;
import retrofit2.http.POST;

@POST("api/something")
Single<Response<ResponseBody>> apiCall(@Body body);

```

and then:

```java

import android.util.Log;

import java.util.ArrayList;
import io.reactivex.Single;
import io.reactivex.schedulers.Schedulers;

public class Remote {
   public static Single<Boolean> createNew(Body body) {
       return RestClient.createService(ItemContract.class)
               .apiCall(body)
               .observeOn(Schedulers.io())
               .flatMap(response -> {
                   if (response.isSuccessful()) {
                           return Single.just(true);
                       }
                   return Single.just(false);
               }).doOnError(Throwable::printStackTrace);
   }
}


```

**ResponseBody** is useful when you do not really care about what you want retrieve from the api request call.
Or, if you want to parse the response manually.
