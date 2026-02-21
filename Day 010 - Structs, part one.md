**February 21, 2026** • [Day 009 - Closures](Day%20009%20-%20Closures.md) ← → [Day 011 - Structs, part two](Day%20011%20-%20Structs,%20part%20two.md)

Structs are one of the ways Swift lets you create your own data types out of several small types. Most of Swift's built-in types — `String`, `Int`, `Bool`, `Array` — are themselves structs. Get your data representations right, and your code will often follow naturally.

---

## Quick Reference

```swift
// Basic struct with a method
struct Album {
    let title: String
    let artist: String
    let year: Int

    func printSummary() {
        print("\(title) (\(year)) by \(artist)")
    }
}

let red = Album(title: "Red", artist: "Taylor Swift", year: 2012)
red.printSummary()

// Mutating method
struct Employee {
    let name: String
    var vacationRemaining: Int

    mutating func takeVacation(days: Int) {
        if vacationRemaining > days {
            vacationRemaining -= days
            print("I'm going on vacation!")
            print("Days remaining: \(vacationRemaining)")
        } else {
            print("Oops! There aren't enough days remaining.")
        }
    }
}

var archer = Employee(name: "Sterling Archer", vacationRemaining: 14)
archer.takeVacation(days: 5)
print(archer.vacationRemaining)

// Computed property with getter and setter
struct Employee {
    let name: String
    var vacationAllocated = 14
    var vacationTaken = 0

    var vacationRemaining: Int {
        get {
            vacationAllocated - vacationTaken
        }
        set {
            vacationAllocated = vacationTaken + newValue
        }
    }
}

// Property observers
struct Game {
    var score = 0 {
        willSet { print("Score will be: \(newValue)") }
        didSet  { print("Score was: \(oldValue)") }
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
```

---

## Structs

A struct lets you bundle related properties and methods into a reusable custom type:

```swift
struct Album {
    let title: String
    let artist: String
    let year: Int

    func printSummary() {
        print("\(title) (\(year)) by \(artist)")
    }
}

let red = Album(title: "Red", artist: "Taylor Swift", year: 2012)
let wings = Album(title: "Wings", artist: "BTS", year: 2016)

print(red.title)
print(wings.artist)

red.printSummary()   // "Red (2012) by Taylor Swift"
wings.printSummary() // "Wings (2016) by BTS"
```

Both `red` and `wings` come from the same `Album` struct, but once created they are separate — just like two strings. When `printSummary()` is called on `red`, it uses `red`'s own values; when called on `wings`, it uses `wings`'s values.

### Naming conventions

Type names (`Album`, `Employee`) use **UpperCamelCase**. Properties and methods use **lowerCamelCase**. This is convention rather than a strict rule, but it's helpful and universally followed in Swift.

### Key vocabulary

|Term|Meaning|
|---|---|
|**Property**|A variable or constant that belongs to a struct|
|**Method**|A function that belongs to a struct|
|**Instance**|A specific value created from a struct (e.g. `red`, `wings`)|
|**Initializer**|The special method used to create an instance|

### Methods vs. functions

The only real difference is that methods belong to a type. This gives them two advantages: they can freely access the type's other properties and methods, and they avoid **namespace pollution** — `wakeUp()` isn't a globally reserved name if it only exists as `someUser.wakeUp()`. Write 100 functions and you have 100 globally reserved names; putting functionality into methods keeps things tidy.

### Mutating methods

If a method needs to change any property, it must be marked `mutating`:

```swift
struct Employee {
    let name: String
    var vacationRemaining: Int

    mutating func takeVacation(days: Int) {
        if vacationRemaining > days {
            vacationRemaining -= days
            print("I'm going on vacation!")
            print("Days remaining: \(vacationRemaining)")
        } else {
            print("Oops! There aren't enough days remaining.")
        }
    }
}

var archer = Employee(name: "Sterling Archer", vacationRemaining: 14)
archer.takeVacation(days: 5)
print(archer.vacationRemaining)
```

> ⚠️ `mutating` methods can only be called on instances created with `var`. If you create an employee with `let`, Swift makes the employee and all its data constant — calling a mutating method on it won't compile.

Two important rules:

- Swift trusts you — marking a method `mutating` prevents it from being called on constant structs, even if the method doesn't actually change anything.
- A non-mutating method cannot call a mutating method — you must mark them both as `mutating`.

