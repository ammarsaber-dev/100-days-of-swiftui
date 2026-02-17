**February 15, 2026** • [Day 4 - Complex data types, part 2](Day%204%20-%20Complex%20data%20types,%20part%202.md) ← → [Day 6 - Loops](Day%206%20-%20Loops.md)

Learn how to make decisions in code: `if`, `else`, `switch`, and the ternary operator.

---

## Quick Reference

```swift
// Basic if/else
if score > 80 {
    print("Pass")
} else {
    print("Fail")
}

// Multiple conditions
if score >= 90 {
    print("A")
} else if score >= 80 {
    print("B")
} else {
    print("C")
}

// AND (both must be true)
if age >= 18 && hasID {
    print("Can enter")
}

// OR (either can be true)
if isAdmin || isOwner {
    print("Can delete")
}

// Switch
switch weather {
case "rain":
    print("Umbrella")
case "snow":
    print("Coat")
default:
    print("Enjoy")
}

// Ternary: condition ? true : false
let result = score > 80 ? "Pass" : "Fail"
```

---

## Comparison Operators

All return `Bool` (true or false):

- `>` greater than
- `<` less than
- `>=` greater than or equal
- `<=` less than or equal
- `==` equal to (not `=`, which is assignment)
- `!=` not equal to

Works with numbers, strings (alphabetical), and many other types. Swift can compare many types out of the box: numbers, strings, dates, and even enums.

```swift
let speed = 88
if speed >= 88 {
    print("Where we're going we don't need roads")
}

let ourName = "Dave Lister"
let friendName = "Arnold Rimmer"
if ourName < friendName {
    print("It's \(ourName) vs \(friendName)")  // D comes before A
}

let name = "Taylor Swift"
if name != "Anonymous" {
    print("Welcome, \(name)")
}
```

### Checking Empty Strings

```swift
// ✅ Best: Fast and clear
if username.isEmpty {
    username = "Anonymous"
}

// ⚠️ Works but slow: Swift must count all characters
if username.count == 0 {
    username = "Anonymous"
}

// ✅ Works and clear
if username == "" {
    username = "Anonymous"
}
```

Use `.isEmpty` instead of `count == 0` for strings—it's much faster because Swift doesn't need to count every character. This is especially important for long strings or complex Unicode characters.

### Working with Arrays

```swift
var numbers = [1, 2, 3]
numbers.append(4)

if numbers.count > 3 {
    numbers.remove(at: 0)
}
print(numbers)  // [2, 3, 4]
```

### Common Mistakes

```swift
// ❌ Using = instead of ==
if score = 80 { }  // Error: = is assignment, not comparison

// ✅ Use comparison operator
if score == 80 { }

// ❌ Type mismatch
let age = 18
if age == "18" { }  // Error: can't compare Int to String

// ✅ Types must match
if age == 18 { }
```

---

## if/else Chains

```swift
let age = 18

if age >= 18 {
    print("You can vote")
} else {
    print("Too young to vote")
}
```

Using `else` is more efficient than two separate `if` statements—Swift only checks the condition once, and the logic is clearer since the paths are mutually exclusive.

```swift
// ❌ Less efficient: checks condition twice
if age >= 18 { print("Adult") }
if age < 18 { print("Minor") }

// ✅ Better: checks once
if age >= 18 {
    print("Adult")
} else {
    print("Minor")
}
```

### Multiple Conditions with else if

```swift
let score = 85

if score >= 90 {
    print("A grade")
} else if score >= 80 {
    print("B grade")
} else if score >= 70 {
    print("C grade")
} else {
    print("Below C")
}
```

You can have multiple `else if` blocks, but only one `if` and one `else`.

### Combining Conditions

```swift
// && (AND): both must be true
let temp = 25
if temp > 20 && temp < 30 {
    print("Nice day")
}

// || (OR): either can be true
let userAge = 14
let hasParentalConsent = true
if userAge >= 18 || hasParentalConsent {
    print("Can buy the game")
}

// Mixing && and ||: use parentheses for clarity
if (isOwner && isEditingEnabled) || isAdmin {
    print("Can delete")
}
```

Always use parentheses when mixing `&&` and `||` to avoid ambiguity. Swift evaluates `&&` before `||`, but explicit parentheses make your intent clear.

### Working with Enums

