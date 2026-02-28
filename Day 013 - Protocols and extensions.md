**February 28, 2026** • [Day 012 - Classes](Day%20012%20-%20Classes.md) ← → [Day 014 - Optionals](Day%20014%20-%20Optionals.md)

Protocols, extensions, and protocol extensions are some of the most Swifty features in the language. Protocol extensions in particular allow us to replace large, complex inheritance hierarchies with smaller, simpler protocols that can be combined together — fulfilling Tony Hoare's idea that "inside every large program, there is a small program trying to get out." You'll use protocols from your very first SwiftUI project.

---

## Quick Reference

```swift
// Protocol
protocol Purchaseable {
    var name: String { get set }
}

struct Book: Purchaseable {
    var name: String
    var author: String
}

func buy(_ item: Purchaseable) {
    print("I'm buying \(item.name)")
}

// Opaque return type
func getRandomNumber() -> some Equatable {
    Int.random(in: 1...6)
}

func getRandomBool() -> some Equatable {
    Bool.random()
}

// Extension — returns new value vs. modifies in place
extension String {
    func trimmed() -> String {
        self.trimmingCharacters(in: .whitespacesAndNewlines)
    }

    mutating func trim() {
        self = self.trimmed()
    }

    var lines: [String] {
        self.components(separatedBy: .newlines)
    }
}

// Protocol extension with default implementation
protocol Person {
    var name: String { get }
    func sayHello()
}

extension Person {
    func sayHello() {
        print("Hi, I'm \(name)")
    }
}

struct Employee: Person {
    let name: String
}

let taylor = Employee(name: "Taylor Swift")
taylor.sayHello() // "Hi, I'm Taylor Swift"
```

---

## Protocols

A protocol defines a contract — a list of properties and methods that a conforming type must implement. Swift enforces this at compile time, so when we say a type conforms to a protocol Swift will make sure it has all the methods and properties required.

```swift
protocol Vehicle {
    var name: String { get }
    var currentPassengers: Int { get set }
    func printSummary()
}
```

Properties must be marked `{ get }` for read-only or `{ get set }` for read-write. Methods are listed with just their signature — no body, no implementation.

To conform to a protocol, add a colon and the protocol name after the type name, then implement everything it requires:

```swift
struct Car: Vehicle {
    var name = "Car"
    var currentPassengers = 1
    func printSummary() {
        print("A \(name) carrying \(currentPassengers) passengers.")
    }
}
```

Conforming types can have additional properties and methods beyond what the protocol requires — the protocol just sets the minimum required functionality. A type can also conform to multiple protocols by listing them separated by commas:

```swift
struct Employee: Payable, NeedsTraining, HasVacation { ... }
```

### Using protocols as types

The real power of protocols is that they let functions work with any conforming type, making our code more flexible while Swift still enforces the rules.

Rather than a function that only works with one specific type:

```swift
struct Book {
    var name: String
}

func buy(_ book: Book) {
    print("I'm buying \(book.name)")
}
```

We can create a protocol that declares the basic functionality we need — here just a `name` string:

```swift
protocol Purchaseable {
    var name: String { get set }
}
```

Then define as many structs as we need, each conforming to that protocol:

```swift
struct Book: Purchaseable {
    var name: String
    var author: String
}

struct Movie: Purchaseable {
    var name: String
    var actors: [String]
}

struct Car: Purchaseable {
    var name: String
    var manufacturer: String
}

struct Coffee: Purchaseable {
    var name: String
    var strength: Int
}
```

Each type has different extra properties — that's fine. Protocols determine the minimum required functionality, but conforming types can always add more. Now we can rewrite `buy()` to accept any `Purchaseable` item:

```swift
func buy(_ item: Purchaseable) {
    print("I'm buying \(item.name)")
}
```

Inside `buy()`, Swift guarantees `item.name` exists because `Purchaseable` requires it. It doesn't guarantee any other properties — only the ones specifically declared in the protocol. Protocols let us create blueprints of how types share functionality, then use those blueprints in our functions to let them work on a wider variety of data.

---

## Opaque Return Types

Swift has a feature called **opaque return types**, marked with the `some` keyword. You don't need to understand in full detail how they work — only that they exist and do a very specific job. You'll see them immediately in SwiftUI, so it's important to be familiar with them.

Let's start with two simple functions:

```swift
func getRandomNumber() -> Int {
    Int.random(in: 1...6)
}

func getRandomBool() -> Bool {
    Bool.random()
}
```

