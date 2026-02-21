**February 16, 2026** • [Day 002 - Simple data types, part 2](Day%20002%20-%20Simple%20data%20types,%20part%202.md) ← → [Day 004 - Complex data types, part 2](Day%20004%20-%20Complex%20data%20types,%20part%202.md)

Learn about arrays, dictionaries, sets, and enums—ways to group multiple pieces of data together.

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

## Arrays

Arrays group multiple values of the same type in order. They hold zero to millions of items and automatically adapt to size. You'll use them constantly—they're the most common collection type.

### Creating and Reading Arrays

```swift
var beatles = ["John", "Paul", "George", "Ringo"]
let numbers = [4, 8, 15, 16, 23, 42]
var temperatures = [25.3, 28.2, 26.4]
```

**Reading by index** (starts at 0):

```swift
print(beatles[0])      // "John" (first item)
print(numbers[1])      // 8 (second item)
print(temperatures[2]) // 26.4 (third item)
```

This is called **zero-based indexing**—the first item is at position 0, not 1.

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
// Full syntax
var scores = Array<Int>()
scores.append(100)

// Shorthand (preferred)
var albums = [String]()
albums.append("Folklore")

// Swift infers type from initial values
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

// Reverse (doesn't rearrange, just remembers to reverse when accessed)
let presidents = ["Bush", "Obama", "Trump", "Biden"]
let reversedPresidents = presidents.reversed()
```

### Common Mistakes

```swift
// ❌ Index out of range
let names = ["Anna", "Alex", "Brian"]
print(names[10])  // Crash!

// ❌ Appending to constant
let scores = [100, 90, 85]
scores.append(95)  // Error

// ✅ Use var for mutable arrays
var scores = [100, 90, 85]
scores.append(95)
```

Accessing an invalid index crashes your app—better than silently returning wrong data. Arrays can grow/shrink automatically if they're declared with `var`.

---

## Dictionaries

Arrays access data by position, which can be dangerous. Dictionaries store data using meaningful keys instead—more convenient and safer.

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

print(employee2["name"])
print(employee2["job"])
print(employee2["location"])
```

Keys are on the left, values on the right. Much clearer than array indices like `employee[0]`.

### Optionals and Default Values

Reading a non-existent key returns an **optional** (might be nil):

```swift
print(employee2["password"])  // key doesn't exist, returns nil
```

Swift doesn't crash like arrays—it returns an optional. Provide a default value to avoid dealing with optionals:

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
// ❌ Reading without default returns Optional
let name = employee2["name"]  // Optional("Taylor Swift")

// ✅ Use default value
let name = employee2["name", default: "Unknown"]

// ❌ Empty dictionary needs type
var data = [:]  // Error

// ✅ Specify types
var data = [String: Int]()
```

Dictionaries optimize for fast retrieval—instant lookup even with 100,000 items. Use them when you need to look up data by meaningful identifiers (`user["country"]`) rather than remembering numeric positions.

**When to use default values:** If missing value = failure/zero, use default (always get a value). If missing value = hasn't happened yet, don't use default (check for nil).

---

## Sets

Sets are like arrays but don't allow duplicates and don't store items in order. They're perfect for "does this exist?" questions and automatic uniqueness.

### Creating Sets

```swift
let people = Set(["Denzel Washington", "Tom Cruise", "Nicolas Cage", "Samuel L Jackson"])
print(people)  // order might differ each time
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

**1. No duplicates:** Perfect for uniqueness requirements (like Screen Actors Guild requiring unique stage names).

**2. Extremely fast lookups:**

```swift
let actors = Set(["Denzel Washington", "Tom Cruise", /* ...998 more */ ])
print(actors.contains("Tom Cruise"))  // Returns instantly
```

With an array of 1,000 items, Swift might check all 1,000. With a set of 1,000,000 items, it returns instantly. Even with 10 million items, `contains()` on sets runs instantly.

### Useful Set Methods

```swift
people.contains("Tom Cruise")  // extremely fast
people.count
people.sorted()  // returns sorted array (not set)
```

### Common Mistakes

```swift
// ❌ Using append()
var names = Set<String>()
names.append("John")  // Error

// ✅ Use insert()
names.insert("John")

// Sets automatically remove duplicates
var numbers = Set([1, 2, 2, 3, 3, 3])
print(numbers)  // {1, 2, 3}

// ❌ Expecting indices
let set = Set(["A", "B", "C"])
print(set[0])  // Error: sets don't have indices
```

Use sets when you need fast existence checks or automatic uniqueness. Use arrays when order matters or duplicates are needed (most cases).

---

## Enums

An **enum** (enumeration) is a set of named values you define. More efficient and safer than strings—prevents typos and invalid data.

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

Only these five values allowed—no typos, no accidents. Swift won't let you use invalid values.

### Using Enums

```swift
var day = Weekday.monday
day = Weekday.tuesday
```

When you type `Weekday.`, Swift shows all available options.

### Enum Shortcuts

Skip enum name after first assignment:

```swift
var day = Weekday.monday
day = .tuesday  // Swift knows it's Weekday.tuesday
day = .friday
```

**Behind the scenes:** Swift stores enums as integers (0, 1, 2...), making them faster than strings.

### Common Mistakes

```swift
// ❌ Invalid case
var day = Weekday.monday
day = Weekday.saturday  // Error: case doesn't exist

// ❌ Mixing types
var day = Weekday.monday
day = "Tuesday"  // Error: can't assign String to Weekday

// ❌ Mixing different enums
enum Direction {
    case north, south, east, west
}
var heading = Direction.north
heading = Weekday.monday  // Error: can't mix enum types
```

Enums give meaningful names to values (clearer than magic numbers like `1`, `2`, `3`) and are type-safe—Swift enforces only defined cases. Use enums when you have a known, fixed set of related values.

---

## Choosing the Right Collection Type

- **Arrays**: Need order and allow duplicates → Most common use case
- **Dictionaries**: Need meaningful keys for lookup → Second most common
- **Sets**: Need fast lookups and guaranteed uniqueness → Less common but powerful
- **Enums**: Need specific, fixed set of named values → For type-safe constants

**Usage frequency:** Arrays (most) → Dictionaries → Sets → Enums

---

**Topics:** [Arrays](https://www.hackingwithswift.com/quick-start/beginners/how-to-store-ordered-data-in-arrays) • [Dictionaries](https://www.hackingwithswift.com/quick-start/beginners/how-to-store-and-find-data-in-dictionaries) • [Sets](https://www.hackingwithswift.com/quick-start/beginners/how-to-use-sets-for-fast-data-lookup) • [Enums](https://www.hackingwithswift.com/quick-start/beginners/how-to-create-and-use-enums)