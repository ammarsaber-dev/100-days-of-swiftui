**February 15, 2026** • [Day 0 - How to become an iOS developer](Day%200%20-%20How%20to%20become%20an%20iOS%20developer.md) ← → [Day 002 - Simple data types, part 2](Day%20002%20-%20Simple%20data%20types,%20part%202.md)

Your first day covers the fundamentals—variables, constants, strings, integers, and doubles. These are the building blocks of every app you'll ever write.

---

## Quick Reference

```swift
// Variables & Constants
var name = "Ted"        // can change
let character = "Roy"   // can't change

// Strings
let text = "Hello"           // String
let multiLine = """
Multiple lines
of text
"""

// Numbers
let whole = 42               // Int (whole numbers)
let decimal = 3.14           // Double (decimals)
let readable = 100_000_000   // underscores for readability

// Operations
var counter = 10
counter += 5                 // compound assignment
counter *= 2

// String methods
text.count                   // character count
text.uppercased()           // uppercase
text.hasPrefix("He")        // starts with?
text.hasSuffix("lo")        // ends with?

// Integer methods
number.isMultiple(of: 3)    // is multiple?

// Type conversion
let a = 1
let b = 2.0
let c = Double(a) + b       // must convert explicitly
```

---

## Getting Started

### Why Swift?

Swift (launched in 2014) is a modern, powerful language designed to be safe, clear, and efficient. It avoids the complexity of older languages, reduces unsafe code, improves readability, and supports global languages.

Swift itself doesn't build user interfaces—that's where SwiftUI comes in. SwiftUI is Apple's framework built specifically for Swift, making it fast and easy to create interactive apps.

### How to Follow Along

Install Xcode 16.2 or later from the Mac App Store (free). We'll use Swift Playgrounds for all examples.

In Xcode, choose **File > New > Playground**, select **Blank** under macOS, and save it somewhere convenient. Playgrounds let you write Swift code and see results side by side.

You can use one playground for the whole course or create a new one per day—whatever works best for you.

**Tip:** The first run may take a few seconds; after that, it runs quickly.

---

## Variables and Constants

Swift uses **variables** (`var`) and **constants** (`let`) to store data. This is fundamental—every app uses both.

### Variables Can Change

```swift
var name = "Ted"
name = "Rebecca"
name = "Keeley"
```

Variables hold temporary information that needs to be updated. Your app takes data—user input, downloaded content, device info—transforms it, and shows it to the user. Variables handle that transformation. They're central to almost every Swift program.

### Constants Cannot Change

```swift
let character = "Daphne"
// character = "Eloise"  // Error: cannot assign to constant
```

Constants are like making a contract with Swift: "this value matters, don't let it change." Once set, the value is locked in. This prevents accidental changes and helps avoid bugs.

`let` comes from math: "let x equal 5." Prefer constants when possible—they're safer and help Swift optimize your code.

### Printing Values

```swift
var playerName = "Roy"
print(playerName)

playerName = "Dani"
print(playerName)

playerName = "Sam"
print(playerName)
```

Run code in Xcode with the blue play button.

### Naming Conventions

Swift uses **camelCase**. Be consistent—`playerName` ≠ `playername`:

```swift
let managerName = "Michael Scott"
let dogBreed = "Samoyed"
let meaningOfLife = "How many roads must a man walk down?"
```

### Common Mistakes

```swift
// ❌ Redeclaring a variable
var name = "Ted"
var name = "Rebecca"  // Error: redeclaring variable

// ✅ Changing a variable's value (no var/let needed)
var name = "Ted"
name = "Rebecca"

// ❌ Trying to change a constant
let character = "Daphne"
character = "Eloise"  // Error: cannot assign to constant

// ❌ Inconsistent naming (case matters!)
let playername = "Roy"
print(playerName)     // Error: playerName not found
```

Swift enforces clarity: are you creating a new variable, or modifying an existing one? Once created, don't redeclare with `var` or `let` again.

---

## Strings

Swift uses **strings** to store text. Strings go inside **double quotes**:

```swift
let actor = "Denzel Washington"
let filename = "paris.jpg"
let result = "You win!"
```

Strings are everywhere in apps—user names, button labels, error messages, downloaded text. Understanding strings is essential.

### Escaping Quotes