> 💡 `Bool.random()` returns either true or false. Unlike random integers and decimals, no parameters are needed because there are no customization options.

Both `Int` and `Bool` conform to `Equatable`, which is what lets us use `==`. We might try returning `Equatable` from both functions:

```swift
// ❌ Won't compile
func getRandomNumber() -> Equatable {
    Int.random(in: 1...6)
}

func getRandomBool() -> Equatable {
    Bool.random()
}
```

Swift throws an error: _"protocol 'Equatable' can only be used as a generic constraint because it has Self or associated type requirements."_ The reason is that returning `Equatable` doesn't make sense — we can't compare an `Int` and a `Bool` using `==` even if both conform to `Equatable`, because they aren't interchangeable types. By returning just `Equatable`, we've hidden their exact type and Swift can no longer safely compare two results.

This is where `some` comes in:

```swift
// ✅ Works
func getRandomNumber() -> some Equatable {
    Int.random(in: 1...6)
}

func getRandomBool() -> some Equatable {
    Bool.random()
}

print(getRandomNumber() == getRandomNumber()) // fine — both are Int behind the scenes
```

The key distinction:

- Returning `Equatable` — "any type conforming to Equatable, we don't know what" — Swift loses track of the specific type
- Returning `some Equatable` — "one specific type conforming to Equatable, but I'm not saying which" — Swift still knows the exact underlying type

Or with a vehicle analogy: returning `Vehicle` means "any sort of vehicle type but we don't know what", whereas returning `some Vehicle` means "a specific sort of vehicle type but we don't want to say which one."

Opaque return types let us hide information from ourselves while Swift still tracks the real type internally. This gives us flexibility to change the underlying type later without breaking anything else.

### Why this matters in SwiftUI

In SwiftUI, every layout you describe — toolbar, tab bar, scrolling grid, labels, fonts, colors — becomes the return type of a function. Writing that type explicitly would be impossibly long and would change every time you adjusted your layout. Instead:

```swift
var body: some View {
    // your layout here
}
```

`some View` tells Swift "this returns some kind of view to lay out — you figure out the exact type." Swift always knows the real underlying type; we just don't have to write it out. When you see `some View` in SwiftUI, that's exactly what's happening.

---

## Extensions

Extensions let us add functionality to any existing type — even ones we didn't create, including Apple's own types.

```swift
var quote = "   The truth is rarely pure and never simple   "
```

To trim whitespace we'd normally write:

```swift
let trimmed = quote.trimmingCharacters(in: .whitespacesAndNewlines)
```

`.whitespacesAndNewlines` comes from Apple's Foundation API — and so does `trimmingCharacters(in:)`. Having to call that every time is wordy, so we can write an extension to make it shorter:

```swift
extension String {
    func trimmed() -> String {
        self.trimmingCharacters(in: .whitespacesAndNewlines)
    }
}

let trimmed = quote.trimmed()
```

Breaking it down: `extension` tells Swift we're adding functionality to an existing type; `String` is the type we're extending; everything inside the braces is added to all strings. `self` automatically refers to the current string instance.

### Extensions vs. global functions

We could write a global function instead:

```swift
func trim(_ string: String) -> String {
    string.trimmingCharacters(in: .whitespacesAndNewlines)
}

let trimmed2 = trim(quote)
```

That's actually less code. But extensions have important advantages over global functions:

- When you type `quote.` Xcode brings up a list of methods including all extension methods — easy to discover
- Extensions are naturally grouped by the type they extend — global functions are hard to organize and keep track of
- Extension methods have full access to the type's internal data, including `private` members
- Extensions make it easy to modify values in place using `mutating`

### Mutating in extensions

When returning a new value, use word endings like `ed` or `ing`. When modifying in place, use a shorter name — this is part of Swift's design guidelines:

```swift
extension String {
    func trimmed() -> String {       // returns a new string
        self.trimmingCharacters(in: .whitespacesAndNewlines)
    }

    mutating func trim() {           // modifies the string directly
        self = self.trimmed()
    }
}

quote.trim() // quote is now trimmed in place
```

> 💡 This naming rule applies across Swift — `sorted()` returns a new array, `sort()` sorts in place.

### Computed properties in extensions

Extensions can add computed properties, but **not stored properties** — adding new stored properties would affect the actual size of the data type, meaning every instance of that type everywhere would need more memory, which would cause all sorts of problems:

```swift
extension String {
    var lines: [String] {
        self.components(separatedBy: .newlines)
    }
}

let lyrics = """
But I keep cruising
Can't stop, won't stop moving
It's like I got this music in my mind
Saying it's gonna be alright
"""

print(lyrics.lines.count) // 4
```

