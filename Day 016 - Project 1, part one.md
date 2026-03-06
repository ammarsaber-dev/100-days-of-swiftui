**March 03, 2026** • [Day 015 - Swift Review](Day%20015%20-%20Swift%20review.md) ← → [Day 017 - Project 1, part two](Day%20017%20-%20Project%201,%20part%20two.md)

We're building **WeSplit** — a check-splitting app that calculates how much each person pays based on the bill, tip, and number of people. This project isn't complicated; its real purpose is to teach the basics of SwiftUI in a practical way.

Each project is broken into three stages:

1. A hands-on introduction to all new techniques in isolation _(today)_
2. A step-by-step guide to build the project
3. Challenges to complete on your own — no solutions provided, because these are there to test you

---

## Putting It All Together

A single view using every concept from today — showing how `Form`, `NavigationStack`, `@State`, two-way bindings, and `ForEach` all connect in practice:

```swift
struct ContentView: View {
    // @State — simple properties stored outside the struct, can be modified freely
    @State private var name = ""
    @State private var tapCount = 0
    @State private var selectedStudent = "Harry"
    // constant — no @State needed
    let students = ["Harry", "Hermione", "Ron"]

    var body: some View {
	    // adds nav bar + fixes safe area scrolling
        NavigationStack {
            Form {
                Section("Your details") {
                    TextField("Enter your name", text: $name)  // $name  — two-way binding
                    Text("Your name is \(name)")               //  name  — read only
                }

                Section("Tap counter") {
                    Button("Tap Count: \(tapCount)") {
                        tapCount += 1 // @State lets this update freely
                    }
                }

                Section("Pick a student") {
                    Picker("Student", selection: $selectedStudent) { // two-way binding
	                    // \.self — strings are their own unique ID
                        ForEach(students, id: \.self) {
                            Text($0)
                        }
                    }
                }
            }
            .navigationTitle("WeSplit") // attached to Form, not NavigationStack
            .navigationBarTitleDisplayMode(.inline) // small title style
        }
    }
}
```

---

## Project Setup

Create a new Xcode project with these settings:

- **Product Name:** WeSplit
- **Organization Identifier:** your reversed domain (e.g. `com.hackingwithswift`) or make one up — `me.yourlastname.yourfirstname` is perfectly fine
- **Interface:** SwiftUI
- **Language:** Swift
- **Storage:** None
- All checkboxes unchecked

> 💡 Apple identifies apps uniquely using a **Bundle Identifier** — a combination of your organization identifier and product name (e.g. `com.apple.keynote`). This ensures every app on the App Store can be identified uniquely.

When you're ready, click Next, then choose somewhere to save your project and click Create. Xcode will think for a second or two, then create your project and open some code ready for you to edit.

All our work for this project will take place in `ContentView.swift`. We'll start by using it as a sandbox to try out new concepts before building the real app.

---

## Basic Structure of a SwiftUI App

When you create a new SwiftUI project, you get these files:

- **WeSplitApp.swift** — launches the app; place anything that needs to live for the app's entire lifetime here
- **ContentView.swift** — the initial UI; where all our work happens for this project
- **Assets.xcassets** — asset catalog for images, colors, app icons, iMessage stickers, and more
- **Preview Content/Preview Assets.xcassets** — example images specifically for designing UI in the canvas

> 💡 You can control whether file extensions show in the project navigator via **Xcode Preferences → General → File Extensions**.

All our work for this project will take place in `ContentView.swift`, which Xcode will already have opened for you. It has some comments at the top — those things marked with two slashes — which are ignored by Swift, so you can use them to add explanations about how your code works.

Below the comments are maybe 15 lines of code:

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack {
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundStyle(.tint)
            Text("Hello, world!")
        }
        .padding()
    }
}

#Preview {
    ContentView()
}
```

`import SwiftUI` — loads the SwiftUI framework. Apple provides many frameworks (machine learning, audio playback, image processing, and more), and rather than loading everything we import only what we need.

`struct ContentView: View` — creates a new struct conforming to the `View` protocol. `View` comes from SwiftUI and is the basic protocol that must be adopted by anything you want to draw on screen — all text, buttons, images, and more are views, including your own layouts.

`var body: some View` — a computed property returning `some View` (an opaque return type). Behind the scenes this returns a very complicated data type based on everything in the layout, but `some View` means we don't need to worry about that. The `View` protocol has only one requirement: a computed property called `body` that returns `some View`.

`VStack` — arranges the globe image and text vertically. The globe image comes from Apple's SF Symbols icon set, which has thousands of icons built right into iOS. Text views are simple pieces of static text that automatically wrap across multiple lines as needed.

`imageScale()`, `foregroundStyle()`, `padding()` — these are **modifiers**. Modifiers are regular methods with one small difference: they always return a new view that contains both your original data plus the extra modification you asked for. So the globe image will be shown larger and in blue, and the whole `body` property returns a padded view, not just a regular one.

`#Preview` — shows a live preview in Xcode's canvas. It won't form part of your final app — it's specifically for Xcode to use during development. You can customize it freely and it will only affect the canvas, not the actual app.

