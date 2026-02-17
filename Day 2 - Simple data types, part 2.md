**February 15, 2026** • [Day 1 - First steps in Swift](Day%201%20-%20First%20steps%20in%20Swift.md) ← → [Day 3 - Complex data types, part 1](Day%203%20-%20Complex%20data%20types,%20part%201.md)

Learn about Booleans (true/false values) and string interpolation (building strings with dynamic data). Plus Checkpoint 1 where you'll write real code from scratch.

---

## Quick Reference

```swift
// Booleans
let goodDogs = true
let gameOver = false
isAuthenticated = !isAuthenticated  // flip with !
gameOver.toggle()                   // flip with toggle()

// String interpolation
let message = "Hello, \(name), you're \(age) years old."
let math = "5 x 5 is \(5 * 5)"
```

---

## Booleans

A **Boolean** stores either `true` or `false`. Named after mathematician George Boole, who studied logic.

### Why Booleans Matter

You've already seen Booleans in action:

```swift
let filename = "paris.jpg"
print(filename.hasSuffix(".jpg"))  // returns true or false

let number = 120
print(number.isMultiple(of: 3))    // returns true or false
```

Both checks return either `true` or `false`, which is exactly what Booleans store. In real apps, you'll use Booleans constantly: Is the user logged in? Has payment succeeded? Should this button be enabled? All Boolean questions.

### Creating Booleans

```swift
let goodDogs = true
let gameOver = false

// Or assign from expressions
let isMultiple = 120.isMultiple(of: 3)
```

### Flipping Boolean Values

Booleans don't support arithmetic like `+` or `-`, but they have two ways to flip values:

**Using `!` operator (creates new value):**

```swift
var isAuthenticated = false
isAuthenticated = !isAuthenticated  // false → true
```

This switches `false` to `true`, and vice versa.

**Using `.toggle()` method (modifies in place):**

```swift
var gameOver = false
gameOver.toggle()  // flips from false to true
```

This does the same as `!`, and is especially helpful in complex code where you just want to flip a state without caring what it currently is.

### Common Mistakes

```swift
// ❌ Trying to toggle a constant
let gameOver = false
gameOver.toggle()  // Error: cannot mutate constant

// ✅ Use var for values that change
var gameOver = false
gameOver.toggle()

// Remember: ! creates a new value, doesn't modify the original
let isEnabled = true
let isDisabled = !isEnabled  // isDisabled is false, isEnabled still true
```

**Key Insights:**

