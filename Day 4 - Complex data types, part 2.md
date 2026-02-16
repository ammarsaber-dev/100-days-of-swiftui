**Date: February 16, 2026**

> **Quick Summary:** Today you'll finish learning about data types by exploring type annotations—Swift's way of letting you explicitly declare what type each variable holds. Then you'll test your knowledge with Checkpoint 2. As Eric Raymond said, "good data structures and bad code works a lot better than the other way around."

---

## Quick Reference

```swift
// Type annotations - explicit type declaration
let username: String = "Taylor"
let age: Int = 26
let height: Double = 1.78
let isActive: Bool = true

// Arrays with type annotation
var albums: [String] = ["Red", "Fearless"]
var numbers: [Int] = [1, 2, 3]

// Dictionaries with type annotation
var user: [String: String] = ["name": "Taylor"]
var scores: [String: Int] = ["Math": 95]

// Sets with type annotation
var words: Set<String> = Set(["hello", "world"])

// Empty collections (type annotation needed)
var teams: [String] = [String]()
var cities: [String] = []
var clues = [String]()  // type inferred from right side
```

---

## Today's Mission

Today we're going to finish our look at data types by examining type annotation, which is Swift's way of letting us dictate exactly what data type each variable and constant should be. Once that's done we'll summarize what's covered, then try another checkpoint so you can evaluate what you've learned so far.

I know, you're probably sick of data types at this point, but as Eric Raymond said, "good data structures and bad code works a lot better than the other way around."

Today ought to take you a lot less than an hour to complete, but that's okay – day 13 will probably take more than an hour, so it all balances out!

### Day 4 Topics

1. How to use type annotations
    - Optional: Why does Swift have type annotations?
    - Optional: Why would you want to create an empty collection?
    - Test: Type annotations
2. Summary: Complex data
3. Checkpoint 2

**Tip:** Even though the checkpoint doesn't ask much, don't be surprised if you spend some time staring at the screen wondering what to do. That isn't a bad sign – in fact, I'd say it's a good sign. I firmly believe there is no learning without struggle, so don't be afraid to fight for it!

---

## 1. How to Use Type Annotations

Swift can figure out what type of data a constant or variable holds based on what you assign to it (**type inference**). But sometimes you want to be explicit about the type, and that's where **type annotations** come in.

### Type Inference vs Type Annotation

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

Now `score` is a decimal (0.0) instead of a whole number, even though we assigned 0.

### Type Annotation Reference

```swift
// String - holds text
let playerName: String = "Roy"

// Int - holds whole numbers
var luckyNumber: Int = 13

// Double - holds decimal numbers
let pi: Double = 3.141

// Bool - holds true or false
var isAuthenticated: Bool = true

// Array - holds multiple values in order (must specify element type)
var albums: [String] = ["Red", "Fearless"]

// Dictionary - holds key-value pairs (must specify key and value types)
var user: [String: String] = ["id": "@twostraws"]

// Set - holds unique values in optimized order (must specify element type)
var books: Set<String> = Set(["The Bluest Eye", "Foundation"])
```

### Creating Empty Collections

Type annotation is needed when creating empty collections:

```swift
var teams: [String] = [String]()  // full syntax
var cities: [String] = []         // some prefer this
var clues = [String]()            // type inferred from right side
```

**Important:** Add `()` when making empty arrays, dictionaries, and sets—that's where Swift lets you customize creation.

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

### When to Use Type Annotations

**Three main reasons:**

1. Swift can't figure out the type
2. You want a different type from Swift's default
3. You don't want to assign a value yet

**Personal preference:** Use type inference as much as possible. Let Swift figure it out by assigning values. Only use annotations when needed.

### The Golden Rule

**Swift must always know what data types your constants and variables contain.** This is type safety—it prevents nonsense like `5 + true`.

### Common Mistakes

```swift
// WRONG: Type annotation can't convert incompatible types
let score: Int = "Zero"  // Error: can't convert String to Int

// WRONG: Using value before assignment
let username: String
print(username)  // Error: constant used before being initialized

// CORRECT: Assign before use
let username: String
username = "@twostraws"
print(username)

// WRONG: Assigning twice to constant
let name: String
name = "Taylor"
name = "Swift"  // Error: cannot assign to value: 'name' is a 'let' constant
```

**Key Takeaway:** Type annotations let you explicitly declare types. Use them when Swift can't infer the type, when you want a different type, or when creating values later. Swift must always know what type each variable holds (type safety).

