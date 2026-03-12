**March 11, 2026** • [Day 021 - Project 2, part 2](Day%20021%20-%20Project%202,%20part%202.md) ← → [Day 023 - Project 3, part 1](Day%20023%20-%20Project%203,%20part%201.md)

You've reached the end of your second SwiftUI project — good job! Time to review what you learned and complete three challenges to make sure it all sticks.

---

## What You Learned

In this project you learned about:

- Building layouts with `VStack`, `HStack`, and `ZStack` — you'll use these in almost every project, combining them to create complex UIs
- Colors and gradients as standalone views
- Buttons with custom labels and images
- Showing alerts — creating them, binding them to state, and dismissing them. Views must always reflect program state, so you can't just show alerts on demand

---

## Challenges

1. Add an `@State` property to store the user's score, modify it when they get an answer right or wrong, then display it in the alert and in the score label.
2. When someone chooses the wrong flag, tell them their mistake in the alert message — e.g. _"Wrong! That's the flag of France."_
3. Make the game show only 8 questions, after which a final alert judges their score and lets them restart the game.

> 💡 For challenge 3: add a second `alert()` modifier watching a different `Bool` property, then connect its button to a `reset()` method that sets the game back to its initial state.

> ⚠️ No solutions are provided — these are there to test you.

---

## My Solution

```swift
import SwiftUI

struct ContentView: View {
    @State private var countries = ["Estonia", "France", "Germany", "Ireland", "Italy", "Monaco", "Nigeria", "Poland", "Spain", "UK", "Ukraine", "US"].shuffled()
    @State private var correctAnswer = Int.random(in: 0...2)

    @State private var showingScore = false
    @State private var scoreTitle = ""
    @State private var scoreCount = 0
    @State private var questionsCount = 8
    @State private var resetTheGame = false

    var body: some View {
        ZStack {
            RadialGradient(stops: [
                .init(color: Color(red: 0.5, green: 0, blue: 1), location: 0.4),
                .init(color: Color(red: 0.1, green: 0.1, blue: 0.1), location: 0.3)
            ], center: .top, startRadius: 200, endRadius: 400)
                .ignoresSafeArea()

            VStack {
                VStack(spacing: 30) {
                    Spacer()
                    VStack {
                        Text("Tap the flag of")
                            .foregroundStyle(.thinMaterial)
                            .font(.subheadline)
                            .fontWeight(.heavy)
                        Text(countries[correctAnswer])
                            .foregroundStyle(.thickMaterial)
                            .font(.largeTitle)
                            .fontWeight(.black)
                    }

                    ForEach(0..<3) { number in
                        Button {
                            flagTapped(number)
                        } label: {
                            Image(countries[number])
                                .clipShape(.capsule)
                                .shadow(radius: 5)
                        }
                    }
                    Spacer()
                    Spacer()
                }

                Text("Score: \(scoreCount)")
                    .font(.title)
                    .fontWeight(.bold)
                    .foregroundStyle(.regularMaterial)

                Spacer()
            }
        }
        .alert(scoreTitle, isPresented: $showingScore) {
            Button("Continue", action: askQuestion)
        }
        .alert("You got \(scoreCount) out of 8", isPresented: $resetTheGame) {
            Button("Do you want to play again?", action: playAgain)
        }
    }

    func flagTapped(_ number: Int) {
        if number == correctAnswer {
            scoreTitle = "Correct"
            scoreCount += 1
        } else {
            scoreTitle = "Wrong! that's the flag of \(countries[number])"
            if scoreCount > 0 {
                scoreCount -= 1
            }
        }
        showingScore = true
        questionsCount -= 1
        if questionsCount == 0 {
            resetTheGame = true
        }
    }

    func askQuestion() {
        countries.shuffle()
        correctAnswer = Int.random(in: 0...2)
    }

    func playAgain() {
        scoreCount = 0
        questionsCount = 8
        askQuestion()
    }
}

#Preview {
    ContentView()
}
```

---

**Review:** [Click here to review what you learned in this project](https://www.hackingwithswift.com/review/ios-swiftui/guess-the-flag)