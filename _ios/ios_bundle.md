---
title: iOS Internals
layout: post
date: 2020-06-07 18:11:09
categories: iOS Mobile CLI
---

## 🤔 How to get iOS Bundle from command line?

```bash
xcodebuild -showBuildSettings | grep PRODUCT_BUNDLE_IDENTIFIER
```

I got it from [here](https://stackoverflow.com/questions/32446065/xcode-script-get-bundle-id-from-build-settings-instead-of-info-plist)