### Optional: Why Does Swift Have Type Annotations?

Type annotations exist for three main reasons:

**1. Swift can't figure out the type** Example: Loading data from the internet—Swift doesn't know what type it is ahead of time.

**2. You want a different type from Swift's default**

```swift
var percentage: Double = 99  // Want Double, not Int
```

**3. You don't want to assign a value yet**

```swift
var name: String  // Declare now, assign later
```

**When to use them?** This is subjective and depends on personal style.

**Preference for type inference:**

- Makes code shorter and easier to read
- Lets you change types by changing initial values

**Preference for explicit annotations:**

- Makes types immediately clear
- More verbose but explicit

Both approaches work fine—it's a question of style.

### Optional: Why Would You Want to Create an Empty Collection?

Sometimes you don't know all your data up front.

Example with fixed data:

```swift
let names = ["Eleanor", "Chidi", "Tahani", "Jianyu", "Michael", "Janet"]
```

But what if you need to calculate your data? You'd:

1. Create an empty array: `var jNames = [String]()`
2. Check each name in `names` to see if it starts with "J"
3. Add matching names to `jNames`

Result: `jNames` would contain `["Jianyu", "Janet"]`

If no names matched (like checking for "X"), the array stays empty—and that's fine. It started empty and ended empty.

Empty collections are essential when building data dynamically based on calculations or conditions.

---

## 2. Summary: Complex Data

We've gone beyond simple data types and started looking at ways to group them together and create our own using enums.

**Arrays**

- Store lots of values in one place
- Read values using integer indices (0, 1, 2...)
- Must specify element type: `[String]`, `[Int]`, etc.
- Methods: `count`, `append()`, `contains()`, `remove(at:)`, `sorted()`

**Dictionaries**

- Store lots of values in one place
- Read values using keys you specify
- Must specify key and value types: `[String: Int]`, etc.
- Methods: `contains()`, `count`, `removeAll()`
- Return optionals when reading keys (might not exist)

**Sets**

- Store lots of values, but don't control the order
- Extremely efficient at finding if they contain a specific item
- Automatically remove duplicates
- Methods: `contains()`, `count`, `insert()`

**Enums**

- Create your own simple types
- Specify a range of acceptable values
- Examples: user actions, file types, notification types
- Type-safe and efficient

**Type Safety**

- Swift must always know the type of data in constants/variables
- Uses type inference to figure it out based on assigned data
- Can use type annotation to force a particular type

**Usage frequency:** Arrays are used most, then dictionaries, then sets. Sets are useful but less common—you'll know when you need them.

---

## 3. Checkpoint 2

Time to solve a small coding challenge. It's not designed to trip you up, but to give you a chance to think about what you've learned.

### The Challenge

Create an array of strings, then write code that prints:

1. The number of items in the array
2. The number of unique items in the array

### Why This Matters

This checkpoint tests whether you understand arrays and sets, and how they differ. Forgetting what you've learned then re-learning it actually makes it sink in deeper—so think about your solution before reading the hints!

### Hints (Use These If You Get Stuck)

1. Start by creating an array of strings: `let albums = ["Red", "Fearless"]`
2. Read the number of items using `albums.count`
3. `count` also exists for sets
4. Sets can be made from arrays: `Set(someArray)`
5. Sets never include duplicates

### Tips for Solving

- Think about what happens when you convert an array with duplicates to a set
- You'll need to print two different counts: one for the array, one for the set
- Use string interpolation to make your output clear

Don't be surprised if you spend time staring at the screen wondering what to do. That's not a bad sign—it's actually a good sign. There is no learning without struggle, so don't be afraid to fight for it!

Try building it yourself before looking at any solution.

---

## Where Now?

You've completed Day 4 and finished learning about complex data types. You now understand arrays, dictionaries, sets, enums, and type annotations—the building blocks for organizing data in Swift.

More importantly, you've tackled Checkpoint 2, which tests your ability to combine different concepts to solve a problem. This kind of problem-solving is what programming is really about.

**Tomorrow:** You'll start learning about conditions and loops—how to make your code make decisions and repeat actions. This is where your programs start to feel truly dynamic.

Keep going. The fundamentals are almost complete, and soon you'll be building real functionality.

---

