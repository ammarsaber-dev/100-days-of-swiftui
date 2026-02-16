**Date: February 15, 2026**

> **Quick Summary:** Today you'll learn about arrays, dictionaries, sets, and enums—ways to group multiple pieces of data together. These are the building blocks for storing collections of information in your apps. Choose the wrong one while learning and nothing bad happens—just experiment and see what works.

---

## Quick Reference

```swift
// Arrays - ordered collections
let colors = ["Red", "Green", "Blue"]
colors[0]                       // access by index
colors.count                    // number of items
colors.append("Yellow")         // add item (needs var)
colors.contains("Red")          // check if exists
colors.sorted()                 // returns sorted array

// Dictionaries - key-value pairs
let user = ["name": "Taylor", "age": "26"]
user["name"]                    // returns Optional
user["job", default: "Unknown"] // with default value
var heights = [String: Int]()   // empty dictionary

// Sets - unique, unordered collections
let numbers = Set([1, 2, 3, 3]) // duplicate removed
numbers.contains(2)             // extremely fast lookup
numbers.insert(4)               // add item

// Enums - define your own types
enum Weekday {
    case monday, tuesday, wednesday
}
var day = Weekday.monday
day = .tuesday                  // shorthand after first use
```

---

## Today's Mission

Today we're looking at complex data types that group data together. Understanding the differences between them, and knowing which to use when, can sometimes trip folks up when learning – as Joseph Campbell once said, "computers are like Old Testament gods: lots of rules and no mercy."

Don't worry: the types we're learning today cover the overwhelming majority of requirements, and if you choose the wrong one while learning, nothing bad will happen.

**Common beginner mistake:** Fear of running code. Trust me – none of the code here can break your computer. Make a change and run it. Make another change and run that. Experiment until you feel comfortable – it all helps.

### Day 3 Topics

