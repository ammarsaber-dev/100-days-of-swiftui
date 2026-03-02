**March 02, 2026** • [Day 013 - Protocols and extensions](Day%20013%20-%20Protocols%20and%20extensions.md) ← → [Day 015 - Swift review](Day%20015%20-%20Swift%20review.md)

Null references – literally when a variable has no value – were called "my billion-dollar mistake" by their inventor Tony Hoare. This final day of Swift fundamentals is devoted entirely to Swift's solution: optionals. An optional answers the question "what if our variable doesn't have any value at all?"

---

## Quick Reference

```swift
// ── if let ───────────────────────────────────────────────────────────────────
// Unwrap inside braces; else runs if optional is empty
let opposites = ["Mario": "Wario", "Luigi": "Waluigi"]

if let marioOpposite = opposites["Mario"] {
    print("Mario's opposite is \(marioOpposite)")
} else {
    print("The optional was empty.")
}

// Shadowing — unwrap into a constant of the same name
func square(number: Int) -> Int { number * number }
var number: Int? = nil

if let number = number {           // long form
    print(square(number: number))
}

if let number {                    // shorthand (same result)
    print(square(number: number))
}

// ── guard let ────────────────────────────────────────────────────────────────
// Braces run when optional IS empty; unwrapped value lives after the block
func printSquare(of number: Int?) {
    guard let number = number else {
        print("Missing input")
        return                     // must exit current scope
    }
    // number is available here
    print("\(number) x \(number) is \(number * number)")
}

// if let vs guard let side by side
func printMeaningOfLife() {
    if let name = getMeaningOfLife() { print(name) }  // value only inside braces
}

func printMeaningOfLife() {
    guard let name = getMeaningOfLife() else { return }
    print(name)                    // value available here — happy path stays unindented
}

// ── Nil Coalescing (??) ───────────────────────────────────────────────────────
// Unwrap or fall back to a default; result is always non-optional
let captains = ["Enterprise": "Picard", "Voyager": "Janeway", "Defiant": "Sisko"]
let new = captains["Serenity"] ?? "N/A"              // missing dictionary key

let tvShows = ["Archer", "Babylon 5", "Ted Lasso"]
let favorite = tvShows.randomElement() ?? "None"     // randomElement() returns optional

struct Book { let title: String; let author: String? }
let book = Book(title: "Beowulf", author: nil)
let author = book.author ?? "Anonymous"              // optional struct property

let input = ""
let number2 = Int(input) ?? 0                        // failable conversion

let savedData = first() ?? second() ?? ""            // chained ?? — tries each in order

// ── Optional Chaining (?.) ────────────────────────────────────────────────────
// nil at any step = whole chain silently returns nil
let names = ["Arya", "Bran", "Robb", "Sansa"]
let chosen = names.randomElement()?.uppercased() ?? "No one"
print("Next in line: \(chosen)")

// Deep chain across multiple optional layers
var optionalBook: Book? = nil
let firstLetter = optionalBook?.author?.first?.uppercased() ?? "A"
print(firstLetter)

// ── try? ─────────────────────────────────────────────────────────────────────
// Converts thrown errors into nil; use when you only care if it worked or not
enum UserError: Error { case badID, networkFailed }

func getUser(id: Int) throws -> String {
    throw UserError.networkFailed
}

if let user = try? getUser(id: 23) {                 // combine with if let
    print("User: \(user)")
}

let user = (try? getUser(id: 23)) ?? "Anonymous"     // combine with ?? (parentheses required)
print(user)
```

---

## Optionals

An optional represents data that might be present or might not. They are marked with `?` after the type name:

```swift
let opposites = [
    "Mario": "Wario",
    "Luigi": "Waluigi"
]

let peachOpposite = opposites["Peach"]
```

We create a `[String: String]` dictionary and try to read a key that doesn't exist. It wouldn't make sense to get a string back if there's no value there, so Swift makes `peachOpposite` a `String?` rather than a `String` — a box that may or may not contain something inside. The special empty value is called `nil`. Any kind of data can be optional, including `Int`, `Double`, `Bool`, enums, structs, and classes.

Think of optionals like Schrödinger's data type — there might be a value inside or there might not, and the only way to find out is to check.

> 💡 For another perspective on optionals, check out this video from Brian Voong: https://www.youtube.com/watch?v=7a7As0uNWOQ

### Non-optionals are a guarantee