**Previous:** [Day 3 - Complex data types, part 1](Day%203%20-%20Complex%20data%20types,%20part%201.md) | **Next:** Day 5 - Conditions and loops# Day 4: Complex Data Types, Part 2**Date: February 15, 2026**

> **Quick Summary:** Today you'll finish learning about data types by exploring type annotations—Swift's way of letting you explicitly declare what type each variable holds. Then you'll test your knowledge with Checkpoint 2. As Eric Raymond said, "good data structures and bad code works a lot better than the other way around."

---

## Quick Reference

```swift
// Type annotations - explicit type declaration
let username: String = "Taylor"
let age: Int = 26
let height: Double = 1.78
let isActive: Bool = true

// Arrays with type annotation
var albums: [String] = ["Red", "Fearless"]
var numbers: [Int] = [1, 2, 3]

// Dictionaries with type annotation
var user: [String: String] = ["name": "Taylor"]
var scores: [String: Int] = ["Math": 95]

// Sets with type annotation
var words: Set<String> = Set(["hello", "world"])

// Empty collections (type annotation required)
var teams: [String] = []
var cities: [String] = [String]()
var clues = [String]()  // type inferred from annotation
```

---

## Today's Mission

Today we're going to finish our look at data types by examining type annotation, which is Swift's way of letting us dictate exactly what data type each variable and constant should be. Once that's done we'll summarize what's covered, then try another checkpoint so you can evaluate what you've learned so far.

I know, you're probably sick of data types at this point, but as Eric Raymond said, "good data structures and bad code works a lot better than the other way around."

Today ought to take you a lot less than an hour to complete, but that's okay – day 13 will probably take more than an hour, so it all balances out!

### Day 4 Topics

1. How to use type annotations
    - Optional: Why does Swift have type annotations?
    - Optional: Why would you want to create an empty collection?
    - Test: Type annotations
2. Summary: Complex data
3. Checkpoint 2

**Tip:** Even though the checkpoint doesn't ask much, don't be surprised if you spend some time staring at the screen wondering what to do. That isn't a bad sign – in fact, I'd say it's a good sign. I firmly believe there is no learning without struggle, so don't be afraid to fight for it!

---

## 1. How to Use Type Annotations

Swift can figure out what type of data a constant or variable holds based on what you assign to it (**type inference**). But sometimes you want to be explicit about the type, and that's where **type annotations** come in.

### Type Inference vs Type Annotation

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

Now `score` is a decimal (0.0) instead of a whole number, even though we assigned 0.

### Type Annotation Reference

```swift
// String - holds text
let playerName: String = "Roy"

// Int - holds whole numbers
var luckyNumber: Int = 13

// Double - holds decimal numbers
let pi: Double = 3.141

// Bool - holds true or false
var isAuthenticated: Bool = true

// Array - holds multiple values in order (must be specialized)
var albums: [String] = ["Red", "Fearless"]

// Dictionary - holds key-value pairs (must be specialized)
var user: [String: String] = ["id": "@twostraws"]

// Set - holds unique values in optimized order (must be specialized)
var books: Set<String> = Set(["The Bluest Eye", "Foundation"])
```

### Creating Empty Collections

Type annotation is required when creating empty collections:

```swift
var teams: [String] = [String]()  // explicit
var teams: [String] = []          // some prefer this
var clues = [String]()            // type inference from annotation
```

**Important:** Add `()` when making empty arrays, dictionaries, and sets—that's where Swift lets you customize creation.

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

You can create a constant without a value, then assign it later (once only):

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

### When to Use Type Annotations

**Most common uses:**

1. Swift can't figure out the type
2. You want a different type from Swift's default
3. You don't want to assign a value yet

**Personal preference:** Use type inference as much as possible. Let Swift figure it out by assigning values. Only use annotations when needed.

### The Golden Rule

**Swift must always know what data types your constants and variables contain.** This is type safety—it prevents nonsense like `5 + true`.

### Common Mistakes

```swift
// WRONG: Type annotation can't convert types
let score: Int = "Zero"  // Error: can't convert String to Int

// WRONG: Using value before assignment
let username: String
print(username)  // Error: constant used before being initialized

// CORRECT: Assign before use
let username: String
username = "@twostraws"
print(username)

// Note: Can only assign once to constants
let name: String
name = "Taylor"
name = "Swift"  // Error: cannot assign twice to constant
```

**Key Takeaway:** Type annotations let you explicitly declare types. Use them when Swift can't infer the type, when you want a different type, or when creating values later. Swift must always know what type each variable holds (type safety).

