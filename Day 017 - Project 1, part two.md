**March 07, 2026** • [Day 016 - Project 1, part one](Day%20016%20-%20Project%201,%20part%20one.md) ← → [Day 018 - Project 1, part three](Day%20018%20-%20Project%201,%20part%20three.md)

Today we turn theory into practice by building the WeSplit app. As Immanuel Kant said: "experience without theory is blind, but theory without experience is mere intellectual play." Everything from yesterday applies here — there are no surprises, just seeing how the pieces fit together.

---

## Putting It All Together

The complete WeSplit app using every concept from today:

```swift
struct ContentView: View {
    @State private var checkAmount = 0.0
    @State private var numberOfPeople = 2
    @State private var tipPercentage = 20
    @FocusState private var amountIsFocused: Bool         // tracks keyboard focus

    let tipPercentages = [10, 15, 20, 25, 0]

    var totalPerPerson: Double {              // computed property — recalculates automatically
        let peopleCount = Double(numberOfPeople + 2) // offset by 2 because ForEach starts at 0
        let tipSelection = Double(tipPercentage)
        let tipValue = checkAmount / 100 * tipSelection
        let grandTotal = checkAmount + tipValue
        return grandTotal / peopleCount
    }

    var body: some View {
        NavigationStack {
            Form {
                Section {
                    TextField("Amount", value: $checkAmount, format: .currency(code: Locale.current.currency?.identifier ?? "USD"))
                        .keyboardType(.decimalPad)        // shows decimal keyboard
                        .focused($amountIsFocused)        // binds focus state to amountIsFocused

                    Picker("Number of people", selection: $numberOfPeople) {
                        ForEach(2..<100) {
                            Text("\($0) people")
                        }
                    }
                }

                Section("How much tip do you want to leave?") {
                    Picker("Tip percentage", selection: $tipPercentage) {
                        ForEach(tipPercentages, id: \.self) {
                            Text($0, format: .percent)
                        }
                    }
                    .pickerStyle(.segmented)              // horizontal segmented control
                }

                Section {
                    Text(totalPerPerson, format: .currency(code: Locale.current.currency?.identifier ?? "USD"))
                }
            }
            .navigationTitle("WeSplit")
            .toolbar {
                if amountIsFocused {
                    Button("Done") {
                        amountIsFocused = false           // dismisses the keyboard
                    }
                }
            }
        }
    }
}
```

---

## Reading Text from the User with TextField

The app needs three `@State` properties for the three pieces of data users will enter, plus one constant for the tip options:

```swift
@State private var checkAmount = 0.0
@State private var numberOfPeople = 2
@State private var tipPercentage = 20

let tipPercentages = [10, 15, 20, 25, 0]
```

Sensible defaults: 0.0 for the check amount, 2 people, and 20% tip.

A basic `TextField` for strings won't work here — we need a number. We can pass a `Double` directly and ask SwiftUI to treat it as a currency. We could start with a hardcoded USD format:

```swift
TextField("Amount", value: $checkAmount, format: .currency(code: "USD"))
```

That's an improvement, but we can do better — hardcoding `"USD"` isn't ideal since over 95% of the world doesn't use US dollars. A better solution is to ask iOS for the current user's currency code:

```swift
TextField("Amount", value: $checkAmount, format: .currency(code: Locale.current.currency?.identifier ?? "USD"))
```

`Locale` is a massive struct built into iOS responsible for storing all the user's region settings — calendar, number separators, metric system, and more. Here we ask for the user's preferred currency code and fall back to `"USD"` if none is set.

When you create text fields in forms, the first parameter is a string used as the **placeholder** — gray text shown inside the text field, giving users an idea of what should be entered. The second parameter is the two-way binding, and the third controls formatting.

To demonstrate `@State` reactivity, we can add a second section showing `checkAmount` live:

```swift
Form {
    Section {
        TextField("Amount", value: $checkAmount, format: .currency(code: Locale.current.currency?.identifier ?? "USD"))
    }

    Section {
        Text(checkAmount, format: .currency(code: Locale.current.currency?.identifier ?? "USD"))
    }
}
```

This synchronization happens because:

- The text field has a two-way binding to `checkAmount`
- `checkAmount` is marked with `@State`, which automatically watches for changes
- When an `@State` property changes, SwiftUI re-invokes `body` and reloads the UI
- Therefore the text view gets the updated value of `checkAmount` automatically

By default a text field shows an alphabetical keyboard, which is annoying for entering numbers. The `.keyboardType()`modifier fixes this:

