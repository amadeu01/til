# Use a custom framework in a playground

**For XCode 9**

Use the functionality provided by a custom framework in your playground. 
Before using a framework, you must convert your playground to a workspace.

## Convert your playground project to a workspace
* Open the playground and choose File > Save As Workspace.

## Import a custom framework

1. Open the workspace containing your playground.

2. Choose File > Add Files to Workspace Name, where Workspace Name is the name of the workspace containing the playground.

3. In the file dialog that appears, navigate to the directory containing the framework, select the framework, and click Add.

## Use a custom framework

1. In the editor, open the desired playground source file.

2. Add an import statement for the framework. For example, to use MyCustomFramework, use the following import statement:

```swift
import MyCustomFramework
```


# Important: The following conditions are required for the custom framework to work in a playground:

* The framework is in the same workspace as the playground.

* The framework has already been built.

* If it is an iOS framework, it is built for a 64-bit runtime destination.

* The workspace contains at least one active scheme that builds a target.

* If it is an Objective-C framework, it was built with the Defines Module (DEFINES_MODULE) build setting set to Yes.


# Other way.

1. Create a playground

2. Import the playground to your existing project.

3. Create a new `Cocoa Framework`

4. Put all files you want to access from playground on that framework

5. Compile that new framework you created (If you use any 3rd party library, you can add to that framework as well)

6. Import on your playground using ```@testable import CustomFramework```

