**Date: February 15, 2026**

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
## Course Overview

SwiftUI is a powerful framework for building interactive apps across Apple platforms, and it's powered by the Swift programming language. To build SwiftUI apps, you need to understand Swift itself.

As Mark Twain said, "the secret to getting ahead is getting started." So let's begin by learning about variables, constants, and basic data types in Swift.

Today, you have seven tutorials to complete. Optional further reading is available if you have time, but it's not required. There are also short tests to check your understanding of key concepts.

It may be tempting to move ahead to more videos and tests, but don't rush—studying one hour a day consistently is better than cramming with long gaps in between.

### Day 1 Topics

1. Introduction: Why Swift?
2. Introduction: About this course
3. Introduction: How to follow along
4. How to create variables and constants
    - Optional: Why does Swift have variables?
    - Optional: Why does Swift have constants as well as variables?
5. How to create strings
    - Optional: Why does Swift need multi-line strings?
6. How to store whole numbers
7. How to store decimal numbers
    - Optional: Why does Swift need both Doubles and Integers?
    - Optional: Why is Swift a type-safe language?

---

## Introduction

### Why Swift?

Swift (launched in 2014) is a modern, powerful language designed to be safe, clear, and efficient. It avoids the complexity of older languages, reduces unsafe code, improves readability, and supports global languages.

Swift itself doesn't build user interfaces — that's where SwiftUI comes in. SwiftUI is Apple's framework built specifically for Swift, making it fast and easy to create interactive apps.

Swift is clean, safe, practical, and profitable — a strong choice for app development.

### About this course

I've been teaching Swift since its 2014 launch, and Hacking with Swift is now the world's largest site dedicated to Swift. Over the years, I've learned which topics matter most, how to structure them clearly, and how to help learners retain what they study—this course reflects that experience.

Rather than covering every detail of Swift, it focuses on the features you'll use most in real apps, with a few carefully chosen advanced topics. After finishing, you'll likely be ready to move on to SwiftUI.

Each chapter is available in both text and video, covering the same material. The hands-on, sometimes repetitive approach reinforces learning, and the frequent "How to…" titles highlight the book's practical focus.

Programming is an art—don't spend all your time sharpening your pencil when you should be drawing.

### How to follow along

This book includes lots of code, and I strongly encourage you to type it yourself—run it, review the output, and experiment to deepen your understanding.

To follow along, install Xcode 26.2 or later from the Mac App Store. It's free and includes everything you need.

We'll use a Swift Playground for all examples. In Xcode, choose **File > New > Playground**, select **Blank** under macOS, and save it somewhere convenient. Playgrounds act as sandboxes, letting you write Swift code and see results side by side.

You can use one playground for the whole book or create a new one per chapter—whatever suits you best.

**Tip:** The first run may take a few seconds to start; after that, it runs quickly.

---
## How to create variables and constants

Swift uses **variables** (`var`) and **constants** (`let`) to store data.

**Variables** can change:

```swift
var name = "Ted"
name = "Rebecca"
name = "Keeley"
```

**Constants** cannot change:

```swift
let character = "Daphne"
// character = "Eloise" → error
```

`let` comes from math: "let x equal 5." Delete lines trying to change constants.

**Printing values** helps you see what's inside your variables:

```swift
var playerName = "Roy"
print(playerName)
playerName = "Dani"
print(playerName)
playerName = "Sam"
print(playerName)
```

Run code in Xcode with the blue play button; you can run up to any line by moving the play icon.

**Naming conventions**: Swift uses **camelCase**. Be consistent—`playerName` ≠ `playername`:

```swift
let managerName = "Michael Scott"
let dogBreed = "Samoyed"
let meaningOfLife = "How many roads must a man walk down?"
```

Prefer constants over variables—they're safer and let Swift optimize your code.

### Common Mistakes

```swift
// WRONG: Redeclaring a variable
var name = "Ted"
var name = "Rebecca"  // Error: redeclaring variable

// CORRECT: Changing a variable's value
var name = "Ted"
name = "Rebecca"      // No 'var' needed

// WRONG: Trying to change a constant
let character = "Daphne"
character = "Eloise"  // Error: cannot assign to constant

// WRONG: Inconsistent naming
let playername = "Roy"
print(playerName)     // Error: playerName not found (case matters!)
```

### Key Takeaway

Use `let` for values that won't change, `var` for values that will. Swift encourages constants for safety and optimization. Always use camelCase for naming.

### Optional: Why does Swift have variables?

Variables store temporary information and are central to almost every Swift program. Your app will take some data—user input, downloaded content, or device info—transform it, and show it to the user. The transformation is where your app's "magic" happens, but storing data in memory is what `var` handles.

Create a variable with `var`:

```swift
var favoriteShow = "Orange is the New Black"
favoriteShow = "The Good Place"
favoriteShow = "Doctor Who"
```

- First line: **create a new variable** called `favoriteShow` and assign a value.
- Lines 2–3: modify the existing variable; **don't use `var` again**.

Using `var` repeatedly for the same name will cause an error. Swift enforces clarity: are you creating a new variable, or modifying an existing one?