```swift
TextField("Amount", value: $checkAmount, format: .currency(code: Locale.current.currency?.identifier ?? "USD"))
    .keyboardType(.decimalPad)
```

`.decimalPad` shows digits 0–9 plus a decimal point, perfect for currency amounts. `.numberPad` is similar but without the decimal point.

> 💡 You'll notice `.keyboardType()` is added on a new line, indented one level deeper than `TextField` — that isn't required, but it helps you keep track of which modifiers apply to which views.

> 💡 `.decimalPad` and `.numberPad` don't prevent users from pasting invalid text, but the text field will automatically filter out bad values when they hit Return.

---

## Creating Pickers in a Form

We already have `numberOfPeople` as an `@State` property — now we attach a `Picker` to it, looping over all numbers from 2 through to 99:

```swift
Section {
    TextField("Amount", value: $checkAmount, format: .currency(code: Locale.current.currency?.identifier ?? "USD"))
        .keyboardType(.decimalPad)

    Picker("Number of people", selection: $numberOfPeople) {
        ForEach(2..<100) {
            Text("\($0) people")
        }
    }
}
```

Running this reveals several things: there's a new row showing "Number of people" on the left and "4 people" on the right, there are two gray arrows on the right edge — the iOS way of signaling that tapping the row shows a menu of options — and the row says "4 people" even though `numberOfPeople` defaults to `2`. So it's a bit of "two steps forward, two steps back" — we have a nice result, but it doesn't work and doesn't show the right information.

The reason for "4 people": `ForEach(2..<100)` creates rows starting at index 0, so:

- Index 0 → "2 people"
- Index 1 → "3 people"
- Index 2 → "4 people" ← what `numberOfPeople = 2` points to

This isn't a bug — it's just how the indexing works.

### Picker Styles

To use a navigation link style — which slides in a new screen with all options:

```swift
Picker("Number of people", selection: $numberOfPeople) {
    ForEach(2..<100) {
        Text("\($0) people")
    }
}
.pickerStyle(.navigationLink)
```

That won't quite work as expected though — this time you'll see a gray disclosure indicator on the right edge, but the whole row will be grayed out and won't show anything when tapped. What SwiftUI wants to do is show a new view with the options, and to do that we need a `NavigationStack`. Wrap your `Form` in one:

```swift
var body: some View {
    NavigationStack {
        Form {
            // everything inside your form
        }
    }
}
```

Now tapping the row slides in a new screen with all options. You'll see a checkmark next to the selected value, and tapping a different number slides the screen away with the new selection saved.

This is **declarative UI design** — we say _what_ we want (a navigation link picker with values), not _how_ to build it. SwiftUI handles the list, the checkmarks, and sliding the screen in and out automatically.

> 💡 Do you prefer the menu picker or the navigation link picker? It's your app — you get to choose! The default menu picker is used going forward here, but go with whichever you prefer.

Then add a navigation title to the form:

```swift
.navigationTitle("WeSplit")
```

> 💡 `.navigationTitle()` is attached to the `Form`, not `NavigationStack` — navigation stacks can show many views, so attaching the title to the content inside lets iOS change titles freely.

---

## Adding a Segmented Control for Tip Percentages

Add a third section between the amount/people section and the result section for the tip picker:

```swift
Section {
    Picker("Tip percentage", selection: $tipPercentage) {
        ForEach(tipPercentages, id: \.self) {
            Text($0, format: .percent)
        }
    }
}
```

That loops over all the options in `tipPercentages`, converting each one into a text view with the `.percent` format. SwiftUI will present a pop-up menu of options when the row is tapped.

To use a segmented control instead, add `.pickerStyle(.segmented)`:

```swift
Section {
    Picker("Tip percentage", selection: $tipPercentage) {
        ForEach(tipPercentages, id: \.self) {
            Text($0, format: .percent)
        }
    }
    .pickerStyle(.segmented)
}
```

Running the app now — users can enter a check amount, select the number of people, and pick a tip percentage. But try to look at the UI with fresh eyes:

- "Amount" makes sense — it's a box users can type a number into
- "Number of people" is self-explanatory
- The bottom label is where we'll show the total — ignore for now
- That middle section though — what are those percentages _for_?

The tip percentages aren't labelled. We could add a `Text` view directly before the segmented control:

```swift
Section {
    Text("How much tip do you want to leave?")

    Picker("Tip percentage", selection: $tipPercentage) {
        ForEach(tipPercentages, id: \.self) {
            Text($0, format: .percent)
        }
    }
    .pickerStyle(.segmented)
}
```

