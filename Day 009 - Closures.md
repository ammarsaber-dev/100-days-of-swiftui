 **February 19, 2026** • [Day 008 - Functions, part two](Day%20008%20-%20Functions,%20part%20two.md) ← → [Day 010 - Structs, part one](Day%20010%20-%20Structs,%20part%20one.md)

Closures are one of the most powerful — and most confusing — features of Swift. They're like anonymous functions you can store in variables and pass around. Once you've climbed this hill, the rest gets much easier. SwiftUI uses them everywhere, so it's worth taking the time to understand them well.

---

## Quick Reference

```swift
// Basic closure
let sayHello = {
    print("Hi there!")
}
sayHello()

// Closure with parameters and return type
let sayHello = { (name: String) -> String in
    "Hi \(name)!"
}

// Closure returning a value with no parameters
let payment = { () -> Bool in
    print("Paying an anonymous person…")
    return true
}

// Passing a closure to sorted()
let team = ["Gloria", "Suzanne", "Piper", "Tiffany", "Tasha"]

let captainFirstTeam = team.sorted {
    if $0 == "Suzanne" { return true }
    if $1 == "Suzanne" { return false }
    return $0 < $1
}

// filter, map
let tOnly = team.filter { $0.hasPrefix("T") }
let uppercased = team.map { $0.uppercased() }

// Function that accepts a function as a parameter
func makeArray(size: Int, using generator: () -> Int) -> [Int] {
    var numbers = [Int]()
    for _ in 0..<size {
        numbers.append(generator())
    }
    return numbers
}

let rolls = makeArray(size: 50) {
    Int.random(in: 1...20)
}
```

---

## Creating and Using Closures

A **closure** is a chunk of code you can store in a variable and call later — essentially an anonymous function.

```swift
let sayHello = {
    print("Hi there!")
}
sayHello()
```

If you want the closure to accept parameters or return a value, those go **inside** the braces, followed by the `in` keyword:

```swift
let sayHello = { (name: String) -> String in
    "Hi \(name)!"
}
```

If you want to return a value but take **no** parameters, you still need the empty parentheses — you can't just write `-> Bool in`:

```swift
let payment = { () -> Bool in
    print("Paying an anonymous person…")
    return true
}
```

The `in` keyword marks the boundary between the closure's signature and its body. It's needed because everything — parameters, return type, and body — lives inside the same pair of braces. Without it, Swift wouldn't know where the body actually starts.

### Why parameters live inside the braces

In a regular function, parameters go outside the braces. Closures can't do that — the whole thing is one block of code stored in a variable. If parameters were outside, it would look like a tuple declaration. Putting them inside, separated by `in`, keeps the whole closure self-contained.

### Function types

Functions and closures have types, just like `Int` or `String`. The type describes what they accept and return, but **not** the external parameter labels:

```swift
var greetCopy: () -> Void = greetUser
```

> ⚠️ When copying a function, don't write the parentheses — `var greetCopy = greetUser`, not `var greetCopy = greetUser()`. The parentheses would _call_ the function and assign its return value instead.

Because labels aren't part of the type, calling a closure (or a copied function) skips external labels entirely:

```swift
sayHello("Taylor")   // no label, even though the closure defined `name`
```

### Passing closures to functions

`sorted()` accepts a custom sorting function — you can pass a named function or write a closure inline:

```swift
let captainFirstTeam = team.sorted(by: { (name1: String, name2: String) -> Bool in
    if name1 == "Suzanne" { return true }
    if name2 == "Suzanne" { return false }
    return name1 < name2
})
```

That's a lot of syntax — but Swift gives us ways to simplify it.

---

## Trailing Closures and Shorthand Syntax

Swift has several tricks to reduce closure verbosity.

### 1. Type inference

Swift already knows `sorted()` needs two `String` parameters returning a `Bool`, so you can drop the types:

```swift
let captainFirstTeam = team.sorted(by: { name1, name2 in
    ...
})
```

### 2. Trailing closure syntax

When a closure is the last parameter to a function, you can move it outside the parentheses and drop the label:

```swift
let captainFirstTeam = team.sorted { name1, name2 in
    if name1 == "Suzanne" { return true }
    if name2 == "Suzanne" { return false }
    return name1 < name2
}
```