### Extensions and initializers

Placing a custom initializer inside an extension preserves Swift's auto-generated memberwise initializer. Placing it directly inside the struct removes it — because Swift assumes the custom init is the intended way to create instances:

```swift
struct Book {
    let title: String
    let pageCount: Int
    let readingHours: Int
}
```

If we add a custom initializer directly inside the struct, the memberwise initializer is gone:

```swift
// ❌ Memberwise initializer lost
struct Book {
    let title: String
    let pageCount: Int
    let readingHours: Int

    init(title: String, pageCount: Int) {
        self.title = title
        self.pageCount = pageCount
        self.readingHours = pageCount / 50
    }
}
```

But if we put the custom initializer in an extension instead, both are preserved:

```swift
// ✅ Both initializers available
extension Book {
    init(title: String, pageCount: Int) {
        self.title = title
        self.pageCount = pageCount
        self.readingHours = pageCount / 50
    }
}

let lotr = Book(title: "Lord of the Rings", pageCount: 1178, readingHours: 24) // memberwise
let hobbit = Book(title: "The Hobbit", pageCount: 310)                          // custom
```

### Organizing code with extensions

Extensions are also great for organizing your own code:

- **Conformance grouping** — add a protocol conformance in a dedicated extension, keeping all its methods together rather than mixed in with unrelated code. When reading the extension you immediately know its scope.
- **Purpose grouping** — split a large type into focused extensions, e.g. one for loading/saving, one for UI logic, one for networking

> 💡 Splitting a type into extensions doesn't make it smaller — the type is exactly the same size. It just makes it easier to read and maintain.

---

## Protocol Extensions

Protocol extensions let us add method implementations directly to a protocol, so every conforming type gets them automatically.

It's very common to check whether an array has any values like this:

```swift
let guests = ["Mario", "Luigi", "Peach"]

if guests.isEmpty == false {
    print("Guest count: \(guests.count)")
}
```

Some prefer using `!`:

```swift
if !guests.isEmpty {
    print("Guest count: \(guests.count)")
}
```

Neither reads naturally. We can fix this with a simple extension on `Array`:

```swift
extension Array {
    var isNotEmpty: Bool {
        isEmpty == false
    }
}

if guests.isNotEmpty {
    print("Guest count: \(guests.count)")
}
```

But this only works on arrays. `Array`, `Set`, and `Dictionary` all conform to a built-in protocol called `Collection`, which already requires `isEmpty` to exist. So if we extend `Collection` instead, all three get `isNotEmpty` at once:

```swift
extension Collection {
    var isNotEmpty: Bool {
        isEmpty == false
    }
}
```

One word change — now `isNotEmpty` works on arrays, sets, dictionaries, and any other type conforming to `Collection`. That tiny extension exists in thousands of Swift projects.

### Default implementations

Protocol extensions can provide default implementations of protocol methods. Conforming types can use the default or provide their own:

```swift
protocol Person {
    var name: String { get }
    func sayHello()
}

extension Person {
    func sayHello() {
        print("Hi, I'm \(name)")
    }
}

// No sayHello() needed — uses the default
struct Employee: Person {
    let name: String
}

let taylor = Employee(name: "Taylor Swift")
taylor.sayHello() // "Hi, I'm Taylor Swift"
```

This is **protocol-oriented programming** — list required methods in a protocol, add default implementations in a protocol extension. All conforming types get the behavior for free and can override it when needed.

### Why protocol extensions matter

`allSatisfy()` is a perfect real-world example — it works identically on arrays, sets, and dictionaries:

```swift
// Array
let numbers = [4, 8, 15, 16]
let allEven = numbers.allSatisfy { $0.isMultiple(of: 2) }

// Set
let numbers2 = Set([4, 8, 15, 16])
let allEven2 = numbers2.allSatisfy { $0.isMultiple(of: 2) }

// Dictionary — each key/value pair is passed in, return true or false
let numbers3 = ["four": 4, "eight": 8, "fifteen": 15, "sixteen": 16]
let allEven3 = numbers3.allSatisfy { $0.value.isMultiple(of: 2) }
```

Rather than writing three separate implementations, Swift wrote `allSatisfy()` once as an extension on `Sequence` — a protocol all three types conform to — and it immediately became available on all of them. This is why Swift is often described as a protocol-oriented programming language.

---

## Checkpoint 8

Create a protocol describing a building, then make two structs — `House` and `Office` — that conform to it.