That works OK, but it doesn't look great — it looks like an item all by itself rather than a label for the segmented control. A much better idea is to pass the text directly into `Section` as a header:

```swift
Section("How much tip do you want to leave?") {
    Picker("Tip percentage", selection: $tipPercentage) {
        ForEach(tipPercentages, id: \.self) {
            Text($0, format: .percent)
        }
    }
    .pickerStyle(.segmented)
}
```

It's a small change to the code, but the end result looks a lot better — the text now looks like a prompt for the segmented control directly below it.

> 💡 SwiftUI lets us add views to both the **header and footer** of a `Section` — always prefer a section header over a loose label inside the section for better visual grouping.

---

## Calculating the Total Per Person

The easiest and cleanest solution is a computed property called `totalPerPerson`. Add it just before `body`, starting with a scaffold that returns `0` so the code doesn't break:

```swift
var totalPerPerson: Double {
    // calculate the total per person here
    return 0
}
```

Now fill it in. First, get the correct people count — `numberOfPeople` is off by 2 because `ForEach` counts from 0, so we add 2. Both values need to be converted to `Double` to work alongside `checkAmount`:

```swift
let peopleCount = Double(numberOfPeople + 2)
let tipSelection = Double(tipPercentage)
```

Then the math:

- `tipValue` — check amount divided by 100, multiplied by tip percentage
- `grandTotal` — check amount plus tip value
- `amountPerPerson` — grand total divided by number of people

Replace `return 0` with:

```swift
let tipValue = checkAmount / 100 * tipSelection
let grandTotal = checkAmount + tipValue
let amountPerPerson = grandTotal / peopleCount

return amountPerPerson
```

The complete computed property:

```swift
var totalPerPerson: Double {
    let peopleCount = Double(numberOfPeople + 2)
    let tipSelection = Double(tipPercentage)

    let tipValue = checkAmount / 100 * tipSelection
    let grandTotal = checkAmount + tipValue
    let amountPerPerson = grandTotal / peopleCount

    return amountPerPerson
}
```

Now update the final section to show `totalPerPerson` instead of `checkAmount`:

```swift
// Replace this:
Section {
    Text(checkAmount, format: .currency(code: Locale.current.currencyCode ?? "USD"))
}

// With this:
Section {
    Text(totalPerPerson, format: .currency(code: Locale.current.currency?.identifier ?? "USD"))
}
```

Because all the values that make up the total are marked with `@State`, changing any of them will cause the total to be recalculated automatically — this is SwiftUI's "views are a function of their state" in action.

> 🎉 That was the last step in this project — pat yourself on the back, because WeSplit is finished!

---

## Hiding the Keyboard

Once the decimal keyboard appears, it never goes away — there's no Return key on `.decimalPad`. The fix requires two steps:

1. Give SwiftUI a way to determine whether the text field should currently have focus
2. Add a button to remove that focus when the user wants, which dismisses the keyboard

**Step 1** — track focus using `@FocusState`, your second property wrapper. It works exactly like `@State` but is specifically designed to handle input focus:

```swift
@FocusState private var amountIsFocused: Bool
```

Attach it to the text field:

```swift
.focused($amountIsFocused)
```

Now SwiftUI silently tracks whether the text field is focused — `amountIsFocused` is `true` when it is, `false` otherwise.

**Step 2** — add a toolbar button that dismisses the keyboard. Add this modifier to the form, below `.navigationTitle()`:

```swift
.toolbar {
    if amountIsFocused {
        Button("Done") {
            amountIsFocused = false
        }
    }
}
```

`toolbar()` — specifies toolbar items for a view; they can appear in the navigation bar, a bottom toolbar, and more. `if amountIsFocused` — only shows the button when the text field is active. `Button("Done")` — tappable text that runs a closure when pressed; setting `amountIsFocused = false` dismisses the keyboard.

> 💡 You'll meet `toolbar()` and `Button` more in future projects — for now, run the app and try it out. It's a big improvement!

---

**Topics:** [TextField](https://www.hackingwithswift.com/books/ios-swiftui/reading-text-from-the-user-with-textfield) • [Pickers in a Form](https://www.hackingwithswift.com/books/ios-swiftui/creating-pickers-in-a-form) • [Segmented Control](https://www.hackingwithswift.com/books/ios-swiftui/adding-a-segmented-control-for-tip-percentages) • [Total Per Person](https://www.hackingwithswift.com/books/ios-swiftui/calculating-the-total-per-person) • [Hiding the Keyboard](https://www.hackingwithswift.com/books/ios-swiftui/hiding-the-keyboard)