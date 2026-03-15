**March 14, 2026** • [Day 024 - Project 3, part 2](Day%20024%20-%20Project%203,%20part%202.md) ← → [Day 026 - Project 4, part 1](Day%20026%20-%20Project%204,%20part%201.md)

This milestone day reviews the core concepts from Projects 1–3 and challenges you to build a complete Rock, Paper, Scissors brain training game from scratch.

---

## What You Learned

Three projects in, and you've already covered SwiftUI's most foundational concepts — the skills that appear in virtually every app you'll build:

- **Forms and Pickers** — scrolling forms that mix text with controls like `Picker`, which SwiftUI renders as a table-based layout with sliding screens.
- **NavigationStack** — gives views a title and prevents content from appearing under the clock.
- **`@State`** — stores changing data in structs, which can't mutate themselves otherwise.
- **Two-way bindings** — `$variable` syntax lets controls like `TextField` and `Picker` both read and write state.
- **`ForEach`** — creates views in a loop, letting you build many views at once.
- **Stack layouts** — `VStack`, `HStack`, and `ZStack` combined to build grids and complex layouts.
- **Colors and gradients as views** — `Color` and `LinearGradient` conform to `View` and can be sized with frames.
- **Buttons** — created with a label and a closure that fires on tap.
- **Alerts** — defined by a condition, toggled from elsewhere in your code.
- **`some View` and modifier order** — opaque result types are central to how SwiftUI works, and why modifier order matters.
- **Ternary conditional modifiers** — apply different styling depending on program state.
- **View composition and custom modifiers** — break complex UI into small, reusable parts.

An important mental shift: `Color.red`, `LinearGradient`, `VStack`, `Group`, and `ForEach` are all views. Anything that has a `body`property returning `some View` conforms to `View` — this composability is SwiftUI's superpower, built entirely on protocols rather than class inheritance. Unlike UIKit's `UIView` (which forced 200+ inherited properties), a SwiftUI view needs only a single `body` property.

All UI ultimately resolves to **primitive views** — `Text`, `Image`, `Color`, `Spacer`, etc. — which return fixed content rather than other views, breaking the otherwise infinite loop of views rendering views.

---

## Key Points

### Structs vs Classes

The five key differences:

1. Classes have no memberwise initializer; structs get one by default.
2. Classes support inheritance; structs do not.
3. Copying a class gives two references to the same data; copying a struct gives a unique copy.
4. Classes can have deinitializers; structs cannot.
5. Variable properties inside a constant class can be changed; inside a constant struct they cannot.

In Objective-C, classes were used for almost everything. In Swift, the choice is yours — and that choice conveys intent. Using structs by default and switching to a class signals that something behaves differently and should be treated differently. If you always use classes, that signal is lost.

> 💡 SwiftUI completely inverts UIKit conventions: UIKit uses structs for data and classes for UI, while SwiftUI does the opposite — a good reminder to learn things even when they don't seem immediately useful.

---

### Working with ForEach

When looping over a range, SwiftUI can uniquely identify each view by its index:

```swift
ForEach(0 ..< 100) { number in
    Text("Row \(number)")
}
```

When looping over an array, SwiftUI needs to know what makes each item unique so it can update only what changed rather than rebuilding the whole layout. For an array of unique strings, you can use the string value itself as the identifier:

```swift
let agents = ["Cyril", "Lana", "Pam", "Sterling"]

VStack {
    ForEach(agents, id: \.self) {
        Text($0)
    }
}
```

`id: \.self` — tells SwiftUI to use each string's own value as its unique identifier. This works as long as values don't repeat. A third approach using the `Identifiable` protocol comes later in the course.

---

### Working with Bindings

`@State` is convenient boilerplate, but bindings can be created manually using `Binding(get:set:)`. This lets you run logic on read or write — something `didSet` can't do with SwiftUI state.

**Passthrough binding** — reads and writes a single `@State` property, no dollar sign needed since it's already a binding:

```swift
struct ContentView: View {
    @State private var selection = 0

    var body: some View {
        let binding = Binding(
            get: { selection },
            set: { selection = $0 }
        )

        return VStack {
            Picker("Select a number", selection: binding) {
                ForEach(0..<3) {
                    Text("Item \($0)")
                }
            }
            .pickerStyle(.segmented)
        }
    }
}
```

**Computed binding** — derives a single value from multiple state properties, and writes back to all of them at once:

```swift
struct ContentView: View {
    @State private var agreedToTerms = false
    @State private var agreedToPrivacyPolicy = false
    @State private var agreedToEmails = false

    var body: some View {
        let agreedToAll = Binding<Bool>(
            get: {
                agreedToTerms && agreedToPrivacyPolicy && agreedToEmails
            },
            set: {
                agreedToTerms = $0
                agreedToPrivacyPolicy = $0
                agreedToEmails = $0
            }
        )

        return VStack {
            Toggle("Agree to terms", isOn: $agreedToTerms)
            Toggle("Agree to privacy policy", isOn: $agreedToPrivacyPolicy)
            Toggle("Agree to receive shipping emails", isOn: $agreedToEmails)
            Toggle("Agree to all", isOn: agreedToAll)
        }
    }
}
```