The canvas automatically previews using one specific device (e.g. iPhone 15 Pro). To change it, click the device name at the top center of your Xcode window and select an alternative.

> 💡 If the canvas isn't visible, go to **Editor → Canvas**. If the preview is paused, press **Option+Cmd+P** to refresh it — you'll use this shortcut a lot.

---

## Form

`Form` is a dedicated SwiftUI view type for user input — a scrolling list of static controls like text and images, but also interactive controls like text fields, toggles, buttons, and more:

```swift
var body: some View {
    Form {
        Text("Hello, world!")
    }
}
```

If you're using Xcode's canvas, you'll see it change quite dramatically: before Hello World was centered on a white screen, but now the screen is a light gray, and Hello World appears in the top left in white. What you're seeing is the beginnings of a list of data, just like you'd see in the Settings app.

You can add as many items as you want — even 10 rows works just fine. To split a form into visual chunks — just like the Settings app does — use `Section`:

```swift
Form {
    Section {
        Text("Hello, world!")
    }

    Section {
        Text("Hello, world!")
        Text("Hello, world!")
    }
}
```

There's no hard rule for when to use sections — they're just there to group related items visually.

---

## NavigationStack

iOS allows content to be placed anywhere on screen, including under the system clock and the home indicator. SwiftUI's **safe area** prevents this by default — on an iPhone 15, it spans from just below the dynamic island down to just above the home indicator.

Forms scroll, which means rows can slide under the clock and become hard to read. A common fix is adding a navigation bar at the top using `NavigationStack`:

```swift
var body: some View {
    NavigationStack {
        Form {
            Section {
                Text("Hello, world!")
            }
        }
    }
}
```

Usually you'll want a title in the navigation bar. Attach `.navigationTitle()` to the content _inside_ `NavigationStack`, not to `NavigationStack` itself — SwiftUI creates a new form that has the title plus all existing content:

```swift
NavigationStack {
    Form {
        Section {
            Text("Hello, world!")
        }
    }
    .navigationTitle("SwiftUI")
}
```

By default the title uses a large font. Add `.navigationBarTitleDisplayMode(.inline)` for a smaller title — just like how the Settings app shows large titles on the first screen and small titles on subsequent screens:

```swift
.navigationBarTitleDisplayMode(.inline)
```

> 💡 You can run the app in the iOS simulator by pressing the Play button in the top-left corner of Xcode, or pressing **Cmd+R**.

---

## @State — Modifying Program State

There's a saying among SwiftUI developers: "views are a function of their state." This means the way your UI looks — what users can see and interact with — is determined by your program's current state. Think of a fighting game: your lives, score, and weapons are all state — the active collection of settings that describe how the game is right now.

Let's put this into practice with a button, which in SwiftUI can be created with a title string and an action closure that gets run when the button is tapped:

```swift
struct ContentView: View {
    var tapCount = 0          // ❌ Can't change inside body

    var body: some View {
        Button("Tap Count: \(tapCount)") {
            tapCount += 1
        }
    }
}
```

The fix is `@State` — a **property wrapper** that stores the value outside the struct in a place SwiftUI can modify freely:

```swift
struct ContentView: View {
    @State private var tapCount = 0   // ✅

    var body: some View {
        Button("Tap Count: \(tapCount)") {
            tapCount += 1
        }
    }
}
```

`@State` allows us to work around the limitation of structs: we know we can't change their properties because structs are fixed, but `@State` allows that value to be stored separately by SwiftUI in a place that can be modified. SwiftUI destroys and recreates view structs frequently, so keeping them small and simple is important for performance — that's also why we use structs rather than classes here.

There are several ways of storing program state in SwiftUI, and you'll learn all of them. `@State` is specifically designed for simple properties stored in one view.

> 💡 Apple recommends marking `@State` properties as `private` — they should only be used inside that one view: `@State private var tapCount = 0`.

