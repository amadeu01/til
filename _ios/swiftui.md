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