### The memberwise initializer

Swift silently generates an initializer from your struct's properties — this is called the **memberwise initializer**:

```swift
var archer1 = Employee(name: "Sterling Archer", vacationRemaining: 14)
// equivalent to:
var archer2 = Employee.init(name: "Sterling Archer", vacationRemaining: 14)
```

If a property has a default value, it becomes an optional parameter in the initializer:

```swift
struct Employee {
    let name: String
    var vacationRemaining = 14
}

let kane = Employee(name: "Lana Kane")                           // uses default of 14
let poovey = Employee(name: "Pam Poovey", vacationRemaining: 35) // overrides default
```

> 💡 Use a `var` property (not `let`) when you want a default value that can still be overridden at creation time. Assigning a default to a `let` constant property removes it from the initializer entirely.

You may have already used this without realising — `Double(a)` works because `Double` is itself a struct with an initializer that accepts an integer.

### Structs vs. tuples

A tuple is effectively an anonymous struct — same idea, just without a name. So when do you use which?

For data you pass across multiple functions, a struct is far clearer:

```swift
func authenticate(_ user: User) { ... }
func showProfile(for user: User) { ... }
func signOut(_ user: User) { ... }
```

Compare that to using a tuple everywhere:

```swift
func authenticate(_ user: (name: String, age: Int, city: String)) { ... }
func showProfile(for user: (name: String, age: Int, city: String)) { ... }
func signOut(_ user: (name: String, age: Int, city: String)) { ... }
```

Adding a new property to the struct is trivial — one line. Adding a new value to the tuple means updating every single function signature, which is tedious and error-prone.

The rule: use tuples when you want to return two or more arbitrary values from a single function. Prefer structs for fixed data you'll send or receive multiple times.

---

## Computed Properties

Structs can have two kinds of property:

- **Stored property** — holds a value in memory
- **Computed property** — calculates its value dynamically every time it's accessed (so it can't be defined as a constant `let`)

Computed properties are a blend of both: accessed like a stored property, but work like a function.

Here's the problem a computed property solves — tracking vacation days as a plain stored value loses information:

```swift
struct Employee {
    let name: String
    var vacationRemaining: Int
}

var archer = Employee(name: "Sterling Archer", vacationRemaining: 14)
archer.vacationRemaining -= 5
print(archer.vacationRemaining) // 9
archer.vacationRemaining -= 3
print(archer.vacationRemaining) // 6
```

We've lost how many days were originally allocated. Using a computed property instead:

```swift
struct Employee {
    let name: String
    var vacationAllocated = 14
    var vacationTaken = 0

    var vacationRemaining: Int {
        vacationAllocated - vacationTaken
    }
}

var archer = Employee(name: "Sterling Archer", vacationAllocated: 14)
archer.vacationTaken += 4
print(archer.vacationRemaining) // 10
archer.vacationTaken += 4
print(archer.vacationRemaining) // 6
```

`vacationRemaining` always reflects the current state without you needing to keep it in sync manually. We're reading what looks like a property, but Swift is running code to calculate it every time.

To make a computed property writable too, add explicit `get` and `set` blocks:

```swift
var vacationRemaining: Int {
    get {
        vacationAllocated - vacationTaken
    }
    set {
        vacationAllocated = vacationTaken + newValue
    }
}
```

`newValue` is automatically provided by Swift inside `set` — it holds whatever value was assigned. With both in place:

```swift
var archer = Employee(name: "Sterling Archer", vacationAllocated: 14)
archer.vacationTaken += 4
archer.vacationRemaining = 5
print(archer.vacationAllocated) // 9
```

### Stored vs. computed: when to use which

Use a **stored property** when the value is read often and doesn't depend on other properties — it's faster since nothing is recalculated. Use a **computed property** when the value depends on other properties — you're guaranteed to always get the latest result without manual updates. Lazy properties and property observers help bridge the gap for edge cases.

> 💡 SwiftUI uses computed properties extensively — you'll see them from your very first project.

---

## Property Observers

Property observers let you run code automatically whenever a property changes — no need to manually call a function every time a value is updated:

- `willSet` — runs _before_ the value changes; Swift provides `newValue`
- `didSet` — runs _after_ the value changes; Swift provides `oldValue`

Without a property observer, you'd need to call `print()` manually after every change — and forgetting once means a silent bug:

