# Instrumented Test

Instrumented unit tests are tests that run on physical devices and emulators, 
and they can take advantage of the Android framework APIs and supporting APIs, such as Android Test. 
You should create instrumented unit tests if your tests need access to instrumentation information (such as the target app's Context) 
or if they require the real implementation of an Android framework component (such as a Parcelable or SharedPreferences object).

Take care with Android Studio. Sometimes, if you have the file with same name on `test` and `androidTest` folders. 
It might think that you want to run one test instead of other. So, it might generate undesirable behavior. 
So, take extra care with naming.

# Unit Test

# JUnit

# Test Orchestrator

When using AndroidJUnitRunner version 1.0 or higher, you have access to a tool called Android Test Orchestrator, 
which allows you to run each of your app's tests within its own invocation of Instrumentation.

Android Test Orchestrator offers the following benefits for your testing environment:

- Minimal shared state. Each test runs in its own Instrumentation instance. Therefore, if your tests share app state, most of that shared state is removed from your device's CPU or memory after each test.
To remove all shared state from your device's CPU and memory after each test, use the clearPackageData flag.

- Crashes are isolated. Even if one test crashes, it takes down only its own instance of Instrumentation, so the other tests in your suite still run.

Config 

```groovy
android {
  defaultConfig {
   ...
   testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"

   // The following argument makes the Android Test Orchestrator run its
   // "pm clear" command after each test invocation. This command ensures
   // that the app's state is completely cleared between tests.
   testInstrumentationRunnerArguments clearPackageData: 'true'
 }

  testOptions {
    execution 'ANDROID_TEST_ORCHESTRATOR'
  }
}

dependencies {
  androidTestImplementation 'com.android.support.test:runner:1.0.2'
  androidTestUtil 'com.android.support.test:orchestrator:1.0.2'
}

```
