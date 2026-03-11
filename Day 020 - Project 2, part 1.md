**March 11, 2026** • [Day 019 – Challenge Day](Day%20019%20-%20Challenge%20day.md) ← → [Day 021 - Project 2, part 2](Day%20021%20-%20Project%202,%20part%202.md)

We're starting **Project 2: Guess the Flag** — a guessing game that teaches players flags of the world. Introduces stacks, colors, gradients, buttons, images, and alerts.

Download assets from GitHub: [https://github.com/twostraws/HackingWithSwift](https://github.com/twostraws/HackingWithSwift) (SwiftUI section). Create a new Xcode project called **GuessTheFlag** using the same settings as before.

---

## Stacks

Three stack types for arranging multiple views:

|Stack|Direction|
|---|---|
|`VStack`|Vertical|
|`HStack`|Horizontal|
|`ZStack`|Depth (back to front)|

SwiftUI can infer a `VStack` when you return multiple views loosely, but being explicit lets you control **spacing** and **alignment**:

```swift
VStack(spacing: 20) { ... }
VStack(alignment: .leading) { ... }
HStack(spacing: 20) { ... }
```

`ZStack` has no spacing (views overlap), but supports alignment. Views are drawn in code order — last view is on top:

```swift
ZStack(alignment: .top) { ... }
```

**`Spacer`** fills all remaining space automatically. Multiple spacers divide the available space between them:

```swift
VStack {
    Text("First")
    Text("Second")
    Text("Third")
    Spacer()          // pushes all content to the top
}

VStack {
    Spacer()          // 1/3 of space
    Text("First")
    Text("Second")
    Text("Third")
    Spacer()          // \
    Spacer()          //  2/3 of space
}
```

> 💡 Stacks can be nested — try placing several `HStack`s inside a `VStack` to make a 3×3 grid.

---

## Colors and Frames

`Color` is a full view that fills all available space by default:

```swift
ZStack {
    Color.red           // fills the ZStack
    Text("Your content")
}
```

> 💡 As a standalone view you must write `Color.red`. As a modifier argument (`.background(.red)`) SwiftUI infers the type.

`.background()` only colors the area behind its own view — even if attached to a container, it won't fill it. To color a whole container, place `Color` inside it as a view:

Constrain a color with `.frame()`:

```swift
Color.red.frame(width: 200, height: 200)
Color.red.frame(minWidth: 200, maxWidth: .infinity, maxHeight: 200)
```

**Semantic colors** adapt to light/dark mode:

- `Color.primary` — default text color (black/white)
- `Color.secondary` — same but slightly transparent

Custom color:

```swift
Color(red: 1, green: 0.8, blue: 0)
```

**Safe area** — iOS reserves space around the Dynamic Island and home indicator. Use `.ignoresSafeArea()` to fill edge to edge (decorative content only):

```swift
ZStack { Color.red; Text("Content") }
.ignoresSafeArea()
```

**Materials** — frosted glass effect over background content:

```swift
Text("Your content")
    .foregroundStyle(.secondary)
    .padding(50)
    .background(.ultraThinMaterial)
```

Available thicknesses: `.ultraThinMaterial` → `.thinMaterial` → `.regularMaterial` → `.thickMaterial` → `.ultraThickMaterial`. Text over a material gets **vibrancy** — background colors subtly bleed through to keep text legible.

---

## Gradients

### LinearGradient

```swift
LinearGradient(colors: [.white, .black], startPoint: .top, endPoint: .bottom)
```

Use **stops** for precise control over where each color sits (0.0–1.0). A gap between stops creates a sharp transition:

```swift
LinearGradient(stops: [
    .init(color: .white, location: 0.45),
    .init(color: .black, location: 0.55),
], startPoint: .top, endPoint: .bottom)
```

### RadialGradient

Radiates outward in a circle:

```swift
RadialGradient(colors: [.blue, .black], center: .center, startRadius: 20, endRadius: 200)
```

### AngularGradient

Also called conic — cycles colors around a circle:

```swift
AngularGradient(colors: [.red, .yellow, .green, .blue, .purple, .red], center: .center)
```

All three can use stops instead of plain colors, and work as standalone views or as `.background()` arguments.

### Simple `.gradient` modifier

Appending `.gradient` to any color creates a subtle automatic linear gradient — no configuration needed:

```swift
Text("Your content")
    .frame(maxWidth: .infinity, maxHeight: .infinity)
    .foregroundStyle(.white)
    .background(.red.gradient)
```

---

## Buttons and Images

Basic button with title + closure (or named function):

```swift
Button("Delete selection") { print("Now deleting…") }
Button("Delete selection", action: executeDelete)
```

**Roles** adjust appearance and accessibility:

```swift
Button("Delete", role: .destructive, action: executeDelete)
```

**Built-in styles** — `.bordered` and `.borderedProminent`, combinable with roles:

```swift
Button("Button 1") { }.buttonStyle(.bordered)
Button("Button 2", role: .destructive) { }.buttonStyle(.bordered)
Button("Button 3") { }.buttonStyle(.borderedProminent)
Button("Button 4", role: .destructive) { }.buttonStyle(.borderedProminent)
```

Customize color with `.tint()`:

```swift
Button("Go") { }.buttonStyle(.borderedProminent).tint(.mint)
```

> ⚠️ Apple recommends against overusing prominent buttons — when everything is prominent, nothing is.

**Custom label** for full control:

```swift
Button { } label: {
    Text("Tap me!").padding().foregroundStyle(.white).background(.red)
}
```

### Images

```swift
Image("pencil")              // from asset catalog; read by screen reader
Image(decorative: "pencil")  // same, but screen reader skips it
Image(systemName: "pencil")  // SF Symbols built-in icon
```

Use `Image(decorative:)` when the image adds no extra information beyond what's already on screen.

### Buttons with Images

Image-only button:

```swift
Button { } label: { Image(systemName: "pencil") }
```

Text + image — two options:

```swift
// Simple
Button("Edit", systemImage: "pencil") { }

// Custom styling with Label
Button { } label: {
    Label("Edit", systemImage: "pencil").padding().foregroundStyle(.white).background(.red)
}
```

`Label` is smarter than a manual `HStack` — SwiftUI automatically shows the icon, text, or both depending on context.

---

## Alerts

Alerts are state-driven — you don't call `alert.show()`, you declare state and SwiftUI shows/hides the alert automatically:

```swift
@State private var showingAlert = false

Button("Show Alert") { showingAlert = true }
.alert("Important message", isPresented: $showingAlert) {
    Button("OK") { }    // empty closure is fine — any button auto-dismisses the alert
}
```

- `isPresented: $showingAlert` is a two-way binding — SwiftUI sets it back to `false` on dismiss
- `.alert()` can be attached anywhere in the view hierarchy — placement doesn't matter
- The button closure runs extra logic beyond dismissal; if you have none, leave it empty

**Multiple buttons with roles:**

```swift
.alert("Important message", isPresented: $showingAlert) {
    Button("Delete", role: .destructive) { }
    Button("Cancel", role: .cancel) { }
}
```

**Add a message** with a second trailing closure:

```swift
.alert("Important message", isPresented: $showingAlert) {
    Button("OK", role: .cancel) { }
} message: {
    Text("Please read this.")
}
```

---

**Topics:** [Guess the Flag Intro](https://www.hackingwithswift.com/books/ios-swiftui/guess-the-flag-introduction) • [Stacks](https://www.hackingwithswift.com/books/ios-swiftui/using-stacks-to-arrange-views) • [Colors & Frames](https://www.hackingwithswift.com/books/ios-swiftui/colors-and-frames) • [Gradients](https://www.hackingwithswift.com/books/ios-swiftui/gradients) • [Buttons & Images](https://www.hackingwithswift.com/books/ios-swiftui/buttons-and-images) • [Alerts](https://www.hackingwithswift.com/books/ios-swiftui/showing-alert-messages)