Include quotation marks inside a string by escaping them with a backslash (`\`):

```swift
let quote = "Then he tapped a sign saying \"Believe\" and walked away."
```

### Multi-line Strings

Use triple quotes for multi-line strings:

```swift
let movie = """
A day in
the life of an
Apple engineer
"""
```

The opening and closing `"""` must be on their own lines. Swift preserves the line breaks as part of the string.

Multi-line strings keep your code readable when dealing with longer text—emails, descriptions, articles, or any text that naturally spans multiple lines.

### String Methods

**Counting characters:**

```swift
print(actor.count)
let nameLength = actor.count
print(nameLength)
```

No parentheses needed for `.count` because you're just reading data—it's a property, not doing work.

**Modifying text:**

```swift
print(result.uppercased())
```

Use parentheses `()` for methods that do work, like `.uppercased()`.

**Checking text:**

```swift
print(movie.hasPrefix("A day"))      // does movie start with "A day"?
print(filename.hasSuffix(".jpg"))    // does filename end with ".jpg"?
```

Strings are **case-sensitive**:

```swift
filename.hasSuffix(".JPG")  // false (case matters!)
```

### Common Mistakes

```swift
// ❌ Unescaped quotes inside string
let quote = "He said "Hello""  // Error: unexpected code

// ✅ Escaped quotes
let quote = "He said \"Hello\""

// ❌ Multi-line without triple quotes
let text = "Line one
Line two"  // Error: unterminated string

// ✅ Multi-line string
let text = """
Line one
Line two
"""

// ❌ Case sensitivity ignored
let filename = "Paris.JPG"
filename.hasSuffix(".jpg")  // Returns false (case matters!)
```

---

## Integers

When working with whole numbers like 3, 5, 50, or 5 million, you're working with **integers** (`Int`).

Integers are everywhere: scores, ages, counts, indices, IDs. You'll use them constantly.

### Creating Integers

```swift
let score = 10
```

Integers can be really big (past quintillions) and really small (negative numbers down to quintillions).

### Making Numbers Readable

Use underscores (`_`) to break up numbers:

```swift
let reallyBig = 100_000_000
```

Swift doesn't care about the underscores—this also works:

```swift
let reallyBig = 1_00__00___00____00
```

The underscores are purely for you to make large numbers easier to read.

### Arithmetic Operators

Create integers from other integers using `+`, `-`, `*`, and `/`:

```swift
let lowerScore = score - 2
let higherScore = score + 10
let doubledScore = score * 2
let halvedScore = score / 2
```

### Compound Assignment Operators

Instead of `counter = counter + 5`, use shorthand:

```swift
var counter = 10
counter += 5   // adds 5 directly
counter *= 2   // multiply and assign
counter -= 10  // subtract and assign
counter /= 2   // divide and assign
```

These shortcuts are faster to write and easier to read once you're familiar with them.

### Useful Methods

```swift
let number = 120
print(number.isMultiple(of: 3))  // true
print(120.isMultiple(of: 3))     // works directly too
```

### Common Mistakes

```swift
// ❌ Using compound operators on constants
let score = 10
score += 5  // Error: cannot modify constant

// ✅ Use var for values that change
var score = 10
score += 5

// Note: Integer division truncates decimals
let result = 10 / 3  // result is 3, not 3.333...
// Use Double for decimal results (see next section)

// ❌ Underscores in wrong place
let number = _100_000  // Error: invalid syntax

// ✅ Underscores between digits only
let number = 100_000
```

Integer division truncates (drops) decimal values. If you need decimals, use Double.

---

## Doubles

Decimal numbers like 3.1, 5.56, or 3.141592654 are **floating-point numbers**. The name comes from how computers store them: moving the decimal point around to fit very large and very small numbers in the same space.

### Why Decimals Are Tricky

This causes decimals to be notoriously problematic:

```swift
let number = 0.1 + 0.2
print(number)  // prints 0.30000000000000004, not 0.3
```

Welcome to the strange world of floating-point arithmetic. Computers can't store some decimal numbers exactly, so they use very close approximations. This is true in every programming language, not just Swift.

### Creating Decimals

Swift calls them `Double` (double-precision floating-point number). They can store absolutely massive numbers.

### Type Safety: Can't Mix Int and Double

Decimals are wholly different from integers—you can't mix them. Integers are 100% accurate, decimals aren't:

```swift
let a = 1
let b = 2.0
let c = a + b  // Error: cannot mix types
```

Must convert explicitly:

```swift
let c = a + Int(b)      // Double → Int (loses decimal)
let c = Double(a) + b   // Int → Double (safe)
```

This seems annoying at first, but it prevents bugs. Swift is protecting you from accidentally losing precision. Integers are 100% accurate, doubles are approximations—that's why Swift won't let you mix them accidentally.

### How Swift Decides Types

Dot = `Double`. No dot = `Int`:

```swift
let double3 = 3.0  // Double
let int1 = 3       // Int
```

Once Swift decides a type, it can't change:

```swift
var name = "Nicolas Cage"
name = "John Travolta"  // works (both strings)
name = 57               // Error! (can't change String to Int)
```

This is **type safety** in action. Swift determines the type based on the initial value, then locks it in. You can change the value, but never the type. Type safety prevents data mistakes—as programs grow complex, Swift tracks types for you.

### Operators

Same as integers:

```swift
var rating = 5.0
rating *= 2
```

### CGFloat

Older APIs use `CGFloat`. Modern Swift lets you use `Double` everywhere, so ignore `CGFloat`.

### Common Mistakes

```swift
// ❌ Mixing Int and Double
let a = 1
let b = 2.0
let c = a + b  // Error: cannot mix types

// ✅ Convert explicitly
let c = Double(a) + b  // or: a + Int(b)

// Note: Floating-point arithmetic isn't exact
let result = 0.1 + 0.2
print(result == 0.3)  // false! (it's 0.30000000000000004)

// ❌ Changing variable type
var value = 3.14
value = 3  // Error: cannot assign Int to Double

// ✅ Keep type consistent
var value = 3.14
value = 3.0  // both are Double

// ❌ Forgetting decimal point
let half = 1 / 2  // result is 0 (integer division!)

// ✅ Use Double for decimal results
let half = 1.0 / 2.0  // result is 0.5
```

Computers use binary—they can't store numbers like 1/3 exactly, so they create very close approximations. It's extremely efficient, and the error is usually irrelevant.

---

**Topics:** [Variables & Constants](https://www.hackingwithswift.com/quick-start/beginners/how-to-create-variables-and-constants) • [Strings](https://www.hackingwithswift.com/quick-start/beginners/how-to-create-strings) • [Integers](https://www.hackingwithswift.com/quick-start/beginners/how-to-store-whole-numbers) • [Doubles](https://www.hackingwithswift.com/quick-start/beginners/how-to-store-decimal-numbers)