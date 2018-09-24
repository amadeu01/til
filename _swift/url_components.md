I just found out that something like that works:

```swift

func doRequestWithAuth() {
        dataTask?.cancel()
        if var urlComponents = URLComponents(string: "http://myuser:mypass@mydomain/") {
            urlComponents.query = "request=mysquery"
            guard let url = urlComponents.url else { return }
            dataTask = defaultSession.dataTask(with: url) { data, response, error in
                defer { self.dataTask = nil }
                if let error = error {
                      [...]
                } else if let data = data,
                    let response = response as? HTTPURLResponse,
                    response.statusCode == 200 {
                       [...]
                }
            }
            dataTask?.resume()
        }
    }
```