```swift
enum TransportOption {
    case airplane, helicopter, bicycle, car, scooter
}

let transport = TransportOption.airplane

if transport == .airplane || transport == .helicopter {
    print("Let's fly!")
} else if transport == .bicycle {
    print("Hope there's a bike path")
} else if transport == .car {
    print("Time for traffic")
} else {
    print("I'm hiring a scooter")
}
```

With enums, the first assignment needs the full name (`TransportOption.airplane`), then you can use shorthand (`.airplane`).

---

## Switch Statements

Switch is better than long `if/else` chains when checking one value against multiple possibilities. It's more readable, more performant (reads the value once), and Swift ensures you handle all cases.

```swift
enum Weather {
    case sun, rain, wind, snow, unknown
}

let forecast = Weather.sun

switch forecast {
case .sun:
    print("Nice day")
case .rain:
    print("Bring umbrella")
case .wind:
    print("Wear layers")
case .snow:
    print("Stay inside")
case .unknown:
    print("Forecast broken")
}
```

### Must Be Exhaustive

Switch must handle every possible case, or use `default` for everything else:

```swift
// ❌ Error: missing cases
switch forecast {
case .sun:
    print("Nice")
case .rain:
    print("Wet")
// Error: Switch must be exhaustive
}

// ✅ Fix: handle all cases or use default
switch forecast {
case .sun:
    print("Nice")
case .rain:
    print("Wet")
default:
    print("Check forecast")
}
```

### Using default for Unlimited Possibilities

```swift
let place = "Metropolis"

switch place {
case "Gotham":
    print("You're Batman!")
case "Mega-City One":
    print("You're Judge Dredd!")
case "Wakanda":
    print("You're Black Panther!")
default:
    print("Who are you?")
}
```

`default` must come last and catches everything not explicitly handled.

### Using fallthrough (Rare)

```swift
let day = 5
print("My true love gave to me…")

switch day {
case 5:
    print("5 golden rings")
    fallthrough
case 4:
    print("4 calling birds")
    fallthrough
case 3:
    print("3 French hens")
    fallthrough
case 2:
    print("2 turtle doves")
    fallthrough
default:
    print("A partridge in a pear tree")
}
```

`fallthrough` makes Swift continue to the next case. Very rarely used in practice—switch normally only executes the first matching case.

**When to use switch:** Checking the same value 3+ times, working with enums (Swift verifies exhaustiveness), or when you need better readability than long `if/else if` chains.

---

## Ternary Operator

Format: `condition ? valueIfTrue : valueIfFalse`

Also called "conditional operator" or "ternary conditional operator" (works with three values). Remember **WTF**: **W**hat condition, **T**rue value, **F**alse value.

```swift
let age = 18
let canVote = age >= 18 ? "Yes" : "No"

// With print
let hour = 23
print(hour < 12 ? "Before noon" : "After noon")

// With arrays
let names = ["Jayne", "Kaylee", "Mal"]
let crewCount = names.isEmpty ? "No one" : "\(names.count) people"

// With enums
enum Theme {
    case light, dark
}
let theme = Theme.dark
let background = theme == .dark ? "black" : "white"
```

### Why It Exists

Sometimes you must use it (especially in SwiftUI). This is invalid:

```swift
// ❌ Invalid: can't use if/else inside function arguments
print(
    if hour < 12 {
        "Morning"
    } else {
        "Afternoon"
    }
)

// ✅ Must use ternary
print(hour < 12 ? "Morning" : "Afternoon")
```

Ternary is required in SwiftUI and other declarative contexts where you need a value, not just an action.

### Common Mistakes

```swift
// ❌ Missing false value
let result = score > 80 ? "Pass"  // Error

// ✅ Must provide both values
let result = score > 80 ? "Pass" : "Fail"

// ❌ Type mismatch
let status = isActive ? "Active" : 0  // Error

// ✅ Both values must be same type
let status = isActive ? "Active" : "Inactive"
```

**When to use:** Simple single-line conditions, when you need a value (not just an action), or when required in declarative contexts. **When not to use:** Complex conditions (use regular if/else for readability) or multi-line ternaries.

---

**Topics:** [Conditions](https://www.hackingwithswift.com/quick-start/beginners/how-to-check-a-condition-is-true-or-false) • [Multiple Conditions](https://www.hackingwithswift.com/quick-start/beginners/how-to-check-multiple-conditions) • [Switch](https://www.hackingwithswift.com/quick-start/beginners/how-to-use-switch-statements-to-check-multiple-conditions) • [Ternary](https://www.hackingwithswift.com/quick-start/beginners/how-to-use-the-ternary-conditional-operator-for-quick-tests)