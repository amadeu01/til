---
title: ADB | Adding text into emulator App
layout: post
date: 2020-06-07 14:57:43
categories: Small Android CLI
---

How to input a text like `text here` into a field on an application that is running on Android device available on adb.
So, first be sure you have the device on your list by using:

```bash
$ adb devices
```

### Input text

```bash
❯ adb shell input text "text here"
```