```swift
struct Game {
    var score = 0
}

var game = Game()
game.score += 10
print("Score is now \(game.score)")
game.score -= 3
print("Score is now \(game.score)")
game.score += 1
// Oops — score changed but was never printed!
```

Attaching `didSet` directly to the property fixes this:

```swift
struct Game {
    var score = 0 {
        didSet {
            print("Score is now \(score)")
        }
    }
}

var game = Game()
game.score += 10 // "Score is now 10"
game.score -= 3  // "Score is now 7"
game.score += 1  // "Score is now 8"
```

Both observers together, showing the automatic `newValue` and `oldValue` constants:

```swift
struct App {
    var contacts = [String]() {
        willSet {
            print("Current value is: \(contacts)")
            print("New value will be: \(newValue)")
        }
        didSet {
            print("There are now \(contacts.count) contacts.")
            print("Old value was \(oldValue)")
        }
    }
}

var app = App()
app.contacts.append("Adrian E")
app.contacts.append("Allen W")
app.contacts.append("Ish S")
```

Note: appending to an array counts as a change, so both observers fire on every `append()`.

### willSet vs. didSet

`didSet` is used far more often — you typically want to react _after_ a change, for example to update the UI or save data. Use `willSet` when you specifically need a snapshot of state _before_ the change. SwiftUI uses `willSet` in some places for animations — it takes a "before" and "after" snapshot and compares the two to work out what needs updating.

> ⚠️ Keep property observers lightweight. Something as innocent as `game.score += 1` can cause serious performance problems if its observer does heavy work.

---

## Custom Initializers

Swift's memberwise initializer is generated automatically, but you can write your own for more control. The one golden rule: **all properties must have a value by the time the initializer ends**.

```swift
struct Player {
    let name: String
    let number: Int

    init(name: String, number: Int) {
        self.name = name
        self.number = number
    }
}

let player = Player(name: "Megan R", number: 15)
```

Three things to notice:

1. No `func` keyword — Swift treats initializers specially
2. No return type — they always return an instance of their own type
3. `self.name` distinguishes the property from the parameter of the same name

Custom initializers don't have to mirror the memberwise one. For example, you could randomise the shirt number:

```swift
struct Player {
    let name: String
    let number: Int

    init(name: String) {
        self.name = name
        number = Int.random(in: 1...99)
    }
}

let player = Player(name: "Megan R")
print(player.number)
```

> ⚠️ You can call other methods inside an initializer, but only _after_ all properties have been given a value — Swift needs to be sure everything is safe before doing anything else.

You can add multiple initializers to a struct, and use external parameter names and default values just like regular functions.

### Losing the memberwise initializer

As soon as you write a custom initializer, Swift removes the auto-generated memberwise one — it assumes your custom init handles everything and that the default one should no longer be available. If you want to keep both, move your custom initializer into an **extension**:

```swift
struct Employee {
    var name: String
    var yearsActive = 0
}

extension Employee {
    init() {
        self.name = "Anonymous"
        print("Creating an anonymous employee…")
    }
}

let roslin = Employee(name: "Laura Roslin") // memberwise still works
let anon = Employee()                        // custom init also works
```

### When to use `self`

`self` refers to the current instance. The most common reason to use it is in initializers, when parameter names match property names:

```swift
struct Student {
    var name: String
    var bestFriend: String

    init(name: String, bestFriend: String) {
        print("Enrolling \(name) in class…")
        self.name = name
        self.bestFriend = bestFriend
    }
}
```

You could avoid it by using different parameter names, but that gets clumsy:

```swift
init(name studentName: String, bestFriend studentBestFriend: String) {
    print("Enrolling \(studentName) in class…")
    name = studentName
    bestFriend = studentBestFriend
}
```

Outside of initializers, you rarely need `self` in structs. The main exception is inside closures that belong to a **class**, where Swift requires it explicitly to make clear you understand what's happening — but that's a topic for later.

---

**Topics:** [Structs](https://www.hackingwithswift.com/quick-start/beginners/how-to-create-your-own-structs) • [Computed Properties](https://www.hackingwithswift.com/quick-start/beginners/how-to-compute-property-values-dynamically) • [Property Observers](https://www.hackingwithswift.com/quick-start/beginners/how-to-take-action-when-a-property-changes) • [Custom Initializers](https://www.hackingwithswift.com/quick-start/beginners/how-to-create-custom-initializers)