1. [How to store ordered data in arrays](https://www.hackingwithswift.com/quick-start/beginners/how-to-store-ordered-data-in-arrays)
    - Optional: [Why does Swift have arrays?](https://www.hackingwithswift.com/quick-start/understanding-swift/why-does-swift-have-arrays)
    - Test: [Arrays](https://www.hackingwithswift.com/review/sixty/arrays)
2. [How to store and find data in dictionaries](https://www.hackingwithswift.com/quick-start/beginners/how-to-store-and-find-data-in-dictionaries)
    - Optional: [Why does Swift have dictionaries as well as arrays?](https://www.hackingwithswift.com/quick-start/understanding-swift/why-does-swift-have-dictionaries-as-well-as-arrays)
    - Optional: [Why does Swift have default values for dictionaries?](https://www.hackingwithswift.com/quick-start/understanding-swift/why-does-swift-have-default-values-for-dictionaries)
    - Test: [Dictionaries](https://www.hackingwithswift.com/review/sixty/dictionaries)
    - Test: [Dictionary default values](https://www.hackingwithswift.com/review/sixty/dictionary-default-values)
3. [How to use sets for fast data lookup](https://www.hackingwithswift.com/quick-start/beginners/how-to-use-sets-for-fast-data-lookup)
    - Optional: [Why are sets different from arrays in Swift?](https://www.hackingwithswift.com/quick-start/understanding-swift/why-are-sets-different-from-arrays-in-swift)
    - Test: [Sets](https://www.hackingwithswift.com/review/sixty/sets)
4. [How to create and use enums](https://www.hackingwithswift.com/quick-start/beginners/how-to-create-and-use-enums)
    - Optional: [Why does Swift need enums?](https://www.hackingwithswift.com/quick-start/understanding-swift/why-does-swift-need-enums)
    - Test: [Enumerations](https://www.hackingwithswift.com/review/sixty/enumerations)

---

## 1. How to Store Ordered Data in Arrays

**Arrays** group multiple values of the same type in order. They can hold zero to millions of items and automatically adapt to any size.

### Creating and Reading Arrays

```swift
var beatles = ["John", "Paul", "George", "Ringo"]
let numbers = [4, 8, 15, 16, 23, 42]
var temperatures = [25.3, 28.2, 26.4]
```

Arrays use square brackets with commas between items.

**Reading by index** (starts at 0):

```swift
print(beatles[0])      // "John" (first item)
print(numbers[1])      // 8 (second item)
print(temperatures[2]) // 26.4 (third item)
```

**Important:** Accessing an index that doesn't exist crashes your app.

### Modifying Arrays

```swift
beatles.append("Adrian")
beatles.append("Allen")  // can add duplicates
```

### Type Safety

Arrays only hold one type:

```swift
temperatures.append("Chris")  // Error: can't mix String with Double

let firstBeatle = beatles[0]  // String
let firstNumber = numbers[0]  // Int
let notAllowed = firstBeatle + firstNumber  // Error: can't mix types
```

### Creating Empty Arrays

```swift
var scores = Array<Int>()
scores.append(100)

// Shorthand (preferred):
var albums = [String]()
albums.append("Folklore")

// Swift infers type from initial values:
var albums = ["Folklore"]  // Swift knows it's [String]
albums.append("Fearless")
```

### Useful Array Methods

```swift
print(albums.count)  // count items

// Remove items
var characters = ["Lana", "Pam", "Ray", "Sterling"]
characters.remove(at: 2)  // removes "Ray"
characters.removeAll()

// Check if contains
let bondMovies = ["Casino Royale", "Spectre", "No Time To Die"]
print(bondMovies.contains("Frozen"))  // false

// Sort (returns new array, original unchanged)
let cities = ["London", "Tokyo", "Rome", "Budapest"]
print(cities.sorted())

// Reverse (doesn't rearrange, just remembers)
let presidents = ["Bush", "Obama", "Trump", "Biden"]
let reversedPresidents = presidents.reversed()
```

### Common Mistakes

```swift
// WRONG: Index out of range
let names = ["Anna", "Alex", "Brian"]
print(names[10])  // Crash!

// WRONG: Appending to constant
let scores = [100, 90, 85]
scores.append(95)  // Error

// CORRECT:
var scores = [100, 90, 85]
scores.append(95)
```

**Key Takeaway:** Arrays store ordered collections of the same type. Index starts at 0. Use `var` for mutable arrays. Methods: `append()`, `remove()`, `contains()`, `sorted()`, `count`.

### Optional: Why Does Swift Have Arrays?

Swift's simple types (String, Int, Bool, Double) store single values. Arrays store many values together.

Useful for weekday names, temperature forecasts, high scores, or any collection of related data.

Arrays can be any size and grow/shrink as needed (if variable). Items accessed by position starting from 0 (**zero-based indexing**).

Swift crashes if you access an invalid index rather than returning wrong data—better to crash than silently fail.

---

## 2. How to Store and Find Data in Dictionaries

Arrays are great for ordered data, but accessing by position can be dangerous. **Dictionaries** store data using meaningful keys instead of positions.

### The Problem with Arrays

```swift
var employee = ["Taylor Swift", "Singer", "Nashville"]
print("Name: \(employee[0])")
print("Job title: \(employee[1])")
print("Location: \(employee[2])")
```

Problems: Can't be sure what each position means, no guarantee an index exists, easy to break if items are removed.

### Creating Dictionaries

Use **key-value pairs**:

```swift
let employee2 = [
    "name": "Taylor Swift",
    "job": "Singer", 
    "location": "Nashville"
]
```

Keys are on the left, values on the right. Much clearer than array indices.

### Reading from Dictionaries

```swift
print(employee2["name"])
print(employee2["job"])
print(employee2["location"])
```

### Optionals and Default Values

Reading a non-existent key returns an **optional** (might be nil):

```swift
print(employee2["password"])  // key doesn't exist!
```

Swift doesn't crash like arrays—it returns an optional.

**Solution:** Provide a default value:

```swift
print(employee2["name", default: "Unknown"])
print(employee2["job", default: "Unknown"])
```

### Different Data Types

```swift
// String keys, Boolean values
let hasGraduated = [
    "Eric": false,
    "Maeve": true,
    "Otis": false,
]

// Int keys, String values
let olympics = [
    2012: "London",
    2016: "Rio de Janeiro",
    2021: "Tokyo"
]
print(olympics[2012, default: "Unknown"])
```

### Creating Empty Dictionaries

```swift
var heights = [String: Int]()
heights["Yao Ming"] = 229
heights["Shaquille O'Neal"] = 216
```

Syntax: `[KeyType: ValueType]()`

### Overwriting Values

Dictionaries don't allow duplicate keys. Setting a value for an existing key overwrites it:

```swift
var archEnemies = [String: String]()
archEnemies["Batman"] = "The Joker"
archEnemies["Batman"] = "Penguin"  // overwrites "The Joker"
```

### Common Mistakes

```swift
// WRONG: Reading without default returns Optional
let name = employee2["name"]  // Optional("Taylor Swift")

// CORRECT: Use default value
let name = employee2["name", default: "Unknown"]

// Empty dictionary needs type
var data = [:]  // Error
var data = [String: Int]()  // Correct
```

**Key Takeaway:** Dictionaries store data with meaningful keys. Reading non-existent keys returns optionals. Use `default:`for fallback values. Keys must be unique—setting existing keys overwrites values.

### Optional: Why Does Swift Have Dictionaries as Well as Arrays?

- **Arrays:** Sequential by index (0, 1, 2...)
- **Dictionaries:** Meaningful keys you choose

Dictionaries are more convenient (`user["country"]` vs remembering "index 7") and optimize for fast retrieval—instant even with 100,000 items.

**Caveat:** Keys might not exist, so you get optionals.

### Optional: Why Does Swift Have Default Values for Dictionaries?

Reading from dictionaries might return nil if keys don't exist. Default values provide backups:

```swift
let results = ["english": 100, "french": 85, "geography": 75]
let historyResult = results["history", default: 0]  // returns 0
```

**When to use:**

- Missing value = failure/zero → use default (always get a value)
- Missing value = hasn't happened yet → don't use default (check for nil)

---

## 3. How to Use Sets for Fast Data Lookup

**Sets** are like arrays but:

- Can't add duplicates
- Don't store items in order

### Creating Sets

```swift
let people = Set(["Denzel Washington", "Tom Cruise", "Nicolas Cage", "Samuel L Jackson"])
print(people)  // order might differ
```

Create an array first, then put it into the set. Set removes duplicates and doesn't maintain order.

### Adding to Sets

Use `insert()` (not `append()`):

```swift
var people = Set<String>()
people.insert("Denzel Washington")
people.insert("Tom Cruise")
```

### Why Use Sets?

**1. No duplicates** Perfect for uniqueness (like Screen Actors Guild requiring unique stage names).

**2. Extremely fast lookups**

Example: Checking if array contains "The Dark Knight":

- Array of 1,000: might check all 1,000
- Set of 1,000,000: returns instantly

Even with 10 million items, `contains()` on sets runs instantly.

### Useful Set Methods

```swift
people.contains("Tom Cruise")  // extremely fast
people.count
people.sorted()  // returns sorted array
```

### Common Mistakes

```swift
// WRONG: Using append()
var names = Set<String>()
names.append("John")  // Error

// CORRECT:
names.insert("John")

// Sets ignore duplicates
var numbers = Set([1, 2, 2, 3, 3, 3])
print(numbers)  // {1, 2, 3}

// WRONG: Expecting indices
let set = Set(["A", "B", "C"])
print(set[0])  // Error: sets don't have indices
```

**Key Takeaway:** Sets store unique, unordered items with extremely fast lookups. Use `insert()` to add. Perfect for "does this exist?" questions or when duplicates aren't allowed.

### Optional: Why Are Sets Different from Arrays in Swift?

- **Sets:** Unordered, no duplicates
- **Arrays:** Ordered, allows duplicates

Sets optimize for fast retrieval. Checking "does this exist?" returns instantly regardless of size.

Arrays must check items sequentially—potentially all 10,000 items.

**Use sets for:** Checking existence, ensuring uniqueness, fast lookups **Use arrays for:** Order matters, duplicates needed, most cases

---

## 4. How to Create and Use Enums

An **enum** (enumeration) is a set of named values. More efficient and safer than strings.

### The Problem with Strings

```swift
var selected = "Monday"
selected = "January"  // Wrong—but Swift allows it
selected = "Friday "  // Extra space—different from "Friday"
```

Requires careful programming and is inefficient.

### Creating Enums

```swift
enum Weekday {
    case monday, tuesday, wednesday, thursday, friday
}
```

Only these five values allowed—no typos, no accidents.

### Using Enums

```swift
var day = Weekday.monday
day = Weekday.tuesday
```

Swift won't let you use invalid values. When you type `Weekday.`, Swift shows all options.

### Enum Shortcuts

**Skip enum name after first assignment:**

```swift
var day = Weekday.monday
day = .tuesday  // Swift knows it's Weekday.tuesday
day = .friday
```

**Behind the scenes:** Swift stores enums as integers (0, 1, 2...), making them faster than strings.

### Common Mistakes

```swift
// WRONG: Invalid case
var day = Weekday.monday
day = Weekday.saturday  // Error: doesn't exist

// WRONG: Mixing types
var day = Weekday.monday
day = "Tuesday"  // Error: can't assign String

// Enums are type-safe
enum Direction {
    case north, south, east, west
}
var heading = Direction.north
heading = Weekday.monday  // Error: can't mix types
```

**Key Takeaway:** Enums define valid values for a type. Prevent typos and invalid data. More efficient than strings. Use shorthand after first assignment. Swift enforces only valid cases.

### Optional: Why Does Swift Need Enums?

Enums give meaningful names to values:

```swift
// Without enums:
let direction = 1  // What does 1 mean?

// With enums:
enum Direction {
    case north, south, east, west
}
let heading = Direction.north  // Clear and safe
```

**Benefits:** Clear names, Swift refuses invalid cases, stored as integers (fast), more powerful in Swift than other languages.

---

## Summary: Complex Data Types

**Arrays**

- Ordered collections of same type
- Access by index (starts at 0)
- Methods: `append()`, `remove(at:)`, `contains()`, `sorted()`, `count`
- Create empty: `var scores = [Int]()`
- Type-safe: one type only

**Dictionaries**

- Key-value pairs with meaningful keys
- Reading keys returns optionals
- Use `default:` for fallback values
- Create empty: `var heights = [String: Int]()`
- Keys unique—setting existing key overwrites
- Fast retrieval regardless of size

**Sets**

- Unique, unordered items
- Extremely fast lookups
- Use `insert()` to add
- Automatically remove duplicates
- Create: `Set(["Name1", "Name2"])` or `Set<String>()`
- Perfect for "does this exist?"

**Enums**

- Define valid named values
- Prevent typos and invalid data
- More efficient than strings
- Use shorthand: `.monday` instead of `Weekday.monday`
- Type-safe: only defined cases
- Stored as integers

**Choose the right type:**

- Order and duplicates? → **Array**
- Meaningful keys? → **Dictionary**
- Fast lookups and uniqueness? → **Set**
- Specific valid values? → **Enum**

---

## Where Now?

You've finished day three, and you're starting to write useful Swift code – make sure to tell the world about your progress!

**Tomorrow:** You'll continue with more complex data types, learning about type annotations and how to be more explicit about the data your code uses.

---

**Previous:** [Day 2 - Simple data types, part 2](Day%202%20-%20Simple%20data%20types,%20part%202.md) | **Next:** [Day 4 - Complex data types, part 2](Day%204%20-%20Complex%20data%20types,%20part%202.md)
