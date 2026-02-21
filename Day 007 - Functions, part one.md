**February 17, 2026** •[Day 006 - Loops](Day%20006%20-%20Loops.md) ← → [Day 008 - Functions, part two](Day%20008%20-%20Functions,%20part%20two.md)

Functions let you wrap up pieces of code, give them a name, and reuse them anywhere. Rather than copying and pasting the same 10 lines in a dozen places, you write it once and call it wherever you need it. Change the function later, and every place that uses it gets updated automatically.

---

## Quick Reference

```swift
// Basic function
func showWelcome() {
    print("Welcome to my app!")
    print("By default This prints out a conversion")
    print("chart from centimeters to inches, but you")
    print("can also set a custom range if you want.")
}
showWelcome()

// Function with parameters
func printTimesTables(number: Int, end: Int) {
    for i in 1...end {
        print("\(i) x \(number) is \(i * number)")
    }
}
printTimesTables(number: 5, end: 20)

// Returning a value
func rollDice() -> Int {
    return Int.random(in: 1...6)
}

let result = rollDice()
print(result)

// Returning a tuple
func getUser() -> (firstName: String, lastName: String) {
    (firstName: "Taylor", lastName: "Swift")
}

let user = getUser()
print("Name: \(user.firstName) \(user.lastName)")

// Destructuring a tuple
let (firstName, lastName) = getUser()
print("Name: \(firstName) \(lastName)")

// External and internal parameter names
func printTimesTables(for number: Int) {
    for i in 1...12 {
        print("\(i) x \(number) is \(i * number)")
    }
}
printTimesTables(for: 5)

// Omitting the external label with _
func isUppercase(_ string: String) -> Bool {
    string == string.uppercased()
}

let string = "HELLO, WORLD"
let result = isUppercase(string)
```

---

## Writing Functions

### Your first function

To create a function, use the `func` keyword, give it a name, and put your code inside the braces:

```swift
func showWelcome() {
    print("Welcome to my app!")
    print("By default This prints out a conversion")
    print("chart from centimeters to inches, but you")
    print("can also set a custom range if you want.")
}
```

To run it, just call it by name — this is known as the **call site**:

```swift
showWelcome()
```

The `()` after the name is where configuration options go. You've already seen this with built-in functions: `isMultiple(of: 2)` lets you check any multiple you want, and `Int.random(in: 1...20)` lets you control the range. Without those parentheses and parameters, these functions would be far less useful — you'd need a separate function for every possible case.

### When should you create a function?

There are three good reasons to pull code into a function:

1. **Reuse** — you want the same behavior in multiple places. Change it once, it updates everywhere.
2. **Clarity** — breaking one long function into smaller named ones makes your code much easier to follow.
3. **Composition** — Swift lets you build new functions out of existing ones, combining small pieces like Lego bricks.

> 💡 If a function needs six or more parameters, that's a "code smell" — a sign it might be doing too much and should be broken up into smaller functions.

---

## Parameters

Parameters are what make functions flexible. You define what data the function needs, and the caller provides it:

```swift
func printTimesTables(number: Int) {
    for i in 1...12 {
        print("\(i) x \(number) is \(i * number)")
    }
}

printTimesTables(number: 5)
```

Inside the function, `number` works like any other constant. When calling the function, you must write the parameter name explicitly — `printTimesTables(number: 5)`, not just `printTimesTables(5)`. This makes it much clearer what each value does, especially when reading code months later.

### Multiple parameters

You can have as many parameters as you need:

```swift
func printTimesTables(number: Int, end: Int) {
    for i in 1...end {
        print("\(i) x \(number) is \(i * number)")
    }
}

printTimesTables(number: 5, end: 20)
```

Parameters must always be passed in the same order they were defined. This is invalid:

```swift
printTimesTables(end: 20, number: 5)  // ❌ Wrong order
```

### Parameters vs Arguments

You might hear both terms. Technically:

- **Parameter** — the placeholder name in the function definition (`number: Int`)
- **Argument** — the actual value passed in when calling (`number: 5`)

In practice, most Swift developers (including Paul Hudson) just use "parameter" for both, and it causes no confusion.

> 💡 Any data you create inside a function is automatically destroyed when the function finishes.

---

## Returning Values

To send a value back from a function, add `->` and the return type before the opening brace, then use `return` to send the value:

```swift
func rollDice() -> Int {
    return Int.random(in: 1...6)
}

let result = rollDice()
print(result)
```

> ⚠️ Swift enforces return types strictly — if you say a function returns an `Int`, it must always return an `Int`. Your code won't even build if you forget.

### Skipping the return keyword

When a function contains only a single expression, you can drop the `return` keyword entirely:

```swift
func rollDice() -> Int {
    Int.random(in: 1...6)
}
```

This works because Swift sees there's only one line of code and knows it must be the return value. It only works for **expressions** — code that resolves to a single value, like `5 + 8` or `Int.random(in: 1...6)`. It does NOT work for **statements** — things like creating a variable (`let name = "Otis"`), starting a loop, or checking a condition with multiple lines. Those are actions, not values, so they can't be returned implicitly.

Remember, this only works when your function contains a single line of code, and that line must actually return the data you promised.

Swift is also smart enough to allow simple `if` and `switch` as expressions, as long as each branch returns a value directly:

```swift
func greet(name: String) -> String {
    if name == "Taylor Swift" {
        "Oh wow!"
    } else {
        "Hello, \(name)"
    }
}
```

You can even assign the result of an `if` directly to a variable:

```swift
func greet(name: String) -> String {
    let response = if name == "Taylor Swift" {
        "Oh wow!"
    } else {
        "Hello, \(name)"
    }

    return response
}
```

