**March 12, 2026** • [Day 022 - Project 2, part 3](Day%20022%20-%20Project%202,%20part%203.md) ← → [Day 024 - Project 3, part 2](Day%20024%20-%20Project%203,%20part%202.md)

Our first **technique project** — instead of building an app, we focus on two fundamentals we've been using without fully understanding: views and modifiers. By the end there'll be a lot less magic and a lot more detail about what actually makes SwiftUI tick.

Create a new Xcode project called **ViewsAndModifiers** to follow along.

---

## Why Structs for Views?

Performance is one reason, but there's something more important.

**Performance** — structs are simpler and faster than classes. In UIKit, every view descended from `UIView` which had many properties and methods — background color, constraints, a rendering layer, and more. Every `UIView` subclass inherited all of those whether they needed them or not. In SwiftUI, all views are trivial structs — they contain exactly what you can see and nothing more. `Color.red` and `LinearGradient` are perfect examples: trivial types that hold very little data. In fact, `Color.red` holds no information other than "fill my space with red." Creating 1,000 or even 100,000 SwiftUI views happens in the blink of an eye.

**Forced clean state (the bigger reason)** — classes can change their values freely, which can lead to messier code and makes it hard for SwiftUI to know when the UI needs updating. By using structs, SwiftUI encourages a more functional design approach: views become simple, inert things that convert data into UI, rather than intelligent things that can grow out of control.

> ⚠️ If you use a class for your view your code will either not compile or crash at runtime. Always use a struct.

---

## What Is Behind the Main SwiftUI View?

Nothing — for SwiftUI developers, there is nothing behind your view. A common mistake is modifying a `VStack` with a background color and expecting it to fill the screen:

```swift
struct ContentView: View {
    var body: some View {
        VStack {
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundStyle(.tint)
            Text("Hello, world!")
        }
        .padding()
        .background(.red)
    }
}
```

That only colors the area directly behind the `VStack`'s content — not the whole screen. There is technically a `UIHostingController` that bridges UIKit and SwiftUI, but if you start modifying it your code won't work on other Apple platforms and may stop working on iOS entirely.

The correct fix is to make the `VStack` take up more space using `.frame()`:

```swift
.frame(maxWidth: .infinity, maxHeight: .infinity)
```

`maxWidth` and `maxHeight` mean "this view _can_ take up all this space" — not that it must. If you have other views around, SwiftUI will make sure they all get enough space.

---

## Why Modifier Order Matters

Almost every time we apply a modifier to a SwiftUI view, we actually create a new view with that change applied — we don't modify the existing view in place. Take a look at this code:

```swift
Button("Hello, world!") {
    // do nothing
}
.background(.red)
.frame(width: 200, height: 200)
```

You won't see a 200×200 red button — you'll see a 200×200 empty square with "Hello, world!" in the middle and a small red rectangle directly around the text. You can peek into the underbelly of SwiftUI by printing the exact type of the body:

```swift
Button("Hello, world!") {
    print(type(of: self.body))
}
.background(.red)
.frame(width: 200, height: 200)
// → ModifiedContent<ModifiedContent<Button<Text>, _BackgroundStyleModifier<Color>>, _FrameLayout>
```

Two things to notice:

- Every modifier wraps the view using generics: `ModifiedContent<OurThing, OurModifier>`
- Multiple modifiers just keep stacking: `ModifiedContent<ModifiedContent<…`

Reading the type **inside out**:

- Innermost: `ModifiedContent<Button<Text>, _BackgroundStyleModifier<Color>>` — a button with a background color applied
- Outer: `ModifiedContent<…, _FrameLayout>` — that whole thing given a larger frame

Rewriting the code to apply the background after the frame gives the result you'd expect:

```swift
Button("Hello, world!") {
    print(type(of: self.body))
}
.frame(width: 200, height: 200)
.background(.red)
```

