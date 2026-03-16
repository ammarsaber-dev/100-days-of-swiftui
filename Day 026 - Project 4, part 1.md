**March 16, 2026** • [Day 025 - Milestone Projects 1-3](Day%20025%20-%20Milestone%20Projects%201-3.md) ← → [Day 027 - Project 4, part 2](Day%20027%20-%20Project%204,%20part%202.md)

Today we start BetterRest, a Core ML-powered app that predicts the ideal bedtime for coffee drinkers based on three inputs: desired wake time, target sleep hours, and daily coffee intake.

---

## BetterRest: Introduction

BetterRest is a forms-based app that converts user input into an alert — simple in structure, but its real purpose is to introduce **machine learning (ML)**.

All iPhones have **Core ML** built in, which lets us write code that makes predictions based on previously seen data. We give our Mac raw training data, it learns the patterns, and the resulting model runs entirely on-device with complete user privacy.

The app asks three questions:

1. When do they want to wake up?
2. How many hours of sleep do they want?
3. How many cups of coffee do they drink per day?

Those values get fed into Core ML to recommend a bedtime. The combinations are in the billions — wake times × sleep hours × coffee amounts — which is exactly why we need ML. The technique is **regression analysis**: the computer finds an algorithm that represents all the data, then applies it accurately to new data it hasn't seen.

> ⚠️ Download the project files from GitHub: https://github.com/twostraws/HackingWithSwift — SwiftUI section. Then create a new Xcode App project called **BetterRest**.

---

## Entering Numbers with Stepper

SwiftUI has two number input controls. `Stepper` gives − and + buttons for precise selection. `Slider` also picks from a range but less precisely — we'll use that later. `Stepper` works with any number type (`Int`, `Double`, etc.) automatically.

```swift
@State private var sleepAmount = 8.0
```

```swift
Stepper("\(sleepAmount) hours", value: $sleepAmount)
```

Without limits, a `Double` stepper allows an absolutely massive range — including negatives. Use `in` to constrain it (inclusive on both ends, blocks impossible values like -1):

```swift
Stepper("\(sleepAmount) hours", value: $sleepAmount, in: 4...12)
```

`step` controls how much each tap moves the value — must match the binding's type:

```swift
Stepper("\(sleepAmount) hours", value: $sleepAmount, in: 4...12, step: 0.25)
```

The label shows `8.000000` — accurate but too accurate. Fix it with `formatted()`:

```swift
Stepper("\(sleepAmount.formatted()) hours", value: $sleepAmount, in: 4...12, step: 0.25)
```

---

## Selecting Dates and Times with DatePicker

`DatePicker` is SwiftUI's dedicated date input, bound to a `Date` property:

```swift
@State private var wakeUp = Date.now
```

```swift
DatePicker("Please enter a date", selection: $wakeUp)
```

An empty string label still wastes layout space and breaks VoiceOver. Use `.labelsHidden()` — hides it visually, keeps it for screen readers:

```swift
DatePicker("Please enter a date", selection: $wakeUp)
    .labelsHidden()
```

Use `displayedComponents` to control what the picker shows — omit for day/hour/minute, `.date` for month/day/year, `.hourAndMinute` for time only:

```swift
DatePicker("Please enter a time", selection: $wakeUp, displayedComponents: .hourAndMinute)
```

`DatePicker` also accepts an `in` range, built from `Date` values:

```swift
func exampleDates() {
    // create a second Date instance set to one day in seconds from now
    let tomorrow = Date.now.addingTimeInterval(86400)

    // create a range from those two
    let range = Date.now...tomorrow
}
```

Swift supports one-sided ranges too — give it a start and it accepts everything after:

```swift
DatePicker("Please enter a date", selection: $wakeUp, in: Date.now...)
```

---

## Working with Dates

Binding a `Date` to a `DatePicker` is easy — what comes after is trickier than it looks. Daylight saving makes some days 23 or 25 hours. Leap seconds correct for the Earth's slowing rotation. Run `cal 9 1752` in your Mac terminal and you'll see 12 days missing — when the calendar switched from Julian to Gregorian. Dates are unavoidable, but always let Apple's frameworks handle the hard parts — less code, correct for every region.

In BetterRest, dates are used three ways:

**1. Choosing a default wake-up time** — `Date` stores everything: year, month, day, hour, minute, timezone. `DateComponents` lets you set or read only the parts you care about:

```swift
var components = DateComponents()
components.hour = 8
components.minute = 0
let date = Calendar.current.date(from: components) ?? .now
```

`components.hour = 8` — set only the hour, leave everything else unspecified `Calendar.current.date(from: components)` — asks iOS to build a full `Date` from those parts `?? .now` — `date(from:)` returns an optional; fall back to now if it fails

**2. Reading the hour and minute from a picked date** — `DatePicker` gives you a full `Date`, but you only need parts of it:

```swift
let components = Calendar.current.dateComponents([.hour, .minute], from: someDate)
let hour = components.hour ?? 0
let minute = components.minute ?? 0
```

`dateComponents([.hour, .minute], from:)` — extracts only the requested parts `components.hour ?? 0` — even explicitly requested parts come back as optionals; always provide defaults

**3. Formatting dates for display** — chain what you want on a `Text` view:

```swift
Text(Date.now, format: .dateTime.hour().minute())
```

```swift
Text(Date.now, format: .dateTime.day().month().year())
```

Chaining order doesn't control display order — iOS arranges it per the user's locale automatically. Or use `formatted()` for a preset style:

```swift
Text(Date.now.formatted(date: .long, time: .shortened))
```

`date: .long` — full date e.g. "March 16, 2026" `time: .shortened` — hour and minute only e.g. "8:00 AM"

---

## Training a Model with Create ML

**Core ML** lets us use machine learning inside our apps. **Create ML** is a dedicated Mac app for building custom models using drag and drop — no code required. Together they put ML within reach of any iOS developer.

Core ML handles many tasks — images, sounds, motion. For BetterRest we use **tabular regression**: give Create ML spreadsheet-like data and it figures out the relationships between columns.

ML works in two steps. **Training** — the computer studies your data to find patterns; can take hours on large datasets. **Prediction** — the finished model runs on-device, estimating results for data it's never seen.

**Opening Create ML:** Xcode → Open Developer Tool → Create ML → **New Document** → **Tabular Regression** → name it **BetterRest** → save to desktop.

**Loading training data:** The CSV has four columns per entry: wake time, estimated sleep, coffee intake, and actual sleep needed. Under **Data** → **Select…** → choose `BetterRest.csv`.

> ⚠️ This CSV is sample data for this project only — don't use it for real health decisions.

**Target and Features:**

- **Target** → `actualSleep` — what the model learns to predict
- **Features** → `wake`, `estimatedSleep`, `coffee` — the inputs it uses to get there

The distinction matters: if we used sleep estimates and actual sleep as features instead, we could train it to predict coffee intake. Target = what you want predicted; features = everything used to predict it.

**Algorithm:** Five options — Automatic, Random Forest, Boosted Tree, Decision Tree, Linear Regression. **Automatic** picks the best one for your data; it limits some advanced options but is more than good enough here.

> 💡 For an overview of each algorithm, Paul has a talk called _Create ML for Everyone_ on YouTube: https://youtu.be/a905KIBw1hs

Click **Train**. Our dataset is small so it finishes in seconds — a big checkmark confirms success.

**Evaluating:** Evaluation tab → Validation → look at **Root Mean Squared Error (RMSE)**. You should see ~170, meaning predictions are off by about 170 seconds (3 minutes) on average.

> 💡 Create ML splits your data automatically — part for training, part held back for validation. It predicts on the validation set and checks how far off it was from the real values.

**Exporting:** Output tab shows the model is ~545 bytes. Create ML took 180KB of raw data and condensed it to almost nothing. Almost all of those bytes are metadata (field names, author). The actual prediction logic is under 100 bytes — because Create ML doesn't store raw values, only the relationship weights it found after running billions of CPU cycles testing combinations. Click **Get** to export to your desktop.

> 💡 To experiment with algorithms, right-click the model source in the left panel → **Duplicate** → retrain.

---

**Topics:** [BetterRest: Introduction](https://www.hackingwithswift.com/books/ios-swiftui/betterrest-introduction) • [Entering numbers with Stepper](https://www.hackingwithswift.com/books/ios-swiftui/entering-numbers-with-stepper) • [Selecting dates and times with DatePicker](https://www.hackingwithswift.com/books/ios-swiftui/selecting-dates-and-times-with-datepicker) • [Working with dates](https://www.hackingwithswift.com/books/ios-swiftui/working-with-dates) • [Training a model with Create ML](https://www.hackingwithswift.com/books/ios-swiftui/training-a-model-with-create-ml)