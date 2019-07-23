When I had to test with iOS build and detox, the annoying yellow box was blocking the automated test.
So, I found out about the flag the react-native uses in their iOS-environment [here](https://github.com/facebook/react-native/blob/cf77067f0c4eda30f814aafa45ddf962eadcfbdb/React/Base/RCTUtils.m#L467-L476).

I realise that I only had to set `IS_TESTING` to `true`.

To change environment variable with `xcodebuild`, the only thing you have to do is: to add the environment variable and its value to the end of the command
like:
```bash
$ xcodebuild -project ios/test.xcodeproj -scheme test -configuration Debug IS_TESTING=1
```



```json
"detox": {
    "configurations": {
      "ios.sim.debug": {
        "binaryPath": "ios/build/Build/Products/Debug-iphonesimulator/test.app",
        "build": "xcodebuild -project ios/test.xcodeproj -scheme test -configuration Debug -sdk iphonesimulator -derivedDataPath ios/build IS_TESTING=1",
        "type": "ios.simulator",
        "name": "iPhone X"
      },
      "ios.sim.release": {
        "binaryPath": "ios/build/Build/Products/Debug-iphonesimulator/test.app",
        "build": "xcodebuild -project ios/test.xcodeproj -scheme test -configuration Release -sdk iphonesimulator -derivedDataPath ios/build IS_TESTING=1",
        "type": "ios.simulator",
        "name": "iPhone X"
      },
```