`agreedToAll` is `true` only when all three toggles are on. Toggling it sets all three at once. SwiftUI isn't doing magic — everything `@State` does automatically can be done by hand.

---

## Challenge

Build a brain training game that challenges players to win or lose at rock, paper, scissors.

- Each turn, the app randomly picks rock, paper, or scissors.
- Each turn, the app alternates between prompting the player to **win** or **lose**.
- The player taps the correct move to satisfy the prompt.
- Correct answer: +1 point. Wrong answer: −1 point (floored at 0).
- The game ends after 10 questions and shows the final score.

**Tips from the source:**

- Use `Int.random(in:)` for the app's move. Use `Bool.random()` for the win/lose prompt, then `toggle()` between rounds.
- Start with the simplest logic: hardcode each case. Then simplify using a wins array — if moves are `["Rock", "Paper", "Scissors"]`, the winning responses are `["Paper", "Scissors", "Rock"]`.
- Emoji work great for the move buttons and scale with `.font(.system(size: 200))`.
- Use `if shouldWin` to show different prompt text.

---

## My Solution

```swift
import SwiftUI

struct MoveButton: View {
    let emoji: String
    let name: String

    var body: some View {
        VStack {
            Text(emoji)
                .font(.system(size: 64))
            Text(name)
                .font(.headline)
        }
        .frame(width: 100, height: 100)
        .background(Color(UIColor.systemBackground))
        .clipShape(RoundedRectangle(cornerRadius: 16))
        .overlay(
            RoundedRectangle(cornerRadius: 16)
                .stroke(Color(UIColor.separator), lineWidth: 2)
        )
    }
}

struct ContentView: View {

    @State private var currentScore = 0
    @State private var shouldWin = false
    @State private var currentQuestionNumber = 1
    @State private var gameOver = false
    @State private var appChoice = Int.random(in: 0..<3)

    let moves = ["Rock", "Paper", "Scissors"]
    let winningMoves = ["Paper", "Scissors", "Rock"]
    let losingMoves  = ["Scissors", "Rock", "Paper"]
    let numberOfQuestions = 10

    let emojis = [
        "Rock": "🪨",
        "Paper": "📃",
        "Scissors": "✂️"
    ]

    var body: some View {
        VStack {
            VStack {
                HStack {
                    HStack(alignment: .lastTextBaseline) {
                        Text("\(currentScore)")
                            .font(.largeTitle)
                            .fontWeight(.black)
                        Text("pts")
                            .font(.footnote)
                            .foregroundStyle(.secondary)
                    }
                    Spacer()
                    Text("Question \(currentQuestionNumber) of \(numberOfQuestions)")
                        .foregroundStyle(.secondary)
                }
                ProgressView(value: Double(currentQuestionNumber), total: Double(numberOfQuestions))
                    .padding(.bottom, 12)
                Divider()
            }
            .padding(.horizontal)

            Spacer()

            VStack {
                VStack {
                    Text("App Chose")
                        .textCase(.uppercase)
                        .fontWeight(.semibold)
                        .foregroundStyle(.secondary)
                    Text(emojis[moves[appChoice]]!)
                        .font(.system(size: 100))
                    Text(moves[appChoice])
                        .font(.largeTitle)
                        .fontWeight(.bold)
                }
                .frame(maxWidth: .infinity)
                .padding()
                .background(.indigo.opacity(0.2))
                .clipShape(RoundedRectangle(cornerRadius: 16))

                HStack {
                    Text("You need to")
                    Text(shouldWin ? "WIN" : "LOSE")
                }
                .frame(maxWidth: .infinity)
                .foregroundStyle(shouldWin ? .green : .red)
                .font(.title2)
                .bold()
                .padding()
                .background(shouldWin ? Color.green.opacity(0.15) : Color.red.opacity(0.15))
                .clipShape(RoundedRectangle(cornerRadius: 16))
            }
            .padding(.horizontal)

            Spacer()

            VStack {
                Text("Your Move")
                    .textCase(.uppercase)
                HStack(spacing: 12) {
                    Button { playerTapped("Rock") } label: { MoveButton(emoji: "🪨", name: "Rock") }
                    Button { playerTapped("Paper") } label: { MoveButton(emoji: "📃", name: "Paper") }
                    Button { playerTapped("Scissors") } label: { MoveButton(emoji: "✂️", name: "Scissors") }
                }
                .buttonStyle(.plain)
            }

            Spacer()
        }
        .alert("You scored \(currentScore) out of \(numberOfQuestions)", isPresented: $gameOver) {
            Button("Play again", action: resetGame)
        }
    }

    func playerTapped(_ choice: String) {
        let correctMove = shouldWin ? winningMoves[appChoice] : losingMoves[appChoice]

        if choice == correctMove {
            currentScore += 1
        } else {
            if currentScore > 0 {
                currentScore -= 1
            }
        }

        if currentQuestionNumber == numberOfQuestions {
            gameOver = true
        } else {
            currentQuestionNumber += 1
            nextRound()
        }
    }

    func nextRound() {
        appChoice = Int.random(in: 0..<3)
        shouldWin.toggle()
    }

    func resetGame() {
        currentScore = 0
        currentQuestionNumber = 1
        appChoice = Int.random(in: 0..<3)
        shouldWin = false
    }
}
```