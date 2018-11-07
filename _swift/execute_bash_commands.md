
This is a way of execute bash commands with swift  :D

```swift
@discardableResult
func shell(_ args: String...) -> Int32 {
    let task = Process()
    task.launchPath = "/usr/bin/env"
    task.arguments = args
    task.launch()
    task.waitUntilExit()
    return task.terminationStatus
}
```

One way of using it is:

```swift
let jsonEncoder = JSONEncoder()

struct Result: Codable {
    var score: Float = 100
}
var result = Result()

func writeResult() {
    let jsonData = try! jsonEncoder.encode(result)
    let jsonString = String(data: jsonData, encoding: .utf8)
    shell("/bin/sh", "-c", "echo '\(jsonString!)\' > result.json")
}

```