Although variables are fundamental, sometimes it's better to avoid them—more on that later.

### Optional: Why does Swift have constants as well as variables?

Constants are like variables, but their value **cannot change once set**. Use `let` to create a constant:

```swift
let character = "Daphne"
```

Swift encourages constants whenever a value doesn't need to change. Unlike variables, constants prevent accidental changes—once set, the value is locked in. This gives you more control and helps avoid bugs, ensuring important data stays consistent.

Using a constant is like making a contract with Swift: "this value matters, don't let it change."

---
## How to create strings

Swift uses **strings** to store text. Strings go inside **double quotes**:

```swift
let actor = "Denzel Washington"
let filename = "paris.jpg"
let result = "You win!"
```

You can include quotation marks inside a string by escaping them with a backslash (`\`):

```swift
let quote = "Then he tapped a sign saying \"Believe\" and walked away."
```

**Multi-line strings** use triple quotes:

```swift
let movie = """
A day in
the life of an
Apple engineer
"""
```

The opening and closing `"""` must be on their own lines.

**Counting characters** uses `.count`:

```swift
print(actor.count)

let nameLength = actor.count
print(nameLength)
```

**Modifying text** like making it uppercase uses parentheses because Swift is doing work:

```swift
print(result.uppercased())
```

No parentheses are needed for `.count` because you're just reading data.

**Checking text** can be done with `hasPrefix()` and `hasSuffix()`:

```swift
print(movie.hasPrefix("A day")) // does movie start with "A day"?
print(filename.hasSuffix(".jpg")) // does filename end with ".jpg"?
```

Strings are **case-sensitive**:

```swift
filename.hasSuffix(".JPG") // false
```

Strings are powerful in Swift, and these are the basics you'll use most often.

### Common Mistakes

```swift
// WRONG: Unescaped quotes inside string
let quote = "He said "Hello""  // Error: unexpected code

// CORRECT: Escaped quotes
let quote = "He said \"Hello\""

// WRONG: Multi-line without triple quotes
let text = "Line one
Line two"  // Error: unterminated string

// CORRECT: Multi-line string
let text = """
Line one
Line two
"""

// WRONG: Case sensitivity ignored
let filename = "Paris.JPG"
filename.hasSuffix(".jpg")  // Returns false (case matters!)
```

### Key Takeaway

Strings use double quotes. Escape internal quotes with `\`. Multi-line strings use `"""` on their own lines. Use `.count` to read length, methods with `()` to modify. Strings are case-sensitive.

### Optional: Why does Swift need multi-line strings?

Standard strings can't contain line breaks, making long text ugly in code:

```swift
var quote = "Change the world by being yourself"
```

Multi-line strings (triple quotes) keep text readable across multiple lines:

```swift
var burns = """
The best laid schemes
O' mice and men
Gang aft agley
"""
```

Swift preserves the line breaks as part of the string.

**Note:** Sometimes you need a _long_ string but want it on a _single line_ in your code (not multi-line). This is rare, but useful for error messages—if someone sees an error and searches your code for it, a multi-line string would break their search.

---
## How to store whole numbers

When you're working with whole numbers like 3, 5, 50, or 5 million, you're working with integers, or `Int` for short.

Create integers like strings—use `let` or `var`, provide a name, then give it a value:

```swift
let score = 10
```

Integers can be really big (past quintillions) and really small (negative numbers down to quintillions).

**Making Numbers Readable** Use underscores `_` to break up numbers however you want:

```swift
let reallyBig = 100_000_000
```

Swift doesn't care about the underscores—this also works:

```swift
let reallyBig = 1_00__00___00____00
```

**Arithmetic Operators** Create integers from other integers using `+`, `-`, `*`, and `/`:

```swift
let lowerScore = score - 2
let higherScore = score + 10
let doubledScore = score * 2
let halvedScore = score / 2
```

**Compound Assignment Operators** Instead of `counter = counter + 5`, use shorthand `+=`:

```swift
var counter = 10
counter += 5   // adds 5 directly
counter *= 2   // multiply and assign
counter -= 10  // subtract and assign
counter /= 2   // divide and assign
```

**Useful Functionality** Integers have helpful methods like `isMultiple(of:)`:

```swift
let number = 120
print(number.isMultiple(of: 3))  // true
print(120.isMultiple(of: 3))     // works directly too
```

### Common Mistakes

```swift
// WRONG: Using compound operators on constants
let score = 10
score += 5  // Error: cannot modify constant

// CORRECT: Use var for values that change
var score = 10
score += 5

// Note: Integer division truncates decimals
let result = 10 / 3  // result is 3, not 3.333...
// Use Double for decimal results (see next section)

// WRONG: Underscores in the wrong place
let number = _100_000  // Error: invalid syntax

// CORRECT: Underscores between digits only
let number = 100_000
```

### Key Takeaway

Use `Int` for whole numbers. Underscores make large numbers readable. Use `+`, `-`, `*`, `/` for arithmetic. Use `+=`, `-=`, `*=`, `/=` as shortcuts. Integer division truncates decimals.

---
## How to Store Decimal Numbers

Decimal numbers like 3.1, 5.56, or 3.141592654 are floating-point numbers. The name comes from how computers store them: moving the decimal point around to fit very large and very small numbers in the same space.

This causes decimals to be notoriously problematic:

```swift
let number = 0.1 + 0.2
print(number)  // prints 0.30000000000000004, not 0.3
```

**Creating Decimals** Swift calls them `Double` (double-precision floating-point number). Can store absolutely massive numbers.

**Type Safety** Decimals are wholly different from integers—you can't mix them. Integers are 100% accurate, decimals aren't:

```swift
let a = 1
let b = 2.0
let c = a + b  // Error!
```

Must convert explicitly:

```swift
let c = a + Int(b)      // Double → Int
let c = Double(a) + b   // Int → Double
```

**How Swift Decides** Dot = `Double`. No dot = `Int`:

```swift
let double3 = 3.0  // Double
let int1 = 3       // Int
```

Once Swift decides a type, it can't change:

```swift
var name = "Nicolas Cage"
name = "John Travolta"  // works
name = 57               // Error!
```

**Operators** Same as integers:

```swift
var rating = 5.0
rating *= 2
```

**CGFloat** Older APIs use `CGFloat`. Swift lets you use `Double` everywhere, so ignore `CGFloat`.

**Why Complex?** Computers use binary—can't store numbers like 1/3 exactly, so creates close approximations. That's why Swift won't let us mix `Int` and `Double` by accident!

### Common Mistakes

```swift
// WRONG: Mixing Int and Double
let a = 1
let b = 2.0
let c = a + b  // Error: cannot mix types

// CORRECT: Convert explicitly
let c = Double(a) + b  // or: a + Int(b)

// Note: Floating-point arithmetic isn't exact
let result = 0.1 + 0.2
print(result == 0.3)  // false! (it's 0.30000000000000004)

// WRONG: Changing variable type
var value = 3.14
value = 3  // Error: cannot assign Int to Double

// CORRECT: Keep the type consistent
var value = 3.14
value = 3.0  // both are Double

// WRONG: Forgetting the decimal point
let half = 1 / 2  // result is 0 (integer division!)

// CORRECT: Use Double for decimal results
let half = 1.0 / 2.0  // result is 0.5
```

### Key Takeaway

Use `Double` for decimal numbers. Can't mix `Int` and `Double` without explicit conversion. Swift determines type by presence of decimal point. Floating-point arithmetic isn't perfectly precise. Once a variable's type is set, it can't change.

### Optional: Why does Swift need both Doubles and Integers?

Swift has different number types designed to solve different problems. They can't be mixed because it will lead to problems.

**The Two Types**

- **Integers**: whole numbers (0, 1, -100, 65 million)
- **Doubles**: decimal numbers (0.1, -1.001, 3.141592654)

Swift decides based on the decimal point:

```swift
var myInt = 1       // integer
var myDouble = 1.0  // double
```

**Why Can't We Mix Them?** Both are 1, so why can't we write `var total = myInt + myDouble`?

Swift is playing it safe: your double is a variable—it could change to 1.1 or 3.5. Swift can't guarantee you won't lose the 0.1 or 0.5, so it doesn't allow mixing them. This will annoy you at first, but it's helpful.

### Optional: Why is Swift a Type-Safe Language?

When you create a variable, Swift figures out its type based on the data you assign, and that variable will always keep that type:

```swift
var meaningOfLife = 42  // Swift makes this an integer
```

You can change the value, but not the type:

```swift
meaningOfLife = "Forty two"  // Error! Can't change integer to string
```

**Why This Helps** Type safety prevents data mistakes. As programs grow complex, it's impossible to keep track of all variable types in your head—so Swift does it for you.

---

## Day 1 Summary

**Variables and Constants**

- Use `var` for values that change, `let` for values that don't
- Prefer constants when possible for safety and optimization
- Use camelCase for naming
- Once created, don't redeclare with `var` or `let` again

**Strings**

- Use `"double quotes"` for single-line strings
- Use `"""triple quotes"""` for multi-line strings (opening/closing on own lines)
- Escape internal quotes with backslash: `\"`
- Methods: `.count`, `.uppercased()`, `.hasPrefix()`, `.hasSuffix()`
- Strings are case-sensitive

**Integers (Int)**

- Whole numbers: positive, negative, or zero
- Use underscores for readability: `100_000_000`
- Operators: `+`, `-`, `*`, `/`
- Compound operators: `+=`, `-=`, `*=`, `/=`
- Methods: `.isMultiple(of:)`
- Integer division truncates (drops) decimal values

**Doubles**

- Decimal numbers (floating-point)
- Not perfectly precise (0.1 + 0.2 ≠ 0.3 exactly)
- Cannot mix with Int without explicit conversion
- Swift determines type by decimal point: `3.0` is Double, `3` is Int
- Same operators as Int

**Type Safety**

- Swift determines variable type based on initial value
- Type cannot change once set
- Prevents accidental data type errors
- Must explicitly convert between Int and Double

**Key Principle:** Once Swift decides a variable's type, that type is locked in. This prevents bugs and helps Swift optimize your code.