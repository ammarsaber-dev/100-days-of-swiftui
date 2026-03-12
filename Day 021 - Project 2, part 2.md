**March 11, 2026** • [Day 020 - Project 2, part 1](Day%20020%20-%20Project%202,%20part%201.md) ← → [Day 022 - Project 2, part 3](Day%20022%20-%20Project%202,%20part%203.md)

Today we turn the overview from Day 20 into a real app. There are no new concepts — just seeing how all the pieces fit together to build **Guess the Flag**.

---

## Stacking Up Buttons

The app needs two properties to store its game data — an array of countries and an integer for the correct answer. Both need `@State` because `askQuestion()` will modify them later:

```swift
@State private var countries = ["Estonia", "France", "Germany", "Ireland", "Italy", "Nigeria", "Poland", "Spain", "UK", "Ukraine", "US"].shuffled()

@State private var correctAnswer = Int.random(in: 0...2)
```

`.shuffled()` randomizes the array order automatically. `Int.random(in: 0...2)` picks a random number from 0 to 2 to decide which flag is correct.

The layout uses a `ZStack` for the background, with two nested `VStack`s inside — the outer one with 30pt spacing between the question block and flags, and the inner one with no custom spacing to keep the two text views tight:

```swift
var body: some View {
    ZStack {
        Color.blue
            .ignoresSafeArea()

        VStack(spacing: 30) {
            VStack {
                Text("Tap the flag of")
                    .foregroundStyle(.white)
                Text(countries[correctAnswer])
                    .foregroundStyle(.white)
            }

            ForEach(0..<3) { number in
                Button {
                    // flag was tapped
                } label: {
                    Image(countries[number])
                }
            }
        }
    }
}
```

Having two `VStack`s gives independent spacing control — the outer one spaces the sections apart, the inner one keeps the prompt text tight together.

---

## Showing the Player's Score with an Alert

To show an alert when a flag is tapped, we need two new `@State` properties — one to control whether the alert is showing, and one to store its title:

```swift
@State private var showingScore = false
@State private var scoreTitle = ""
```

The `flagTapped()` method checks whether the tapped flag matches the correct answer, sets the title accordingly, then triggers the alert:

```swift
func flagTapped(_ number: Int) {
    if number == correctAnswer {
        scoreTitle = "Correct"
    } else {
        scoreTitle = "Wrong"
    }
    showingScore = true
}
```

Replace the `// flag was tapped` comment with `flagTapped(number)` — `number` is already provided by `ForEach`.

When the alert is dismissed, the game needs to continue. `askQuestion()` reshuffles the countries and picks a new correct answer:

```swift
func askQuestion() {
    countries.shuffle()
    correctAnswer = Int.random(in: 0...2)
}
```

Attach the alert to the `ZStack`:

```swift
.alert(scoreTitle, isPresented: $showingScore) {
    Button("Continue", action: askQuestion)
} message: {
    Text("Your score is ???")
}
```

---

## Styling Our Flags

**Background** — replace the solid blue with a linear gradient for better contrast against flags that contain blue:

```swift
LinearGradient(colors: [.blue, .black], startPoint: .top, endPoint: .bottom)
    .ignoresSafeArea()
```

**Typography** — make the country name the most prominent text using `font()` and `weight()`:

```swift
Text("Tap the flag of")
    .font(.subheadline.weight(.heavy))

Text(countries[correctAnswer])
    .font(.largeTitle.weight(.semibold))
```

`largeTitle` is the largest built-in iOS font size and automatically scales with the user's Dynamic Type setting.

**Flag images** — clip each flag to a capsule shape and add a shadow:

```swift
Image(countries[number])
    .clipShape(.capsule)
    .shadow(radius: 5)
```

Capsule fully rounds the shortest edges while keeping the longest edges straight — great for flag buttons. `shadow(radius:)`defaults to a translucent black with no X/Y offset.

> 💡 Four built-in clip shapes: `rectangle`, `roundedRectangle`, `circle`, and `capsule`.

---

## Upgrading Our Design

**Background** — replace the linear gradient with a radial gradient using two stops at the same location, which eliminates the fade and creates a hard color switch — a blue circle over a red background. Custom color values give muted, flag-like tones rather than harsh system colors:

```swift
RadialGradient(stops: [
    .init(color: Color(red: 0.1, green: 0.2, blue: 0.45), location: 0.3),
    .init(color: Color(red: 0.76, green: 0.15, blue: 0.26), location: 0.3),
], center: .top, startRadius: 200, endRadius: 400)
    .ignoresSafeArea()
```

**Flag card** — reduce inner `VStack` spacing to 15 and wrap it in a styled rounded card:

```swift
VStack(spacing: 15) { ... }
    .frame(maxWidth: .infinity)
    .padding(.vertical, 20)
    .background(.regularMaterial)
    .clipShape(.rect(cornerRadius: 20))
```

**Outer layout** — wrap everything in a new `VStack` with a title above and a score label below:

```swift
VStack {
    Text("Guess the Flag")
        .font(.largeTitle.bold())  // shortcut for .font(.largeTitle.weight(.bold))
        .foregroundStyle(.white)

    // VStack(spacing: 15) card here

    Text("Score: ???")
        .foregroundStyle(.white)
        .font(.title.bold())
}
.padding()
```

**Text colors inside the card** — remove `.foregroundStyle(.white)` from `Text(countries[correctAnswer])` so it uses the system primary color (black in light mode, white in dark mode). Change "Tap the flag of" to `.foregroundStyle(.secondary)` for the iOS vibrancy effect.

**Spacers** — add spacers so the layout scales across all device sizes. On large devices they spread the UI out; on small devices they shrink to almost nothing:

```swift
VStack {
    Spacer()                     // before title
    Text("Guess the Flag")...

    VStack(spacing: 15) { ... }  // the card

    Spacer()                     // \
    Spacer()                     //  twice the space before score
    Text("Score: ???")...
    Spacer()                     // after score
}
.padding()
```

> 💡 Always test your layout on multiple device sizes — from the small iPhone SE up to a Pro Max.

---

**Topics:** [Stacking Up Buttons](https://www.hackingwithswift.com/books/ios-swiftui/stacking-up-buttons) • [Score Alert](https://www.hackingwithswift.com/books/ios-swiftui/showing-the-players-score-with-an-alert) • [Styling Flags](https://www.hackingwithswift.com/books/ios-swiftui/styling-our-flags) • [Upgrading the Design](https://www.hackingwithswift.com/books/ios-swiftui/upgrading-our-design)