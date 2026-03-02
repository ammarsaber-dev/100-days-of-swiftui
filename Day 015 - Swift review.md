**March 02, 2026** • [Day 014 - Optionals](Day%20014%20-%20Optionals.md) ← → [Day 016 - Project 1, part one](Day%20016%20-%20Project%201,%20part%20one.md)

A review of all Swift fundamentals covered in Days 1–14. Repetition helps cement these concepts — they are the foundation of everything you'll write in SwiftUI.

> 📖 Follow the full review here: [Learn essential Swift in one hour](https://www.hackingwithswift.com/articles/242/learn-essential-swift-in-one-hour) 🔤 Test your knowledge of the most useful terms for Swift developers with the [Swift word search (PDF)](https://www.hackingwithswift.com/files/100/15-wordsearch.pdf) — and if you need help, check the [Swift glossary](https://www.hackingwithswift.com/glossary)

---

## Variables & Constants

```swift
var name = "Ted"
name = "Rebecca"       // variables can change

let user = "Daphne"    // constants cannot change

print(user)
```

Use `let` by default; only use `var` when you need to change the value.

---

## Strings

```swift
let actor = "Tom Cruise 🏃‍♂️"
let quote = "He tapped a sign saying \"Believe\" and walked away."
let movie = """
A day in
the life of an
Apple engineer
"""

print(actor.count)
print(quote.hasPrefix("He"))
print(quote.hasSuffix("Away."))  // false — strings are case-sensitive
```

---

## Integers

```swift
let score = 10
let higherScore = score + 10
let halvedScore = score / 2

var counter = 10
counter += 5

let number = 120
print(number.isMultiple(of: 3))

let id = Int.random(in: 1...1000)
```

---

## Decimals & Booleans

```swift
let score = 3.0        // Double — Swift won't mix Int and Double without conversion

let goodDogs = true
var isSaved = false
isSaved.toggle()       // flips true ↔ false
```

---

## String Interpolation

```swift
let name = "Taylor"
let age = 26
let message = "I'm \(name) and I'm \(age) years old."
print(message)
```

---

## Arrays

```swift
var colors = ["Red", "Green", "Blue"]
let numbers = [4, 8, 15, 16]
var readings = [0.1, 0.5, 0.8]

print(colors[0])
print(readings[2])

colors.append("Tartan")     // type must match existing items
colors.remove(at: 0)
print(colors.count)
print(colors.contains("Octarine"))
```

> 💡 Make sure an item exists at the index you're asking for, otherwise your code will crash.

---

## Dictionaries

```swift
let employee = [
    "name": "Taylor",
    "job": "Singer"
]

print(employee["name", default: "Unknown"])
print(employee["job", default: "Unknown"])
```

Always provide a default value — reading a dictionary key returns an optional.

---

## Sets

```swift
var numbers = Set([1, 1, 3, 5, 7])  // duplicates ignored, order not preserved
print(numbers)

numbers.insert(10)
print(numbers.contains(3))          // effectively instant, even for 10,000,000 items
```

---

## Enums

```swift
enum Weekday {
    case monday, tuesday, wednesday, thursday, friday
}

var day = Weekday.monday
day = .friday
```

---

## Type Annotations

```swift
let player: String = "Roy"
var luckyNumber: Int = 13
let pi: Double = 3.141
var isEnabled: Bool = true
var albums: [String] = ["Red", "Fearless"]
var user: [String: String] = ["id": "@twostraws"]
var books: Set<String> = Set(["The Bluest Eye", "Foundation"])

// Empty collections
var teams: [String] = [String]()
var clues = [String]()

// Enum with type annotation
enum UIStyle { case light, dark, system }
var style: UIStyle = .light
```

---

## Conditions

```swift
let age = 16

if age < 12 {
    print("You can't vote")
} else if age < 18 {
    print("You can vote soon.")
} else {
    print("You can vote now.")
}

// && requires both conditions to be true; || requires either to be true
let temp = 26
if temp > 20 && temp < 30 { print("It's a nice day.") }
```

---

## Switch Statements

```swift
enum Weather { case sun, rain, wind }
let forecast = Weather.sun

switch forecast {
case .sun:   print("A nice day.")
case .rain:  print("Pack an umbrella.")
default:     print("Should be okay.")
}
```

Switch statements must be exhaustive — all possible values must be handled.

---

## Ternary Operator

```swift
let age = 18
let canVote = age >= 18 ? "Yes" : "No"
```

---

## Loops

```swift
let platforms = ["iOS", "macOS", "tvOS", "watchOS"]
for os in platforms { print("Swift works on \(os).") }

for i in 1...12  { print("5 x \(i) is \(5 * i)") }  // 1 through 12 inclusive
for i in 1..<13  { print("5 x \(i) is \(5 * i)") }  // excludes 13

// Ignore loop variable with _
var lyric = "Haters gonna"
for _ in 1...5 { lyric += " hate" }

// while loop
var count = 10
while count > 0 { print("\(count)…"); count -= 1 }
print("Go!")

// continue skips current iteration; break exits the loop entirely
let files = ["me.jpg", "work.txt", "sophie.jpg"]
for file in files {
    if file.hasSuffix(".jpg") == false { continue }
    print("Found picture: \(file)")
}
```

---

## Functions

```swift
func printTimesTables(number: Int) {
    for i in 1...12 { print("\(i) x \(number) is \(i * number)") }
}
printTimesTables(number: 5)

// Return value
func rollDice() -> Int {
    Int.random(in: 1...6)   // return keyword optional for single-line functions
}

// Tuple return — multiple values
func getUser() -> (firstName: String, lastName: String) {
    (firstName: "Taylor", lastName: "Swift")
}
let user = getUser()
print("Name: \(user.firstName) \(user.lastName)")

// Destructuring — ignore unneeded values with _
let (firstName, _) = getUser()

// External/internal parameter labels
func printTimesTables(for number: Int) {
    for i in 1...12 { print("\(i) x \(number) is \(i * number)") }
}
printTimesTables(for: 5)

// Skip external label with _
func isUppercase(_ string: String) -> Bool { string == string.uppercased() }
let result = isUppercase("HELLO, WORLD")

// Default parameter values
func greet(_ person: String, formal: Bool = false) {
    if formal { print("Welcome, \(person)!") }
    else       { print("Hi, \(person)!")     }
}
greet("Tim", formal: true)
greet("Taylor")
```

---

## Error Handling

```swift
enum PasswordError: Error { case short, obvious }

func checkPassword(_ password: String) throws -> String {
    if password.count < 5  { throw PasswordError.short   }
    if password == "12345" { throw PasswordError.obvious }
    return password.count < 10 ? "OK" : "Good"
}

let string = "12345"
do {
    let result = try checkPassword(string)
    print("Rating: \(result)")
} catch PasswordError.obvious {
    print("I have the same combination on my luggage!")
} catch {
    print("There was an error.")
}
```

Always include a `catch` block that handles every possible error.

---

## Closures

```swift
// Basic closure
let sayHello = { print("Hi there!") }
sayHello()

// Closure with parameters
let sayHello = { (name: String) -> String in
    "Hi \(name)!"
}

// Full syntax → progressively simplified
let team = ["Gloria", "Suzanne", "Tiffany", "Tasha"]

let onlyT = team.filter({ (name: String) -> Bool in return name.hasPrefix("T") })
let onlyT = team.filter({ name in name.hasPrefix("T") })  // inferred types
let onlyT = team.filter { name in name.hasPrefix("T") }   // trailing closure
let onlyT = team.filter { $0.hasPrefix("T") }             // shorthand argument
```

---

## Structs

```swift
struct Album {
    let title: String
    let artist: String
    var isReleased = true

    func printSummary() { print("\(title) by \(artist)") }
    mutating func removeFromSale() { isReleased = false }  // mutating required to change properties
}

let red = Album(title: "Red", artist: "Taylor Swift")  // memberwise initializer — auto-generated
print(red.title)
red.printSummary()

// Computed property — with getter and setter
struct Employee {
    let name: String
    var vacationAllocated = 14
    var vacationTaken = 0

    var vacationRemaining: Int {
        get { vacationAllocated - vacationTaken }
        set { vacationAllocated = vacationTaken + newValue }  // newValue provided by Swift
    }
}

// Property observers
struct Game {
    var score = 0 {
        didSet { print("Score is now \(score)") }  // willSet runs before the change
    }
}

// Custom initializer
struct Player {
    let name: String
    let number: Int

    init(name: String) {
        self.name = name
        number = Int.random(in: 1...99)
    }
}

// Access control
struct BankAccount {
    private(set) var funds = 0          // readable outside, not writable

    mutating func deposit(amount: Int)  { funds += amount }
    mutating func withdraw(amount: Int) -> Bool {
        if funds > amount { funds -= amount; return true }
        return false
    }
}

// Static properties and methods — belong to the type, not instances
struct AppData {
    static let version = "1.3 beta 2"
    static let settings = "settings.json"
}
print(AppData.version)
```

---

## Classes

```swift
// Inheritance
class Employee {
    let hours: Int
    init(hours: Int) { self.hours = hours }
    func printSummary() { print("I work \(hours) hours a day.") }
}

class Developer: Employee {
    func work() { print("I'm coding for \(hours) hours.") }
    override func printSummary() {
        print("I spend \(hours) hours a day searching Stack Overflow.")
    }
}

let novall = Developer(hours: 8)
novall.work()
novall.printSummary()

// Child initializer — own properties first, then super
class Vehicle {
    let isElectric: Bool
    init(isElectric: Bool) { self.isElectric = isElectric }
}

class Car: Vehicle {
    let isConvertible: Bool
    init(isElectric: Bool, isConvertible: Bool) {
        self.isConvertible = isConvertible
        super.init(isElectric: isElectric)
    }
}

// Shared data — all copies point to the same object
class Singer {
    var name = "Adele"
}
var singer1 = Singer()
var singer2 = singer1
singer2.name = "Justin"
print(singer1.name)  // "Justin" — both changed
print(singer2.name)  // "Justin"

// Deinitializer
class User {
    let id: Int
    init(id: Int) { self.id = id; print("User \(id): I'm alive!") }
    deinit { print("User \(id): I'm dead!") }
}

for i in 1...3 {
    let user = User(id: i)
    print("User \(user.id): I'm in control!")
}

// Constant class — variable properties can still change
class User {
    var name = "Paul"
}
let user = User()
user.name = "Taylor"  // allowed — no mutating keyword needed in classes
print(user.name)
```

Classes differ from structs in five ways: inheritance, no memberwise initializer, shared data (reference type), deinitializers, and constant instances can have variable properties changed.

---

## Protocols & Extensions

```swift
// Protocol — defines required functionality
protocol Vehicle {
    var name: String { get }
    var currentPassengers: Int { get set }
    func estimateTime(for distance: Int) -> Int
    func travel(distance: Int)
}

struct Car: Vehicle {
    let name = "Car"
    var currentPassengers = 1
    func estimateTime(for distance: Int) -> Int { distance / 50 }
    func travel(distance: Int) { print("I'm driving \(distance)km.") }
}

// Use protocol as a type — works with any conforming type
func commute(distance: Int, using vehicle: Vehicle) {
    if vehicle.estimateTime(for: distance) > 100 { print("Too slow!") }
    else { vehicle.travel(distance: distance) }
}

let car = Car()
commute(distance: 100, using: car)

// Extension — add functionality to any type
extension String {
    func trimmed() -> String {
        self.trimmingCharacters(in: .whitespacesAndNewlines)
    }
    mutating func trim() { self = self.trimmed() }
    var lines: [String] { self.components(separatedBy: .newlines) }
}

var quote = "   The truth is rarely pure and never simple   "
let trimmed = quote.trimmed()
quote.trim()

let lyrics = """
But I keep cruising
Can't stop, won't stop moving
"""
print(lyrics.lines.count)

// Protocol extension — default implementations for all conforming types
extension Collection {
    var isNotEmpty: Bool { isEmpty == false }
}

let guests = ["Mario", "Luigi", "Peach"]
if guests.isNotEmpty { print("Guest count: \(guests.count)") }
```

---

## Optionals

```swift
// if let
let opposites = ["Mario": "Wario", "Luigi": "Waluigi"]
let peachOpposite = opposites["Peach"]   // String? — might be nil

if let marioOpposite = opposites["Mario"] {
    print("Mario's opposite is \(marioOpposite)")
}

// guard let — exits if optional is empty; value lives after the block
func printSquare(of number: Int?) {
    guard let number = number else {
        print("Missing input")
        return
    }
    print("\(number) x \(number) is \(number * number)")
}

// Nil coalescing — unwrap or use default
let tvShows = ["Archer", "Babylon 5", "Ted Lasso"]
let favorite = tvShows.randomElement() ?? "None"

let input = ""
let number = Int(input) ?? 0

// Optional chaining
let names = ["Arya", "Bran", "Robb", "Sansa"]
let chosen = names.randomElement()?.uppercased()
print("Next in line: \(chosen ?? "No one")")

// try?
enum UserError: Error { case badID, networkFailed }
func getUser(id: Int) throws -> String { throw UserError.networkFailed }

if let user = try? getUser(id: 23) { print("User: \(user)") }
```

---

> 🎉 That's all the Swift fundamentals! Next up: SwiftUI projects — the difficulty ramps up from here. Enjoy the quiet while it lasts!
> 
> 💡 To continue learning: [100 Days of SwiftUI](https://www.hackingwithswift.com/100/swiftui)