> 💡 Think of SwiftUI as rendering after every single modifier. Once `.background(.red)` is applied, expanding the frame later won't repaint it — that was already done. This isn't actually how SwiftUI works internally (it would be a performance nightmare), but it's a neat mental shortcut while learning.

Because modifiers stack rather than replace, you can apply the same modifier multiple times — each one simply adds to whatever was there before:

```swift
Text("Hello, world!")
    .padding()
    .background(.red)
    .padding()
    .background(.blue)
    .padding()
    .background(.green)
    .padding()
    .background(.yellow)
```

---

## Why `some View`?

SwiftUI relies heavily on a Swift feature called **opaque return types**. `some View` means "one specific object that conforms to the `View` protocol, but I don't want to say exactly what type it is." The compiler knows the exact type even if we don't write it out.

**Performance** — SwiftUI needs to be able to look at the views we're showing and understand how they change, so it can correctly update the UI. `some View` gives it that extra information. Without it, SwiftUI would have to throw everything away and start again after every small change.

**`View` has an associated type** — `View` by itself doesn't mean anything, similar to how you can't say "this variable is an array" without saying what's inside it. It has a hole that must be filled. This is why returning `View` doesn't work, but returning `Text` does:

```swift
// ❌ returning View makes no sense — Swift wants to know what's inside
struct ContentView: View {
    var body: View {
        Text("Hello, world!")
    }
}

// ✅ returning Text is fine — hole filled
struct ContentView: View {
    var body: Text {
        Text("Hello, world!")
    }
}
```

When you stack modifiers the return type becomes `ModifiedContent<ModifiedContent<Button<Text>, ...>, ...>` — writing that by hand is painful. `some View` lets us skip it entirely:

```swift
// What some View lets us do — say "this will be a view" without the exact long type
Button("Hello World") {
    print(type(of: self.body))
}
.frame(width: 200, height: 200)
.background(.red)
```

**`TupleView`** — when you put multiple views inside a `VStack`, SwiftUI silently creates a `TupleView` to hold them. Two text views → `TupleView` of two. Three → `TupleView` of three. Keeps expanding up to ten views.

**`@ViewBuilder`** — `body` is automatically marked with `@ViewBuilder`, which silently wraps multiple views into one of those `TupleView` containers. That's why you can return multiple views from `body` without a stack. This isn't magic — if you right-click on the `View` protocol (in Xcode) and choose "Jump to Definition", you'll see the requirement for `body` and that it's marked with `@ViewBuilder`.

---

## Conditional Modifiers

The easiest way to apply a modifier conditionally is with the **ternary operator**:

```swift
struct ContentView: View {
    @State private var useRedText = false

    var body: some View {
        Button("Hello World") {
            useRedText.toggle()
        }
        .foregroundStyle(useRedText ? .red : .blue)
    }
}
```

SwiftUI watches `useRedText` and re-invokes `body` whenever it changes, so the color updates immediately. You can often use regular `if` conditions to return different views, but this creates more work for SwiftUI — rather than seeing one `Button` being recolored, it now sees two different `Button` views and destroys one to create the other:

```swift
// ❌ less efficient — SwiftUI sees two different Buttons
var body: some View {
    if useRedText {
        Button("Hello World") {
            useRedText.toggle()
        }
        .foregroundStyle(.red)
    } else {
        Button("Hello World") {
            useRedText.toggle()
        }
        .foregroundStyle(.blue)
    }
}
```

Sometimes `if` statements are unavoidable, but prefer the ternary where possible.

> 💡 **WTF** mnemonic for ternary order — **W**hat to check, **T**rue, **F**alse.

---

## Environment Modifiers

Many modifiers can be applied to containers, automatically applying to every child view inside. These are called **environment modifiers**. They look the same as regular modifiers from a coding perspective, but child views can override them:

```swift
// font() is an environment modifier — child override works ✅
VStack {
    Text("Gryffindor")
        .font(.largeTitle)    // overrides the .title from the VStack
    Text("Hufflepuff")
    Text("Ravenclaw")
    Text("Slytherin")
}
.font(.title)
```

