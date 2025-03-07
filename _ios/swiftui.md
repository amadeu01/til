# TIL

## NavigationView by NavigationStack

### Summary: When to Use Which?

| Feature                                   | NavigationView (Old) | NavigationStack (New) |
|-------------------------------------------|----------------------|----------------------|
| *Simple navigation*                     | :white_check_mark: Works            | :white_check_mark: Works            |
| *Programmatic navigation (isActive)*  | :white_check_mark: Works            | :x: Must use NavigationPath |
| *Deep linking / Dynamic paths*          | :x: Limited          | :white_check_mark: Recommended |
| *Split view (iPad/macOS)*               | :white_check_mark: Works            | :x: Use NavigationSplitView instead |
| *Navigation bar styling*                | :white_check_mark: Works            | :warning: Requires .toolbar{} adjustments |


## Animation inside computed var and switches 

### Why It Happens:

- **View Identity:** SwiftUI relies on the identity of views to determine what has changed. When the switch is in the main body, the transitions are applied because SwiftUI can track which views are appearing or disappearing.

- **Computed Views and @ViewBuilder:** When you extract your view logic into a computed property with @ViewBuilder, the system may lose the context needed to apply transitions, because it treats the result as a brand new view each time rather than a state change of an existing view.

How to Fix It:
1.	Move the Switch into the Main body: Keep your state-dependent view logic directly inside the body so that SwiftUI can properly animate changes.
2.	Preserve View Identity: If you must use a computed property, consider wrapping each case in a container that maintains its identity (for example, by using .id(someUniqueValue)), so that SwiftUI can detect when a view is added or removed.

```swift
import SwiftUI

struct AnimatedStateView: View {
    enum ViewState: String {
        case loading, empty, content
    }
    
    @State private var state: ViewState = .loading
    
    var body: some View {
        ZStack {
            contentForState
        }
        .animation(.easeInOut(duration: 0.5), value: state)
        .onAppear {
            simulateStateChanges()
        }
    }
    
    @ViewBuilder
    private var contentForState: some View {
        // By using a unique id based on the state, we force SwiftUI to treat each case as distinct
        Group {
            switch state {
            case .loading:
                ProgressView("Loading...")
                    .transition(.opacity)
            case .empty:
                Text("No data available")
                    .font(.title)
                    .foregroundColor(.gray)
                    .transition(.opacity)
            case .content:
                VStack {
                    Text("Here is the content! 🎉")
                        .font(.title)
                        .padding()
                    Image(systemName: "checkmark.circle.fill")
                        .resizable()
                        .frame(width: 80, height: 80)
                        .foregroundColor(.green)
                }
                .transition(.opacity)
            }
        }
        .id(state.rawValue) // Using the raw value ensures unique identity per state
    }
    
    private func simulateStateChanges() {
        DispatchQueue.main.asyncAfter(deadline: .now() + 1.5) {
            withAnimation { state = .empty }
        }
        DispatchQueue.main.asyncAfter(deadline: .now() + 3.0) {
            withAnimation { state = .content }
        }
    }
}

struct AnimatedStateView_Previews: PreviewProvider {
    static var previews: some View {
        AnimatedStateView()
    }
}
```


### Make Swift.Error Hashable

```swift
struct ErrorWrapper: Error, Hashable {
        let underlyingError: Error
        
        init(_ error: Error) {
            self.underlyingError = error
        }
        
        // Hashable conformance
        func hash(into hasher: inout Hasher) {
            hasher.combine(underlyingError.localizedDescription)
        }
        
        // Equatable conformance
        static func == (lhs: ErrorWrapper, rhs: ErrorWrapper) -> Bool {
            lhs.underlyingError.localizedDescription == rhs.underlyingError.localizedDescription
        }
    }
```

## TIL (Today I Learned) - March 6, 2025

When trying to truncate `Text` in SwiftUI without showing ellipses (`...`), I found out that there's no native support for something like `.truncationMode(.none)`. Surprisingly, SwiftUI doesn't provide a straightforward way to truncate the text by simply ignoring the last overflowing word—it's either truncated with ellipses or fully displayed.

The workaround involves creating a custom wrapper using UIKit's `UILabel` to manually measure text size and decide exactly where to truncate, thereby preventing the ellipses from showing up.

Here's an example of a custom SwiftUI view using UIKit internally to handle this:

```swift
import UIKit
import SwiftUI

struct TruncatedText: View {
    let text: String
    let lineLimit: Int

    @State private var truncatedText: String = ""

    var body: some View {
        GeometryReader { geometry in
            Text(truncatedText)
                .lineLimit(lineLimit)
                .onAppear {
                    truncatedText = truncateText(
                        text,
                        lineLimit: lineLimit,
                        maxWidth: geometry.size.width
                    )
                }
        }
    }

    func truncateText(_ text: String, lineLimit: Int, maxWidth: CGFloat) -> String {
        let words = text.split(separator: " ")
        var truncated = ""

        for word in words {
            let testText = truncated.isEmpty ? String(word) : truncated + " " + word

            let uiLabel = UILabel()
            uiLabel.numberOfLines = lineLimit
            uiLabel.lineBreakMode = .byTruncatingTail
            uiLabel.font = UIFont.systemFont(ofSize: 17)
            uiLabel.text = testText

            let size = uiLabel.sizeThatFits(CGSize(width: maxWidth, height: .greatestFiniteMagnitude))

            if size.height > uiLabel.font.lineHeight * CGFloat(lineLimit) {
                break
            } else {
                truncated = testText
            }
        }

        return truncated
    }
}
```

I think UIKit handled this scenario better with simpler native solutions compared to SwiftUI.

### #swiftUI #iOSDevelopment #UIKit #TIL


### Show SwiftUI using Xcode Playgrounds

```swift
PlaygroundPage.current.setLiveView(
    MyView()
        .frame(width: 300, height: 200) // make sure you use some frame here
)

```