The real insight, as Zev Eisenberg put it: "Swift didn't introduce optionals. It introduced non-optionals." A non-optional `Int` guarantees a number is always there — it might be 0 or 1 million, but it's definitely there. An optional `Int` set to `nil` has no value at all — not even 0. The same distinction applies everywhere:

- A non-optional `String` might be `"Hello"` or `""` — both are real strings
- An optional `String` set to `nil` is the complete absence of a string — not even empty
- A non-optional array might have items or be empty — both are different from an optional array set to `nil`

Swift enforces this distinction at compile time. You cannot use an optional where a non-optional is required:

```swift
func square(number: Int) -> Int {
    number * number
}

var number: Int? = nil
print(square(number: number)) // ❌ Won't compile — must unwrap first
```

To use the optional, we must unwrap it first:

```swift
if let unwrappedNumber = number {
    print(square(number: unwrappedNumber))
}
```

---

## Unwrapping with `if let`

The most common way to unwrap an optional. It combines a condition (`if`) with creating a constant (`let`) and does three things at once: reads the optional value, unwraps it if it has something inside, and runs the body only if unwrapping succeeded:

```swift
if let marioOpposite = opposites["Mario"] {
    print("Mario's opposite is \(marioOpposite)")
}
```

You can add an `else` block — it's just a normal condition:

```swift
var username: String? = nil

if let unwrappedName = username {
    print("We got a user: \(unwrappedName)")
} else {
    print("The optional was empty.")
}
```

### Shadowing

It's very common to unwrap into a constant of the same name. This creates a temporary second constant available only inside the body — called **shadowing**:

```swift
if let number = number {
    print(square(number: number))
}
```

Swift even allows a shorthand that avoids the repetition entirely:

```swift
if let number {
    print(square(number: number))
}
```

Both do the same thing — inside the braces `number` is a real `Int`; outside it's still an `Int?`.

---

## Unwrapping with `guard let`

`guard let` also unwraps optionals but flips the logic — its braces run when the optional is _empty_, and it requires you to exit the current scope:

```swift
func printSquare(of number: Int?) {
    guard let number = number else {
        print("Missing input")

        // 1: We *must* exit the function here
        return
    }

    // 2: `number` is still available outside of `guard`
    print("\(number) x \(number) is \(number * number)")
}
```

Compared directly:

```swift
var myVar: Int? = 3

if let unwrappedIf = myVar {
    print("Run if myVar has a value inside")
    // unwrappedIf's scope ends here
}
// unwrappedIf can't be used here anymore!

guard let unwrappedGuard = myVar else {
    print("Run if myVar doesn't have a value inside")
}
// unwrappedGuard can be used here as well!
```

`guard` provides the ability to check whether our program state is what we expect, and bail out if it isn't — this is called an **early return**. Swift enforces two rules when using `guard`:

1. You must use `return` (or `break`/`continue`) if the check fails
2. The unwrapped value is available _after_ the guard block, not just inside it

### When to use which

Prefer `guard let` when validating a function's inputs at the start — it lets the rest of the function focus on the "happy path". Here's the same logic written both ways:

```swift
// if let version
func printMeaningOfLife() {
    if let name = getMeaningOfLife() {
        print(name)
    }
}

// guard let version — happy path is less nested
func printMeaningOfLife() {
    guard let name = getMeaningOfLife() else {
        return
    }
    print(name)
}
```

Use `if let` when you just want to do something with the value. Use `guard let` when you specifically need to confirm conditions are correct before continuing.

> 💡 `guard` works with any condition, not just optionals. For example: `guard someArray.isEmpty else { return }`.

---

## Nil Coalescing

The nil coalescing operator `??` unwraps an optional and provides a default value if it's empty — the result is always a non-optional:

```swift
let captains = [
    "Enterprise": "Picard",
    "Voyager": "Janeway",
    "Defiant": "Sisko"
]

let new = captains["Serenity"] ?? "N/A"
```

That reads the value from the dictionary and attempts to unwrap it. If the optional has a value it's sent back; if not, `"N/A"`is used instead. Either way `new` is a real `String`, not a `String?`.

> 💡 Dictionaries also support a `default:` parameter — `captains["Serenity", default: "N/A"]` — which produces the same result. Both are valid, but nil coalescing works with _any_ optional, not just dictionaries.

It works anywhere you have an optional:

```swift
// randomElement() returns optional because the array might be empty
let tvShows = ["Archer", "Babylon 5", "Ted Lasso"]
let favorite = tvShows.randomElement() ?? "None"

// optional property on a struct
struct Book {
    let title: String
    let author: String?
}

let book = Book(title: "Beowulf", author: nil)
let author = book.author ?? "Anonymous"
print(author)

// failable conversion — Int("Hello") returns nil
let input = ""
let number = Int(input) ?? 0
print(number)
```

You can also chain `??` — Swift tries each in order and uses the first non-nil result:

```swift
let savedData = first() ?? second() ?? ""
```

---

## Optional Chaining

Optional chaining lets you read optionals _inside_ optionals in a single line using `?` before each step. If any step is `nil`, the whole chain silently returns `nil` without crashing:

```swift
let names = ["Arya", "Bran", "Robb", "Sansa"]

let chosen = names.randomElement()?.uppercased() ?? "No one"
print("Next in line: \(chosen)")
```

`randomElement()` returns an optional because the array might be empty. The `?.` means "if there's a value, unwrap it and call `uppercased()`; otherwise return nil." The result is still an optional, so nil coalescing provides a fallback.

Optional chaining makes a great companion to nil coalescing. Chains can go as deep as needed — the moment any part returns `nil`, the rest of the line is skipped:

```swift
struct Book {
    let title: String
    let author: String?
}

var book: Book? = nil
let author = book?.author?.first?.uppercased() ?? "A"
print(author)
```

Reading left to right: "if we have a book, and the book has an author, and the author has a first letter, uppercase it and send it back — otherwise send back A." That covers four optional layers in one clean line.

> 💡 For more information on optional chaining, check out this blog post from Andy Bargh: https://andybargh.com/optional-chaining/

---

## Handling Function Failure with `try?`

When calling a throwing function, we normally use `try` and handle errors with `catch`, or `try!` to force-unwrap and crash if it fails (use `try!` very rarely). A third option is `try?`, which converts any thrown error into `nil`, making the function return an optional instead:

```swift
enum UserError: Error {
    case badID, networkFailed
}

func getUser(id: Int) throws -> String {
    throw UserError.networkFailed
}

if let user = try? getUser(id: 23) {
    print("User: \(user)")
}
```

We don't get to know exactly what error was thrown, but often that's fine — we just care whether the call succeeded or not. Compare to the full `do`/`try`/`catch`:

```swift
// Full error handling — know exactly what failed
do {
    let result = try runRiskyFunction()
    print(result)
} catch {
    // it failed!
}

// try? — only care if it worked or not
if let result = try? runRiskyFunction() {
    print(result)
}
```

Combine with nil coalescing for a default on failure — wrap `try?` in parentheses so Swift parses it correctly:

```swift
let user = (try? getUser(id: 23)) ?? "Anonymous"
print(user)
```

### When to use `try?`

`try?` is most useful in three situations:

1. With `guard let` — exit the current function if the `try?` call returns `nil`
2. With nil coalescing — attempt something or provide a default value on failure
3. When calling a throwing function with no return value and you genuinely don't care if it fails — e.g. writing to a log file or sending analytics to a server

> 💡 If you need to know _what_ error occurred, use `do`/`try`/`catch` instead.

---

## Checkpoint 9

Write a function — in a **single line of code** — that accepts an optional array of integers and returns one randomly. If the array is missing or empty, return a random number in the range 1 through 100.

Hints: the function should accept `[Int]?` and return a non-optional `Int`. Use optional chaining to call `randomElement()` on the optional array, then nil coalescing to fall back to a random number.

**Solution:**

```swift
func randomFrom(_ numbers: [Int]?) -> Int {
    numbers?.randomElement() ?? Int.random(in: 1...100)
}
```

Optional chaining calls `randomElement()` on the optional array (returning another optional), then nil coalescing falls back to a random number if either the array or the random element is `nil` — all in one line.

---

**Topics:** [Optionals](https://www.hackingwithswift.com/quick-start/beginners/how-to-handle-missing-data-with-optionals) • [Unwrapping with guard](https://www.hackingwithswift.com/quick-start/beginners/how-to-unwrap-optionals-with-guard) • [Nil Coalescing](https://www.hackingwithswift.com/quick-start/beginners/how-to-unwrap-optionals-with-nil-coalescing) • [Optional Chaining](https://www.hackingwithswift.com/quick-start/beginners/how-to-handle-multiple-optionals-using-optional-chaining) • [Optional try](https://www.hackingwithswift.com/quick-start/beginners/how-to-handle-function-failure-with-optionals)