---

## Two-Way Binding with $

`@State` lets us modify view structs freely, but user interface controls like text fields need something more: they need to both _show_ a value and _write back_ any changes the user makes — this is called a **two-way binding**.

Building up to the solution step by step — this won't compile because SwiftUI wants to know where to store the text in the text field:

```swift
struct ContentView: View {
    var body: some View {
        Form {
            TextField("Enter your name")
            Text("Hello, world!")
        }
    }
}
```

Adding a `name` property gets closer, but still won't work — Swift needs to be able to update `name` to match what the user types:

```swift
struct ContentView: View {
    var name = ""

    var body: some View {
        Form {
            TextField("Enter your name", text: name)  // ❌ still won't compile
            Text("Hello, world!")
        }
    }
}
```

Making it `@State` still isn't enough:

```swift
@State private var name = ""
// ❌ still won't compile — passing name directly only reads it
```

The problem is that Swift differentiates between "show the value of this property here" and "show the value of this property here, but write any changes back to the property." SwiftUI needs to ensure that whatever is in the text field is also in the `name` property — fulfilling the promise that views are a function of their state.

The fix: prefix the property with `$` to create a two-way binding. This tells Swift to read the value _and_ write changes back as they happen:

```swift
struct ContentView: View {
    @State private var name = ""

    var body: some View {
        Form {
            TextField("Enter your name", text: $name)  // ✅ two-way binding
            Text("Your name is \(name)")                // no $ — read only
        }
    }
}
```

Notice `Text` uses `name` without `$` — we only want to read the value there, not write it back. When you see `$` before a property name, it creates a two-way binding: the value is both read and written.

---

## ForEach & Picker

`ForEach` is a dedicated SwiftUI view that loops over arrays and ranges, creating as many views as needed. It runs a closure once for every item, passing in the current loop item:

```swift
Form {
    ForEach(0..<100) { number in
        Text("Row \(number)")
    }
}

// Shorthand with $0
Form {
    ForEach(0..<100) {
        Text("Row \($0)")
    }
}
```

`ForEach` is particularly useful with `Picker`, which lets users select from a list of options:

```swift
struct ContentView: View {
    let students = ["Harry", "Hermione", "Ron"]  // constant — no @State needed
    @State private var selectedStudent = "Harry"

    var body: some View {
        NavigationStack {
            Form {
                Picker("Select your student", selection: $selectedStudent) {
                    ForEach(students, id: \.self) {
                        Text($0)
                    }
                }
            }
        }
    }
}
```

A few things worth clarifying:

- `students` doesn't need `@State` because it's a constant — it won't change
- `selectedStudent` starts as `"Harry"` but can change, so it's marked `@State`
- The `Picker` label `"Select your student"` tells users what it does and is also read aloud by screen readers
- The `Picker` has a two-way binding to `selectedStudent` via `$selectedStudent`

### Why `id: \.self`?

SwiftUI needs to be able to identify every view on screen uniquely so it can detect when things change — for example, if we rearranged our array so Ron came first, SwiftUI would move his text view at the same time.

For an array of structs, we might point to a unique property like an `id` integer. Here we have a plain string array — the only thing unique about each string is the string itself, so `\.self` means "the strings themselves are the unique identifiers." This works fine as long as there are no duplicates in the array.

We'll look at other ways to use `ForEach` in the future, but that's enough for this project.

> 💡 Before moving on to building the real project, reset `ContentView.swift` back to a clean slate:
> 
> ```swift
> import SwiftUI
> 
> struct ContentView: View {
>     var body: some View {
>         Text("Hello, world!")
>     }
> }
> 
> #Preview {
>     ContentView()
> }
> ```

---

**Topics:** [WeSplit Introduction](https://www.hackingwithswift.com/books/ios-swiftui/wesplit-introduction) • [SwiftUI App Structure](https://www.hackingwithswift.com/books/ios-swiftui/understanding-the-basic-structure-of-a-swiftui-app) • [Form](https://www.hackingwithswift.com/books/ios-swiftui/creating-a-form) • [NavigationStack](https://www.hackingwithswift.com/books/ios-swiftui/adding-a-navigation-bar) • [@State](https://www.hackingwithswift.com/books/ios-swiftui/modifying-program-state) • [Two-Way Binding](https://www.hackingwithswift.com/books/ios-swiftui/binding-state-to-user-interface-controls) • [ForEach](https://www.hackingwithswift.com/books/ios-swiftui/creating-views-in-a-loop)