**Your protocol must require:**

1. A property for the number of rooms
2. A property for the cost as an integer (e.g. 500,000 for a building costing $500,000)
3. A property for the name of the estate agent responsible for selling it
4. A method that prints a sales summary describing the building and its properties

> 💡 Hints:
> 
> - Design the protocol fully before touching any structs
> - Protocols can't contain method implementations — just signatures
> - Decide whether properties should be `{ get }` or `{ get set }` — rooms might be fixed, or maybe you want to account for someone adding an extension to the building later
> - Use `{ get }` for read-only and `{ get set }` for read-write

**Solution:**

```swift
protocol Building {
    var rooms: Int { get }
    var cost: Int { get set }
    var agentName: String { get set }
    func printSummary()
}

struct House: Building {
    var rooms: Int
    var cost: Int
    var agentName: String

    func printSummary() {
        print("A house with \(rooms) rooms, costs $\(cost), listed by \(agentName).")
    }
}

struct Office: Building {
    var rooms: Int
    var cost: Int
    var agentName: String

    func printSummary() {
        print("An office with \(rooms) rooms, costs $\(cost), listed by \(agentName).")
    }
}

let house = House(rooms: 4, cost: 500_000, agentName: "John Smith")
let office = Office(rooms: 20, cost: 2_000_000, agentName: "Jane Doe")

house.printSummary()
office.printSummary()
```

---

## Bonus: Getting the Most from Protocol Extensions

> ⚠️ This section goes beyond beginner Swift — you don't need this to start building SwiftUI apps. It's purely for the curious!

We can add a `squared()` method to `Int` using an extension:

```swift
extension Int {
    func squared() -> Int {
        self * self
    }
}

let wholeNumber = 5
print(wholeNumber.squared()) // 25
```

But what if we want the same on `Double` without copying the code? Both `Int` and `Double` conform to the `Numeric` protocol, so we can extend that instead. However, this won't work:

```swift
// ❌ Won't compile — self * self could be a Double, not an Int
extension Numeric {
    func squared() -> Int {
        self * self
    }
}
```

The fix is to use `Self` (capital S) as the return type — meaning "whatever type this method was actually called on":

```swift
// ✅ Works for both Int and Double
extension Numeric {
    func squared() -> Self {
        self * self
    }
}
```

Remember: `self` refers to the current _value_, `Self` refers to the current _type_. So for an integer set to 5, `squared()` effectively becomes:

```swift
func squared() -> Int { 5 * 5 }
```

And for a decimal set to 3.141:

```swift
func squared() -> Double { 3.141 * 3.141 }
```

### Equatable and Comparable

Swift has a protocol called `Equatable` that allows comparing two objects with `==` and `!=`. We can make a struct conform to it and Swift will automatically compare all properties:

```swift
struct User: Equatable {
    let name: String
}

let user1 = User(name: "Link")
let user2 = User(name: "Zelda")
print(user1 == user2)  // false
print(user1 != user2)  // true
```

There's also `Comparable`, which lets Swift sort objects. Swift can't implement this automatically — you need to write a `<`function that returns `true` if the first instance should be sorted before the second:

```swift
struct User: Equatable, Comparable {
    let name: String
}

func <(lhs: User, rhs: User) -> Bool {
    lhs.name < rhs.name
}

let user1 = User(name: "Taylor")
let user2 = User(name: "Adele")
print(user1 < user2) // false — "Taylor" comes after "Adele"
```

The clever part: because Swift uses protocol extensions internally, implementing just `<` and `==` automatically gives you `<=`, `>`, and `>=` for free:

```swift
print(user1 <= user2)
print(user1 > user2)
print(user1 >= user2)
```

Even better — you don't need to add `Equatable` at all when conforming to `Comparable`. Behind the scenes, `Comparable` uses protocol inheritance to automatically include `Equatable`:

```swift
struct User: Comparable {
    let name: String
}
```

That's protocol extensions and protocol inheritance working together — a small amount of code unlocking a lot of behavior automatically.

---

**Topics:** [Protocols](https://www.hackingwithswift.com/quick-start/beginners/how-to-create-and-use-protocols) • [Opaque Return Types](https://www.hackingwithswift.com/quick-start/beginners/how-to-use-opaque-return-types) • [Extensions](https://www.hackingwithswift.com/quick-start/beginners/how-to-create-and-use-extensions) • [Protocol Extensions](https://www.hackingwithswift.com/quick-start/beginners/how-to-create-and-use-protocol-extensions)