Think of it like a ternary — the `if` itself becomes the value. This does **not** work if any branch creates a new variable:

```swift
// ❌ Not allowed — creates a new variable inside a branch
func greet(name: String) -> String {
    if name == "Taylor Swift" {
        "Oh wow!"
    } else {
        let greeting = "Hello, \(name)"
        return greeting
    }
}
```

### Real examples

Checking if two strings contain the same letters:

```swift
func areLettersIdentical(string1: String, string2: String) -> Bool {
    string1.sorted() == string2.sorted()
}
```

The Pythagorean theorem:

```swift
func pythagoras(a: Double, b: Double) -> Double {
    sqrt(a * a + b * b)
}

let c = pythagoras(a: 3, b: 4)
print(c)  // 5.0
```

### Using return to exit early

Even in functions that don't return a value, you can use `return` by itself to exit early:

```swift
func checkInput(name: String) {
    if name.isEmpty { return }  // exit immediately if no name
    print("Hello, \(name)!")
}
```

---

## Returning Multiple Values with Tuples

Returning two or more values from a function? You have a few options, but tuples are the best.

**Arrays** work but are fragile — you have to remember what index holds what:

```swift
func getUser() -> [String] {
    ["Taylor", "Swift"]
}

let user = getUser()
print("Name: \(user[0]) \(user[1])")  // what was index 0 again?
```

**Dictionaries** are clearer but force you to provide default values even when you know the key exists:

```swift
func getUser() -> [String: String] {
    [
        "firstName": "Taylor",
        "lastName": "Swift"
    ]
}

let user = getUser()
print("Name: \(user["firstName", default: "Anonymous"]) \(user["lastName", default: "Anonymous"])")  // the default is annoying
```

**Tuples** give you the best of both — named access without the uncertainty:

```swift
func getUser() -> (firstName: String, lastName: String) {
    (firstName: "Taylor", lastName: "Swift")
}

let user = getUser()
print("Name: \(user.firstName) \(user.lastName)")
```

Why tuples win over dictionaries: Swift knows exactly what values exist in the tuple and their types — no defaults needed. You access values with dot syntax (`user.firstName`), not strings, so no typos. The tuple can only contain what you defined — no surprise extra values.

### Skipping names on return

When returning a named tuple, Swift already knows the names from the return type, so you don't need to repeat them:

```swift
func getUser() -> (firstName: String, lastName: String) {
    ("Taylor", "Swift")  // no need to repeat the names here
}
```

### Unnamed tuples

If the tuple doesn't have named elements, access by index:

```swift
func getUser() -> (String, String) {
    ("Taylor", "Swift")
}

let user = getUser()
print("Name: \(user.0) \(user.1)")
```

Named elements are almost always preferable.

### Destructuring tuples

Rather than accessing values through the tuple, you can pull it apart into individual constants:

```swift
func getUser() -> (firstName: String, lastName: String) {
    (firstName: "Taylor", lastName: "Swift")
}

let user = getUser()
let firstName = user.firstName
let lastName = user.lastName

print("Name: \(firstName) \(lastName)")

// Shorthand — destructure directly:
let (firstName, lastName) = getUser()
print("Name: \(firstName) \(lastName)")
```

If you don't need all the values, use `_` to ignore the ones you don't:

```swift
let (firstName, _) = getUser()
print("Name: \(firstName)")
```

### Array vs Set vs Tuple — which to use?

- **Array** — ordered list, allows duplicates. Use for high scores, to-do lists, ordered articles.
- **Set** — unordered, no duplicates. Use for dictionary words in a game, or tracking which articles a user has read (when order doesn't matter).
- **Tuple** — fixed number of values with fixed types. Use when you need precisely two strings, or a string and an integer, etc.

---

## Customizing Parameter Labels

Swift lets you define two names for each parameter: one used **outside** the function (at the call site), and one used **inside** the function body.

### Removing the external label with `_`

When the parameter name is obvious from context, you can remove the external label entirely using `_`:

```swift
func isUppercase(_ string: String) -> Bool {
    string == string.uppercased()
}

let string = "HELLO, WORLD"
let result = isUppercase(string)
```

This is great when your function name is a verb and the parameter is the noun it acts on: `greet(taylor)` instead of `greet(person: taylor)`, `buy(toothbrush)` instead of `buy(item: toothbrush)`, `sing(song)` instead of `sing(song: song)`.

> 💡 You'll see `_` used a lot in Apple's older UIKit/AppKit frameworks, which were designed for Objective-C where the first parameter was always unnamed.

### Using different external and internal names

Sometimes the label that reads well at the call site doesn't work well inside the function. You can define both:

```swift
func printTimesTables(for number: Int) {
    for i in 1...12 {
        print("\(i) x \(number) is \(i * number)")
    }
}

printTimesTables(for: 5)
```

Breaking that down: `for` is the **external** name used when calling the function, `number` is the **internal** name used inside the function body. `for` reads naturally at the call site ("print times tables for 5"), but can't be used inside the function since it's a Swift keyword.

So Swift gives us two tools to control parameter labels: use `_` to remove the external label entirely, or use two names (`for number`) to have different labels inside and outside.

---

**Topics:** [Writing Functions](https://www.hackingwithswift.com/quick-start/beginners/how-to-reuse-code-with-functions) • [Returning Values](https://www.hackingwithswift.com/quick-start/beginners/how-to-return-values-from-functions) • [Tuples](https://www.hackingwithswift.com/quick-start/beginners/how-to-return-multiple-values-from-functions) • [Parameter Labels](https://www.hackingwithswift.com/quick-start/beginners/how-to-customize-parameter-labels)