### 3. Shorthand parameter names

Swift automatically provides `$0`, `$1`, `$2`… for closure parameters, letting you drop the parameter list entirely:

```swift
let captainFirstTeam = team.sorted {
    if $0 == "Suzanne" { return true }
    if $1 == "Suzanne" { return false }
    return $0 < $1
}
```

For a simple reverse sort, this gets remarkably concise:

```swift
let reverseTeam = team.sorted { $0 > $1 }
```

### When not to use shorthand

Prefer shorthand syntax **unless**:

- The closure body is long
- `$0` and friends are used more than two or three times
- There are three or more parameters (`$2`, `$3`…)

### filter() and map()

Two other common higher-order functions that pair naturally with closures:

```swift
// filter — keep items where the closure returns true
let tOnly = team.filter { $0.hasPrefix("T") }
// ["Tiffany", "Tasha"]

// map — transform every item, returning a new array
let uppercaseTeam = team.map { $0.uppercased() }
// ["GLORIA", "SUZANNE", "PIPER", "TIFFANY", "TASHA"]
```

> 💡 With `map()`, the return type doesn't have to match the input type — you could turn an array of integers into an array of strings.

---

## Accepting Functions as Parameters

You can write functions that accept other functions (or closures) as parameters. The type annotation describes what the function parameter accepts and returns:

```swift
func makeArray(size: Int, using generator: () -> Int) -> [Int] {
    var numbers = [Int]()
    for _ in 0..<size {
        let newNumber = generator()
        numbers.append(newNumber)
    }
    return numbers
}
```

Breaking down the signature:

- `size: Int` — first parameter, an integer
- `using generator: () -> Int` — second parameter, a function that takes no arguments and returns an `Int`
- `-> [Int]` — `makeArray` itself returns an array of integers

Calling it with a trailing closure:

```swift
let rolls = makeArray(size: 50) {
    Int.random(in: 1...20)
}
```

### Multiple trailing closures

When a function accepts several closure parameters, you use **multiple trailing closure syntax** — common in SwiftUI:

```swift
func doImportantWork(first: () -> Void, second: () -> Void, third: () -> Void) {
    first()
    second()
    third()
}

doImportantWork {
    print("First work")
} second: {
    print("Second work")
} third: {
    print("Third work")
}
```

The first closure drops its label; subsequent ones keep theirs.

### Why closures as parameters?

Closures shine for **deferred execution** — work you want to happen later, not immediately. Real-world examples:

- **Network requests** — "fetch this data, and when you're done, run this closure" — so the UI doesn't freeze
- **Animations** — "run this code when the animation finishes"
- **Siri integration** — Apple passes your app a closure to call when you're ready, rather than blocking Siri's UI while waiting for a response

---

## Checkpoint 5

Chain `filter()`, `sorted()`, and `map()` together without temporary variables.

**Input:**

```swift
let luckyNumbers = [7, 4, 38, 21, 16, 15, 12, 33, 31, 49]
```

**Requirements:**

1. Filter out any even numbers
2. Sort in ascending order
3. Map to strings in the format `"7 is a lucky number"`
4. Print each result on its own line

**Expected output:**

```
7 is a lucky number
15 is a lucky number
21 is a lucky number
31 is a lucky number
33 is a lucky number
49 is a lucky number
```

**Solution:**

```swift
let luckyNumbers = [7, 4, 38, 21, 16, 15, 12, 33, 31, 49]

luckyNumbers
    .filter { !$0.isMultiple(of: 2) }
    .sorted()
    .map { "\($0) is a lucky number" }
    .forEach { print($0) }
```

> 💡 Order matters — sort before mapping to strings, otherwise `sorted()` will do a string sort where `"15"` comes before `"7"`.

---

**Topics:** [Closures](https://www.hackingwithswift.com/quick-start/beginners/how-to-create-and-use-closures) • [Trailing Closures](https://www.hackingwithswift.com/quick-start/beginners/how-to-use-trailing-closures-and-shorthand-syntax) • [Functions as Parameters](https://www.hackingwithswift.com/quick-start/beginners/how-to-accept-functions-as-parameters)