- Booleans store `true` or `false`
- Use `!` to create a flipped copy (doesn't change original)
- Use `.toggle()` to flip in place (must be `var`)
- Used constantly in apps for conditions, flags, and state tracking
- Named after George Boole, mathematician who studied logic

---

## String Interpolation

Swift lets us combine strings in two ways: using `+` or **string interpolation**. One is straightforward but inefficient. The other is cleaner, faster, and handles different data types automatically.

### The + Operator Approach

Using `+`, you can join strings together:

```swift
let greeting = "Hello, " + "world!"

let people = "Haters"
let action = "hate"
let lyric = people + " gonna " + action  // "Haters gonna hate"
```

This works fine for simple cases. Using `+` for strings is an example of **operator overloading**—the same operator can add numbers or join strings. `+=` also works for strings.

### The Problem with +

Here's the catch: using `+` repeatedly is inefficient. Swift creates temporary strings for each join:

```swift
let code = "1" + "2" + "3" + "4" + "5"
// Swift creates "12", then "123", then "1234", then "12345"
// Four temporary strings that immediately get thrown away
```

For small strings this doesn't matter. But in real apps where you're building complex strings repeatedly, this adds up.

Also, `+` doesn't work with different types:

```swift
let age = 26
let message = "I am " + age  // Error: cannot concatenate String and Int

// Works but clunky: manually convert to String
let message = "I am " + String(age)
```

### String Interpolation: The Better Way

**String interpolation** is more efficient and flexible. It uses a backslash and parentheses to insert variables directly into strings:

```swift
let name = "Taylor"
let age = 26
let message = "Hello, my name is \(name) and I'm \(age) years old."
```

Notice we didn't convert `age` to a string manually—string interpolation handled it automatically.

You can include integers, decimals, calculations, or any Swift value:

```swift
let number = 11
let mission = "Apollo \(number) landed on the moon."

print("5 x 5 is \(5 * 5)")  // "5 x 5 is 25"
```

Compare this to the old way:

```swift
let missionMessage = "Apollo " + String(number) + " landed on the moon."
```

String interpolation is cleaner, faster, and more versatile. It's the standard approach in Swift.

### Common Mistakes

```swift
// ❌ Mixing types with + operator
let age = 26
let message = "I am " + age  // Error: cannot concatenate String and Int

// ⚠️ Works but clunky: manually convert to String
let message = "I am " + String(age)

// ✅ Better: use string interpolation
let message = "I am \(age)"

// ❌ Forgetting the backslash
let message = "Hello (name)"  // Prints "Hello (name)" literally

// ✅ Include backslash
let message = "Hello \(name)"

// String interpolation works with any type
let isActive = true
let status = "Status: \(isActive)"  // "Status: true"
```

**Key Insights:**

- Use string interpolation `\(variable)` instead of `+` for joining strings
- More efficient: doesn't create temporary strings like `+` does
- Cleaner: no manual type conversion needed
- Handles automatic type conversion for any data type
- Works with expressions: `\(5 * 5)` evaluates and inserts result
- Standard approach in Swift—prefer it over `+`
- String interpolation inserts dynamic data at runtime, essential for showing user-specific information
- For advanced formatting and custom string interpolation: [Super-powered String Interpolation](https://www.hackingwithswift.com/articles/178/super-powered-string-interpolation-in-swift-5-0)

---

## Summary: Simple Data Types

**Constants and Variables**

- Use `let` for constants, `var` for variables
- Prefer `let` to prevent accidental changes and help Swift optimize your code

**Strings**

- Double quotes for single lines, triple quotes for multi-line
- Methods: `.count`, `.uppercased()`, `.hasPrefix()`, `.hasSuffix()`
- Join with `+` or string interpolation `\(variable)` (prefer interpolation)

**Integers**

- Whole numbers with operators: `+`, `-`, `*`, `/`, `+=`, `-=`, `*=`, `/=`
- Methods: `.isMultiple(of:)`
- Use underscores for readability: `100_000_000`

**Doubles**

- Decimal numbers (double-precision floating-point)
- Not perfectly precise due to how computers store decimals
- Can't mix with Int without explicit conversion

**Booleans**

- Store `true` or `false`
- Flip with `!` operator (creates new value) or `.toggle()` method (modifies in place)
- Used for conditions, flags, and state tracking

**String Interpolation**

- Insert values into strings: `\(variable)`
- Automatic type conversion
- More efficient than `+` for building strings

**Type Safety**

- Swift determines type based on initial value
- Type can't change once set
- Must convert between types explicitly

---

## Checkpoint 1

Time to write your first real program from scratch. No copying and pasting, no following line-by-line instructions—just you and the skills you've learned.

### The Challenge

Convert temperatures from Celsius to Fahrenheit.

**Your task:** Create a Swift playground that:

- Creates a constant holding any Celsius temperature
- Converts it to Fahrenheit by multiplying by 9, dividing by 5, then adding 32
- Prints both Celsius and Fahrenheit values for the user

### Hints

- Use `let` to create your constant—naming it `celsius` would make sense
- Celsius is usually a decimal, so use something like `25.0` instead of `25`
- Use `*` for multiplication and `/` for division
- Use `\(variableName)` for string interpolation when printing
- You can type the degrees symbol (°) with Option+Shift+8, allowing output like `25°C` or `77°F`

### Why This Matters

This checkpoint tests whether you've absorbed the fundamentals. It's deliberately simple—you have all the knowledge you need. But there's a big difference between following a tutorial and building something yourself.

If you get stuck, that's normal. Reread the relevant sections, experiment, and don't be afraid to make mistakes. Every error message is teaching you something.

**Try building it yourself before looking at any solution.** Struggling is part of learning—spend time thinking about it first!

---

**Topics:** [Booleans](https://www.hackingwithswift.com/quick-start/beginners/how-to-store-truth-with-booleans) • [String Interpolation](https://www.hackingwithswift.com/quick-start/beginners/how-to-join-strings-together) • [Simple Data Summary](https://www.hackingwithswift.com/quick-start/beginners/summary-simple-data) • [Checkpoint 1](https://www.hackingwithswift.com/quick-start/beginners/checkpoint-1)