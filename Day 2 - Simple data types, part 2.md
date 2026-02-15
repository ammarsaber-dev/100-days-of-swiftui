**Date: February 15, 2026**

> **Quick Summary:** Today you'll learn about Booleans (true/false values) and string interpolation (building strings with dynamic data). Plus your first checkpoint where you'll write real code from scratch. Two simple concepts that you'll use in literally every app you build.

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

## Today's Mission

"Lynch's Law" says that when things get tough, people quit — so well done for coming back for day 2!

Yesterday you learned about simple data types (single values like numbers and strings). Today, you'll continue with Booleans (storing true/false values) and string interpolation (building strings from other values).

Today also includes your first checkpoint, where you'll write your own code to test your understanding. Starting from scratch can be challenging, but you'll have plenty of time and helpful hints.

### Day 2 Topics

1. [How to store truth with Booleans](https://www.hackingwithswift.com/quick-start/beginners/how-to-store-truth-with-booleans)
    - Test: [Doubles and Booleans](https://www.hackingwithswift.com/review/sixty/doubles-and-booleans)
2. [How to join strings together](https://www.hackingwithswift.com/quick-start/beginners/how-to-join-strings-together)
    - Optional: [Why does Swift have string interpolation?](https://www.hackingwithswift.com/quick-start/understanding-swift/why-does-swift-have-string-interpolation)
    - Test: [String interpolation](https://www.hackingwithswift.com/review/sixty/string-interpolation)
3. [Summary: Simple data](https://www.hackingwithswift.com/quick-start/beginners/summary-simple-data)

When you’re ready, please proceed onto the checkpoint:
- [Checkpoint 1](https://www.hackingwithswift.com/quick-start/beginners/checkpoint-1)

---

## 1. How to Store Truth with Booleans

Alongside strings, integers, and decimals, there's another simple data type: **Boolean**. A Boolean stores either `true` or `false`. It's named after mathematician George Boole, who studied logic.

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

You can create Booleans directly:

```swift
let goodDogs = true
let gameOver = false
```

Or assign them from expressions that evaluate to true or false:

```swift
let isMultiple = 120.isMultiple(of: 3)
```

### Flipping Boolean Values

Booleans don't support arithmetic like `+` or `-`, but they do have the `!` operator, meaning "not," which flips the value:

```swift
var isAuthenticated = false
isAuthenticated = !isAuthenticated  // false → true
```

This switches `false` to `true`, and vice versa.

Booleans also have a `toggle()` method that flips their value in place:

```swift
var gameOver = false
gameOver.toggle()  // flips from false to true
```

This does the same as `!`, and is especially helpful in more complex code where you just want to flip a state without caring what it currently is.

### Common Mistakes

```swift
// WRONG: Trying to toggle a constant
let gameOver = false
gameOver.toggle()  // Error: cannot mutate constant

// CORRECT: Use var for values that change
var gameOver = false
gameOver.toggle()

// Remember: ! creates a new value, doesn't modify the original
let isEnabled = true
let isDisabled = !isEnabled  // isDisabled is false, isEnabled still true
```

**Key Takeaway:** Booleans store `true` or `false`. Use `!` to create a flipped copy. Use `.toggle()` to flip in place. Must use `var` to toggle.

---

## 2. How to Join Strings Together

Swift lets us combine strings in two ways: using `+` or **string interpolation**. One is straightforward but inefficient. The other is cleaner, faster, and handles different data types automatically.

### The + Operator Approach

Using `+`, you can join strings together:

```swift
let greeting = "Hello, " + "world!"
let people = "Haters"
let action = "hate"
let lyric = people + " gonna " + action  // "Haters gonna hate"
```

This works fine for simple cases. Using `+` for strings is an example of **operator overloading** — the same operator can add numbers or join strings. `+=` also works for strings.

### The Problem with +

Here's the catch: using `+` repeatedly is inefficient. Swift creates temporary strings for each join:

```swift
let code = "1" + "2" + "3" + "4" + "5"
// Swift creates "12", then "123", then "1234", then "12345"
// Four temporary strings that immediately get thrown away
```

For small strings this doesn't matter. But in real apps where you're building complex strings repeatedly, this adds up.

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
// WRONG: Mixing types with + operator
let age = 26
let message = "I am " + age  // Error: cannot concatenate String and Int

// Works but clunky: manually convert to String
let message = "I am " + String(age)

// Better: use string interpolation
let message = "I am \(age)"

// WRONG: Forgetting the backslash
let message = "Hello (name)"  // Prints "Hello (name)" literally

// CORRECT: Include backslash
let message = "Hello \(name)"

// String interpolation works with any type
let isActive = true
let status = "Status: \(isActive)"  // "Status: true"
```

**Key Takeaway:** Use string interpolation `\(variable)` instead of `+`. It's more efficient, cleaner, and handles type conversion automatically. Works with any data type.

### Optional: Why Does Swift Have String Interpolation?

Swift uses string interpolation to insert dynamic data into strings at runtime. This is useful when showing information to your user—whether messages being printed, text on buttons, or other app content—because we usually want to display relevant data rather than static strings.

String interpolation replaces parts of a string with values we provide:

```swift
var city = "Cardiff"
var message = "Welcome to \(city)!"
```

In this simple example, we could have just written `"Welcome to Cardiff!"`, but in real apps, dynamic insertion lets us show real-world user data rather than hardcoded text.

Swift can place any kind of data inside string interpolation. The result might not always be meaningful, but for all basic Swift types—strings, integers, Booleans, etc.—the results work seamlessly.

**Going Deeper:** String interpolation is extremely powerful in Swift. For advanced examples like custom formatting and complex expressions, see: https://www.hackingwithswift.com/articles/178/super-powered-string-interpolation-in-swift-5-0

---

## 3. Summary: Simple Data

We've covered a lot about the basics of data. Here's everything in one place:

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

It's a lot, but you'll use these repeatedly in apps until it all feels natural.

---

## 4. Checkpoint 1

Time to write your first real program from scratch. No copying and pasting, no following line-by-line instructions—just you, your keyboard, and the skills you've learned.

### The Challenge

Convert temperatures from Celsius to Fahrenheit.

**Your task:** Create a Swift playground that:

- Creates a constant holding any Celsius temperature
- Converts it to Fahrenheit by multiplying by 9, dividing by 5, then adding 32
- Prints both Celsius and Fahrenheit values for the user

### Hints (Use These If You Get Stuck)

- Use `let` to create your constant—naming it `celsius` would make sense
- Celsius is usually a decimal, so use something like `25.0` instead of `25`
- Use `*` for multiplication and `/` for division
- Use `\(variableName)` for string interpolation when printing
- You can type the degrees symbol (°) with Option+Shift+8, allowing output like `25°C` or `77°F`

### Why This Matters

This checkpoint tests whether you've absorbed the fundamentals. It's deliberately simple—you have all the knowledge you need. But there's a big difference between following a tutorial and building something yourself.

If you get stuck, that's normal. Reread the relevant sections, experiment, and don't be afraid to make mistakes. Every error message is teaching you something.

Try building it yourself before looking at any solution. The course will get more challenging soon, and this ensures you've fully understood the fundamentals.

---

## Where Now?

You've completed Day 2. You now understand all the basic simple data types in Swift: strings, integers, doubles, and Booleans. You know how to store values, perform calculations, and build dynamic strings.

More importantly, you've written your first program from scratch. That's a real milestone—many people never get this far.

**Tomorrow:** You'll move from simple data to collections—arrays, dictionaries, sets, and enums. These let you store multiple values together, which is essential for building real apps.

Keep going. You're making real progress.

---

**Previous:** [Day 1 - First steps in Swift](Day%201%20-%20First%20steps%20in%20Swift.md) | **Next:** [Day 3 - Complex data types, part 1](Day%203%20-%20Complex%20data%20types,%20part%201.md)
