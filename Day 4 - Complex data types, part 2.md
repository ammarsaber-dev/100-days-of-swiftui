**February 16, 2026** • [Day 3 - Complex data types, part 1](Day%203%20-%20Complex%20data%20types,%20part%201.md) ← → [Day 5 - Conditions](Day%205%20-%20Conditions.md)

Learn about type annotations—Swift's way of letting you explicitly declare what type each variable holds. Then review complex data types and complete Checkpoint 2.

---

## Quick Reference

```swift
// Type annotations - explicit type declaration
let username: String = "Taylor"
let age: Int = 26
let height: Double = 1.78
let isActive: Bool = true

// Arrays
var albums: [String] = ["Red", "Fearless"]
var numbers: [Int] = [1, 2, 3]

// Dictionaries
var user: [String: String] = ["name": "Taylor"]
var scores: [String: Int] = ["Math": 95]

// Sets
var words: Set<String> = Set(["hello", "world"])

// Empty collections (type annotation needed)
var teams: [String] = [String]()
var cities: [String] = []
var clues = [String]()  // type inferred from right side

// Enums
enum UIStyle {
    case light, dark, system
}
var style: UIStyle = .light
```

---

## Type Inference vs Type Annotation

**Type inference** (Swift figures it out):

```swift
let surname = "Lasso"  // Swift infers String
var score = 0          // Swift infers Int
```

**Type annotation** (you tell Swift explicitly):

```swift
let surname: String = "Lasso"
var score: Int = 0
```

### Why Use Type Annotations?

Sometimes you want to override Swift's choice:

```swift
var score: Double = 0  // Without :Double, Swift would infer Int
```

Now `score` is a decimal (0.0) instead of a whole number.

### Type Annotation for All Types

```swift
// String - holds text
let playerName: String = "Roy"

// Int - holds whole numbers
var luckyNumber: Int = 13

// Double - holds decimal numbers
let pi: Double = 3.141

// Bool - holds true or false
var isAuthenticated: Bool = true

// Array - holds multiple values in order
var albums: [String] = ["Red", "Fearless"]

// Dictionary - holds key-value pairs
var user: [String: String] = ["id": "@twostraws"]

// Set - holds unique values
var books: Set<String> = Set(["The Bluest Eye", "Foundation"])
```

### Creating Empty Collections

Type annotation is needed when creating empty collections:

```swift
var teams: [String] = [String]()  // full syntax
var cities: [String] = []         // shorter syntax
var clues = [String]()            // type inferred from right side
```

Add `()` when making empty arrays, dictionaries, and sets—that's where Swift lets you customize creation.

**Why create empty collections?** When you don't know all your data up front and need to build it dynamically:

```swift
let names = ["Eleanor", "Chidi", "Tahani", "Jianyu", "Michael", "Janet"]
var jNames = [String]()  // Start empty

// Check each name and add if it starts with "J"
for name in names {
    if name.hasPrefix("J") {
        jNames.append(name)
    }
}
// Result: ["Jianyu", "Janet"]
```

### Enums with Type Annotations

```swift
enum UIStyle {
    case light, dark, system
}

var style = UIStyle.light
style = .dark  // Swift knows it must be UIStyle
```

Values of an enum have the same type as the enum itself.

### Constants Without Initial Values

You can create a constant without a value, then assign it later (only once):

```swift
let username: String
// lots of complex logic
username = "@twostraws"
// lots more complex logic
print(username)
```

Swift ensures:

- You don't use `username` before it has a value
- You only set the value once (it remains constant)

This requires type annotation because Swift needs to know what type `username` will contain.

### Common Mistakes

```swift
// ❌ Type annotation can't convert incompatible types
let score: Int = "Zero"  // Error: can't convert String to Int

// ❌ Using value before assignment
let username: String
print(username)  // Error: constant used before being initialized

// ✅ Assign before use
let username: String
username = "@twostraws"
print(username)

// ❌ Assigning twice to constant
let name: String
name = "Taylor"
name = "Swift"  // Error: cannot assign to value: 'name' is a 'let' constant
```

**Key Insights:**

- **Type inference**: Swift figures out the type from the value you assign
- **Type annotation**: You explicitly tell Swift the type with a colon (`:`)
- **Three reasons to use type annotations**: (1) Swift can't figure out the type, (2) You want a different type from Swift's default, (3) You don't want to assign a value yet
- **Type safety**: Swift must always know what type each constant/variable holds—prevents errors like `5 + true`
- **Personal preference**: Use type inference as much as possible—cleaner and easier to read. Only use annotations when needed
- Empty collections require type annotation so Swift knows what they'll contain
- Constants can be declared without values but must be assigned exactly once before use

---

## Summary: Complex Data Types

### Arrays

- Store lots of values in one place, accessed by integer index (0, 1, 2...)
- Syntax: `[String]`, `[Int]`, etc.
- Methods: `count`, `append()`, `contains()`, `remove(at:)`, `sorted()`
- Most commonly used collection type

```swift
var albums = ["Red", "Fearless", "1989"]
print(albums.count)  // 3
albums.append("Reputation")
print(albums.contains("Red"))  // true
```

### Dictionaries

- Store lots of values accessed by keys you specify
- Syntax: `[String: Int]`, `[String: String]`, etc.
- Methods: `contains()`, `count`, `removeAll()`
- Return optionals when reading keys (key might not exist)

```swift
var user = ["name": "Taylor", "job": "Singer"]
print(user["name"])  // Optional("Taylor")
print(user["address"])  // nil (doesn't exist)
```

### Sets

- Store lots of values without order
- Extremely efficient at finding if they contain a specific item
- Automatically remove duplicates
- Syntax: `Set<String>`, `Set<Int>`, etc.
- Methods: `contains()`, `count`, `insert()`

```swift
var numbers = Set([1, 2, 3, 2, 1])
print(numbers)  // [1, 2, 3] - duplicates removed
print(numbers.contains(2))  // true - very fast
```

### Enums

- Create your own simple types with a fixed range of acceptable values
- Type-safe and efficient
- Use when you have a known set of related values

```swift
enum Weather {
    case sun, rain, wind, snow
}
let today = Weather.sun
```

**Key Insights:**

- **Usage frequency**: Arrays (most common) → Dictionaries → Sets → Enums
- Arrays maintain order, sets don't but are faster for membership checks
- Dictionaries return optionals because keys might not exist
- Sets automatically handle uniqueness—perfect for removing duplicates
- Enums prevent invalid values—you can't accidentally use a string that doesn't match

---

## Checkpoint 2

Create an array of strings, then write code that prints:

1. The number of items in the array
2. The number of unique items in the array

### Hints

1. Start by creating an array of strings: `let albums = ["Red", "Fearless", "Red"]`
2. Read the number of items using `albums.count`
3. `count` also exists for sets
4. Sets can be made from arrays: `Set(someArray)`
5. Sets never include duplicates

### Why This Matters

This tests whether you understand arrays and sets, and how they differ. The key insight: converting an array to a set automatically removes duplicates.

**Don't look at solutions immediately.** Struggling is part of learning—spend time thinking about it first!

---

**Topics:** [Type Annotations](https://www.hackingwithswift.com/quick-start/beginners/how-to-use-type-annotations) • [Complex Data Summary](https://www.hackingwithswift.com/quick-start/beginners/summary-complex-data) • [Checkpoint 2](https://www.hackingwithswift.com/quick-start/beginners/checkpoint-2)