Regular modifiers don't work this way — a child trying to "override" one just adds to it instead:

```swift
// blur() is a regular modifier — child blur stacks on top, doesn't replace ❌
VStack {
    Text("Gryffindor")
        .blur(radius: 0)    // total blur = 5, not 0
    Text("Hufflepuff")
    Text("Ravenclaw")
    Text("Slytherin")
}
.blur(radius: 5)
```

> 💡 There is no way of knowing ahead of time which modifiers are environment vs regular other than reading the documentation for each one individually.

---

## Views as Properties

You can create views as properties to keep `body` cleaner and avoid repetition:

```swift
struct ContentView: View {
    let motto1 = Text("Draco dormiens")
    let motto2 = Text("nunquam titillandus")

    var body: some View {
        VStack {
            motto1
            motto2
        }
    }
}
```

You can even apply modifiers directly to those properties at the use site:

```swift
VStack {
    motto1
        .foregroundStyle(.red)
    motto2
        .foregroundStyle(.blue)
}
```

> ⚠️ Swift doesn't allow a stored property to reference other stored properties — it causes problems when the object is created. This means you can't create a `TextField` bound to another stored property this way.

You can also use **computed properties**. Unlike `body`, Swift won't automatically apply `@ViewBuilder` here, so if you want to return multiple views you have three options:

```swift
// Option 1 — wrap in a stack
var spells: some View {
    VStack {
        Text("Lumos")
        Text("Obliviate")
    }
}

// Option 2 — wrap in a Group (arrangement decided at the call site, not here)
var spells: some View {
    Group {
        Text("Lumos")
        Text("Obliviate")
    }
}

// Option 3 — add @ViewBuilder yourself (preferred — mirrors how body works)
@ViewBuilder var spells: some View {
    Text("Lumos")
    Text("Obliviate")
}
```

> 💡 Prefer `@ViewBuilder` as it mirrors `body`, but be careful not to cram too much into properties — it's usually a sign that a view needs to be broken up into smaller pieces.

---

## View Composition

SwiftUI lets you break complex views into smaller custom views without any meaningful performance impact. For example, these two text views share the same styling:

```swift
struct ContentView: View {
    var body: some View {
        VStack(spacing: 10) {
            Text("First")
                .font(.largeTitle)
                .padding()
                .foregroundStyle(.white)
                .background(.blue)
                .clipShape(.capsule)

            Text("Second")
                .font(.largeTitle)
                .padding()
                .foregroundStyle(.white)
                .background(.blue)
                .clipShape(.capsule)
        }
    }
}
```

Extract the repeated styling into a custom view:

```swift
struct CapsuleText: View {
    var text: String

    var body: some View {
        Text(text)
            .font(.largeTitle)
            .padding()
            .foregroundStyle(.white)
            .background(.blue)
            .clipShape(.capsule)
    }
}
```

Then use it cleanly:

```swift
struct ContentView: View {
    var body: some View {
        VStack(spacing: 10) {
            CapsuleText(text: "First")
            CapsuleText(text: "Second")
        }
    }
}
```

You can also leave some modifiers out of the custom view and apply them at the call site for flexibility:

```swift
VStack(spacing: 10) {
    CapsuleText(text: "First")
        .foregroundStyle(.white)
    CapsuleText(text: "Second")
        .foregroundStyle(.yellow)
}
```

---

## Custom Modifiers

Create a struct conforming to `ViewModifier` — it has one requirement: a `body(content:)` method that accepts whatever content it's given and returns `some View`:

```swift
struct Title: ViewModifier {
    func body(content: Content) -> some View {
        content
            .font(.largeTitle)
            .foregroundStyle(.white)
            .padding()
            .background(.blue)
            .clipShape(.rect(cornerRadius: 10))
    }
}
```

