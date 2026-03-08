**March 08, 2026** • [Day 017 - Project 1, part two](Day%20017%20-%20Project%201%2C%20part%20two.md) ← → [Day 019 - Challenge day](Day%20019%20-%20Challenge%20day.md)

You've reached the end of your first SwiftUI app — good job! We've covered a lot of ground, but the pace was intentionally slow to make sure it all sinks in. There's lots more to cover in future projects, so taking a little extra time now is okay.

---

## What You Learned

In this project you learned about:

- The basic structure of SwiftUI apps
- How to build forms and sections
- Creating navigation stacks and navigation bar titles
- How to store program state with the `@State` and `@FocusState` property wrappers
- How to create user interface controls like `TextField` and `Picker`
- How to create views in a loop using `ForEach`

---

## Challenges

One of the best ways to learn is to write your own code as often as possible. Here are three ways to extend WeSplit to make sure you fully understand what's going on:

1. Add a header to the third section, saying "Amount per person"
2. Add another section showing the total amount for the check — the original amount plus tip value, without dividing by the number of people
3. Change the tip percentage picker to show a new screen rather than using a segmented control, and give it a wider range of options — everything from 0% to 100%

> 💡 For challenge 3, use the range `0..<101` rather than a fixed array.

> ⚠️ No solutions are provided for these challenges — they are there to test you, not to follow along with.

---

## My Solution

![](Screenshot%202026-03-08%20at%209.49.45%20PM.png)

```swift
import SwiftUI

struct ContentView: View {
    @State private var checkAmount = 0.0
    @State private var numberOfPeople = 2
    @State private var tipPercentage = 20
    
    @FocusState private var amountIsFocused: Bool
        
    var totalPerPerson: Double {
        let peopleCount = Double(numberOfPeople + 2)
        let tipSelection = Double(tipPercentage)
        
        let tipValue = checkAmount / 100 * tipSelection
        let grandTotal = checkAmount + tipValue
        let amountPerPerson = grandTotal / peopleCount
        
        return amountPerPerson
    }
    
    var totalAmount: Double {
        let tipSelection = Double(tipPercentage)
        
        let tipValue = checkAmount / 100 * tipSelection
        let grandTotal = checkAmount + tipValue

        return grandTotal
    }
    
    var body: some View {
        NavigationStack {
            Form {
                Section {
                    TextField("Amount", value: $checkAmount, format: .currency(code: Locale.current.currency?.identifier ?? "USD"))
                        .keyboardType(.decimalPad)
                        .focused($amountIsFocused)
                    
                    Picker("Number of people", selection: $numberOfPeople) {
                        ForEach(2..<100) {
                            Text("\($0) people")
                        }
                    }
                    .pickerStyle(.navigationLink)
                }
                
                Section("How much tip do you want to leave?") {
                    Picker("Tip percentage", selection: $tipPercentage) {
                        ForEach(0..<101) {
                            Text($0, format: .percent)
                        }
                    }
                    .pickerStyle(.navigationLink)
                }
                
                Section("Amount per person") {
                    Text(totalPerPerson, format: .currency(code: Locale.current.currency?.identifier ?? "USD"))
                }
                
                Section("Total Amount") {
                    Text(totalAmount, format: .currency(code: Locale.current.currency?.identifier ?? "USD"))
                }
            }
            .navigationTitle("WeSplit")
            .toolbar {
                if amountIsFocused {
                    Button("Done") {
                        amountIsFocused = false
                    }
                }
            }
        }
    }
}
```

---

**Review:** [Click here to review what you learned in this project](https://www.hackingwithswift.com/review/ios-swiftui/wesplit)