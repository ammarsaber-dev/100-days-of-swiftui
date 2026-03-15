**March 13, 2026** • [Day 023 - Project 3, part 1](Day%20023%20-%20Project%203,%20part%201.md) ← → [Day 025 - Milestone Projects 1-3](Day%20025%20-%20Milestone%20Projects%201-3.md)

Views and modifiers are the fundamental building blocks of any SwiftUI app. This wrap-up session focuses on solidifying that foundation through three challenges that revisit earlier projects.

## Views and Modifiers: Wrap Up

### Challenge

1. Go back to project 1 and use a conditional modifier to change the total amount text view to red if the user selects a 0% tip.
2. Go back to project 2 and replace the `Image` view used for flags with a new `FlagImage()` view that renders one flag image using the specific set of modifiers we had.
3. Create a custom `ViewModifier` (and accompanying `View` extension) that makes a view have a large, blue font suitable for prominent titles in a view.

---

## My Solutions

### Challenge 1 — Conditional modifier for 0% tip (Project 1)

In WeSplit, apply a conditional `.foregroundStyle` to the total amount `Text` view:

```swift
Section("Total Amount") {
    Text(totalAmount, format: .currency(code: Locale.current.currency?.identifier ?? "USD"))
        .foregroundStyle(tipPercentage == 0 ? .red : .primary)
}
```

### Challenge 2 — `FlagImage()` view (Project 2)

In GuessTheFlag, extract the flag `Image` into its own view and replace all flag `Button` labels with it:

```swift
struct FlagImage: View {
    var country: String
    var body: some View {
        Image(country)
            .clipShape(.capsule)
            .shadow(radius: 5)
    }
}
```

### Challenge 3 — Custom `ViewModifier` for prominent titles

```swift
//
//  ContentView.swift
//  ViewsAndModifiers
//
//  Created by Ammar Saber on 12/03/2026.
//

import SwiftUI

struct ProminentTitle: ViewModifier {
    func body(content: Content) -> some View {
        content
            .font(.largeTitle)
            .foregroundStyle(.blue)
    }
}

extension View {
    func largeBlueFont() -> some View {
        modifier(ProminentTitle())
    }
}

struct ContentView: View {
    
    var body: some View {
        Text("Hello, World!")
            .largeBlueFont()
    }
}

#Preview {
    ContentView()
}
```