Apply it using `.modifier()`:

```swift
Text("Hello World")
    .modifier(Title())
```

It's usually a smart idea to wrap it in a `View` extension for a cleaner call site:

```swift
extension View {
    func titleStyle() -> some View {
        modifier(Title())
    }
}

Text("Hello World")
    .titleStyle()
```

Custom modifiers can also **create entirely new view structure** — not just apply existing modifiers. Modifiers return new objects rather than modifying existing ones, so you can embed the view in a stack and add other views around it:

```swift
struct Watermark: ViewModifier {
    var text: String

    func body(content: Content) -> some View {
        ZStack(alignment: .bottomTrailing) {
            content
            Text(text)
                .font(.caption)
                .foregroundStyle(.white)
                .padding(5)
                .background(.black)
        }
    }
}

extension View {
    func watermarked(with text: String) -> some View {
        modifier(Watermark(text: text))
    }
}
```

With that in place, you can add a watermark to any view:

```swift
Color.blue
    .frame(width: 300, height: 200)
    .watermarked(with: "Hacking with Swift")
```

> 💡 The main reason to prefer a custom `ViewModifier` over a plain `View` extension: **custom modifiers can have stored properties, extensions cannot**.

---

## Custom Containers _(Bonus)_

You can build your own container views using Swift generics. `struct GridStack<Content: View>: View` means "content can be anything that conforms to `View`", and `GridStack` itself also conforms to `View`. The `content` closure accepts two integers (row and column) and returns a view for that cell:

```swift
struct GridStack<Content: View>: View {
    let rows: Int
    let columns: Int
    let content: (Int, Int) -> Content

    var body: some View {
        VStack {
            ForEach(0..<rows, id: \.self) { row in
                HStack {
                    ForEach(0..<columns, id: \.self) { column in
                        content(row, column)
                    }
                }
            }
        }
    }
}
```

Use it like this:

```swift
struct ContentView: View {
    var body: some View {
        GridStack(rows: 4, columns: 4) { row, col in
            Text("R\(row) C\(col)")
        }
    }
}
```

`GridStack` is flexible — cells can hold any view, including stacks:

```swift
GridStack(rows: 4, columns: 4) { row, col in
    HStack {
        Image(systemName: "\(row * 4 + col).circle")
        Text("R\(row) C\(col)")
    }
}
```

Add `@ViewBuilder` to `content` for even more flexibility — callers can then put multiple views in a cell without wrapping them in a stack:

```swift
@ViewBuilder let content: (Int, Int) -> Content
```

```swift
// With @ViewBuilder — no HStack needed
GridStack(rows: 4, columns: 4) { row, col in
    Image(systemName: "\(row * 4 + col).circle")
    Text("R\(row) C\(col)")
}
```

---

**Topics:** [Views & Modifiers Intro](https://www.hackingwithswift.com/books/ios-swiftui/views-and-modifiers-introduction) • [Structs for Views](https://www.hackingwithswift.com/books/ios-swiftui/why-does-swiftui-use-structs-for-views) • [Behind the View](https://www.hackingwithswift.com/books/ios-swiftui/what-is-behind-the-main-swiftui-view) • [Modifier Order](https://www.hackingwithswift.com/books/ios-swiftui/why-modifier-order-matters) • [some View](https://www.hackingwithswift.com/books/ios-swiftui/why-does-swiftui-use-some-view-for-its-view-type) • [Conditional Modifiers](https://www.hackingwithswift.com/books/ios-swiftui/conditional-modifiers) • [Environment Modifiers](https://www.hackingwithswift.com/books/ios-swiftui/environment-modifiers) • [Views as Properties](https://www.hackingwithswift.com/books/ios-swiftui/views-as-properties) • [View Composition](https://www.hackingwithswift.com/books/ios-swiftui/view-composition) • [Custom Modifiers](https://www.hackingwithswift.com/books/ios-swiftui/custom-modifiers)