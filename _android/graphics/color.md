---
title: Android Color Resources
layout: post
date: 2020-06-07 15:07:27
categories: Small Android
---

# Colors from [android.graphics.Color](https://developer.android.com/reference/android/graphics/Color)

I might be wrong, but it seems that that the only methods that return `Color` class are:

- Color convert(ColorSpace colorSpace)
- Color valueOf(float r, float g, float b) - added in API level 26
- Color valueOf(float r, float g, float b, float a) - added in API level 26
- Color valueOf(float[] components, ColorSpace colorSpace) - added in API level 26
- Color valueOf(int color) - added in API level 26
- Color valueOf(long color) - added in API level 26
- Color valueOf(float r, float g, float b, float a, ColorSpace colorSpace) - added in API level 26

You cannot do something like `Color(0xf59563)` because the constructor available for `Color` is

```java
public Color() {...}
```

It seems that if you build your app for old versions of android as well, you cannot use `valueOf(...)`.
So, what is available for you is:

1.  Use resources `colors.xml` to store your color, then use like that:

```java

       View view = new View(getBaseContext());
       int colorIntRepresentation = getResources().getColor(R.color.colorPrimary);
       view.setBackgroundColor(colorIntRepresentation);
```

2.  Use `Color` static method `Color.parseColor()`, then use the int result to pass it forward. Such as:

```java
       View view = new View(getBaseContext());
       int colorIntRepresentation = Color.parseColor("#3F51B5");
       view.setBackgroundColor(colorIntRepresentation);
```

However, if I do want to use the `Color` class? Then, I guess you have to put your _min_ **android** requirement to _26_.