### Optional: Why Does Swift Have Type Annotations?

Type annotations exist for three main reasons:

**1. Swift can't figure out the type** Example: Loading data from the internet—Swift doesn't know what type it is ahead of time.

**2. You want a different type from Swift's default**

```swift
var percentage: Double = 99  // Want Double, not Int
```

**3. You don't want to assign a value yet**

```swift
var name: String  // Declare now, assign later
```

**When to use them?** This is subjective and depends on personal style.

**Preference for type inference:**

- Makes code shorter and easier to read
- Lets you change types by changing initial values

**Preference for explicit annotations:**

- Makes types immediately clear
- More verbose but explicit

Both approaches work fine—it's a question of style.

### Optional: Why Would You Want to Create an Empty Collection?

Sometimes you don't know all your data up front.

Example with fixed data:

```swift
let names = ["Eleanor", "Chidi", "Tahani", "Jianyu", "Michael", "Janet"]
```

But what if you need to calculate your data? You'd:

1. Create an empty array: `var jNames = [String]()`
2. Check each name in `names` to see if it starts with "J"
3. Add matching names to `jNames`

Result: `jNames` would contain `["Jianyu", "Janet"]`

If no names matched (like checking for "X"), the array stays empty—and that's fine. It started empty and ended empty.

Empty collections are essential when building data dynamically based on calculations or conditions.

---

## 2. Summary: Complex Data

We've gone beyond simple data types and started looking at ways to group them together and create our own using enums.

**Arrays**

- Store lots of values in one place
- Read values using integer indices (0, 1, 2...)
- Must be specialized (one specific type)
- Methods: `count`, `append()`, `contains()`, `remove()`, `sorted()`

**Dictionaries**

- Store lots of values in one place
- Read values using keys you specify
- Must be specialized (one type for keys, one for values)
- Methods: `contains()`, `count`, `removeAll()`
- Return optionals when reading keys

**Sets**

- Store lots of values, but don't control the order
- Really efficient at finding if they contain a specific item
- Automatically remove duplicates
- Methods: `contains()`, `count`, `insert()`

**Enums**

- Create your own simple types
- Specify a range of acceptable values
- Examples: user actions, file types, notification types
- Type-safe and efficient

**Type Safety**

- Swift must always know the type of data in constants/variables
- Uses type inference to figure it out based on assigned data
- Can use type annotation to force a particular type

**Usage frequency:** Arrays are used most, then dictionaries, then sets. Sets are useful but less common—you'll know when you need them.

---

## 3. Checkpoint 2

Time to solve a small coding challenge. It's not designed to trip you up, but to give you a chance to think about what you've learned.

### The Challenge

Create an array of strings, then write code that prints:

1. The number of items in the array
2. The number of unique items in the array

### Why This Matters

This checkpoint tests whether you understand arrays and sets, and how they differ. Forgetting what you've learned then re-learning it actually makes it sink in deeper—so think about your solution before reading the hints!

### Hints (Use These If You Get Stuck)

1. Start by creating an array of strings: `let albums = ["Red", "Fearless"]`
2. Read the number of items using `albums.count`
3. `count` also exists for sets
4. Sets can be made from arrays: `Set(someArray)`
5. Sets never include duplicates

### Tips for Solving

- Think about what happens when you convert an array with duplicates to a set
- You'll need to print two different counts: one for the array, one for the set
- Use string interpolation to make your output clear

Don't be surprised if you spend time staring at the screen wondering what to do. That's not a bad sign—it's actually a good sign. There is no learning without struggle, so don't be afraid to fight for it!

Try building it yourself before looking at any solution.

---

## Where Now?

You've completed Day 4 and finished learning about complex data types. You now understand arrays, dictionaries, sets, enums, and type annotations—the building blocks for organizing data in Swift.

More importantly, you've tackled Checkpoint 2, which tests your ability to combine different concepts to solve a problem. This kind of problem-solving is what programming is really about.

**Tomorrow:** You'll start learning about conditions and loops—how to make your code make decisions and repeat actions. This is where your programs start to feel truly dynamic.

Keep going. The fundamentals are almost complete, and soon you'll be building real functionality.

---

**Previous:** [Day 3 - Complex data types, part 1](Day%203%20-%20Complex%20data%20types,%20part%201.md) | **Next:** [Day 5 - Conditions](Day%205%20-%20Conditions.md)
