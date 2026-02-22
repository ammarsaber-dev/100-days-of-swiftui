**February 22, 2026** • [Day 010 - Structs, part one](Day%20010%20-%20Structs,%20part%20one.md) ← → [Day 012 - Classes](Day%20012%20-%20Classes.md)

Structs are already powerful, but today you'll learn two features that take them further: access control, which lets you hide internal data from the outside world, and static properties and methods, which belong to the struct itself rather than to any individual instance. Both are used extensively in SwiftUI from your very first project.

---

## Quick Reference

```swift
// Access control
struct BankAccount {
    private(set) var funds = 0

    mutating func deposit(amount: Int) {
        funds += amount
    }

    mutating func withdraw(amount: Int) -> Bool {
        if funds >= amount {
            funds -= amount
            return true
        } else {
            return false
        }
    }
}

var account = BankAccount()
account.deposit(amount: 100)
print(account.funds)        // readable from outside
// account.funds -= 1000   — not allowed, can't write directly

// Static properties and methods
struct School {
    static var studentCount = 0

    static func add(student: String) {
        print("\(student) joined the school.")
        studentCount += 1
    }
}

School.add(student: "Taylor Swift")
print(School.studentCount)

// Shared app-wide constants
struct AppData {
    static let version = "1.3 beta 2"
    static let saveFilename = "settings.json"
    static let homeURL = "https://www.hackingwithswift.com"
}

// Static example data for SwiftUI previews
struct Employee {
    let username: String
    let password: String

    static let example = Employee(username: "cfederighi", password: "hairforceone")
}
```

---

## Access Control

By default, Swift's structs let you access their properties and methods freely from anywhere. But often that isn't what you want — sometimes you want to hide some data from external access, or because some methods need to be called in a certain way or order and shouldn't be touched externally.

Here's the problem without access control:

```swift
struct BankAccount {
    var funds = 0

    mutating func deposit(amount: Int) {
        funds += amount
    }

    mutating func withdraw(amount: Int) -> Bool {
        if funds >= amount {
            funds -= amount
            return true
        } else {
            return false
        }
    }
}
```

This should be used like this:

```swift
var account = BankAccount()
account.deposit(amount: 100)
let success = account.withdraw(amount: 200)

if success {
    print("Withdrew money successfully")
} else {
    print("Failed to get the money")
}
```

But because `funds` is exposed publicly, nothing stops someone from doing this:

```swift
account.funds -= 1000
```

That completely bypasses the logic we put in place to prevent overdrafts, and now the program can behave in unpredictable ways. The fix is one extra word:

```swift
private var funds = 0
```

Now `funds` is accessible inside `deposit()` and `withdraw()`, but reading or writing it from outside the struct won't compile. This is **access control** — it controls how a struct's properties and methods can be accessed from outside the struct.

### Access control options

Swift provides several levels, but when learning you'll mainly use these:

|Keyword|Meaning|
|---|---|
|`private`|Don't let anything outside the struct use this|
|`fileprivate`|Don't let anything outside the current file use this|
|`public`|Let anyone, anywhere use this|
|`private(set)`|Anyone can _read_ it, but only the struct can _write_ it|

In the `BankAccount` example, `private(set)` is actually the best choice for `funds` — you can check your balance at any time, but you can't change it without going through the proper methods:

```swift
private(set) var funds = 0
```

> ⚠️ If you use `private` access control for one or more properties, chances are you'll need to create your own initializer.

### Why bother with access control?

It might seem pointless — you wrote the code, so you could just remove the restrictions. But that misses the point entirely.

Sometimes access control is used in code you don't own — such as Apple's own APIs — and you simply have to abide by the rules. But even in your own code it's valuable. Access control lets us determine how a value should be used, so that if something needs to be accessed very carefully, you must follow the rules.

A real example: in the Unwrap Swift learning app, learned sections are stored like this:

```swift
private var learnedSections = Set<String>()
```

It's private because learning a section needs to do more than just insert a string into that set — it also needs to update the user interface to reflect the change, and save the new information so the app remembers it was learned. Without `private`, it would be easy to write to it directly and accidentally leave the UI out of sync with the data. By marking it private, Swift simply won't allow that mistake.

Access control also controls how other people see your code — its "surface area". If a struct has 30 properties and methods but 25 are marked `private`, it's immediately clear which 5 are meant to be used externally and which are internal plumbing.

> 💡 Access control can be quite a thorny issue, particularly when you take into account external code. Apple's full documentation on it can be found here: https://docs.swift.org/swift-book/LanguageGuide/AccessControl.html

---

## Static Properties and Methods

Every instance of a struct gets its own unique copy of the struct's properties. But sometimes you want a property or method to belong to the **struct itself** — shared across everything, or usable without creating an instance at all. That's what `static` is for.

```swift
struct School {
    static var studentCount = 0

    static func add(student: String) {
        print("\(student) joined the school.")
        studentCount += 1
    }
}

School.add(student: "Taylor Swift")
print(School.studentCount)
```

Notice we never created an instance of `School` — we used `add()` and `studentCount` directly on the struct itself. Both belong to `School` the type, not to any individual school object.

Also notice that `add()` modifies `studentCount` without needing the `mutating` keyword — `mutating` is only needed for regular instance methods when an instance might be a constant. With static methods there is no instance at all, so it doesn't apply.

### Mixing static and non-static

There are two rules when combining static and non-static code:

- **Static code cannot access non-static code** — it wouldn't make sense, because there's no way to know which particular instance's properties you'd be referring to.
- **Non-static code can access static code** — use the type name directly, like `School.studentCount`, or use `Self` to refer to the current type.

This brings up an important distinction: `self` (lowercase) refers to the current _value_ of the struct, while `Self` (uppercase) refers to the current _type_. It follows Swift's usual naming convention — types always start with a capital letter, just like `Int`, `Double`, and `Bool`.

### Two practical uses for static data

**1. Storing shared app-wide values**

Rather than scattering constants throughout your code, you can group them in one place:

```swift
struct AppData {
    static let version = "1.3 beta 2"
    static let saveFilename = "settings.json"
    static let homeURL = "https://www.hackingwithswift.com"
}
```

Anywhere you need the app's version number — an about screen, debug output, logging information, support emails — you just read `AppData.version`. One source of truth, accessible everywhere.

**2. Creating example instances for SwiftUI previews**

SwiftUI works best when it can show previews of your app as you develop, and those previews often need sample data. A static `example` property on your struct makes this trivial:

```swift
struct Employee {
    let username: String
    let password: String

    static let example = Employee(username: "cfederighi", password: "hairforceone")
}
```

Whenever you need a sample `Employee` for a design preview, you just use `Employee.example` and you're done.

### A real-world example: the Unwrap app

Another good example of static data in practice also comes from the Unwrap app. A shared URL is stored statically so it can be referenced anywhere without creating an instance:

```swift
struct Unwrap {
    static let appURL = "https://itunes.apple.com/app/id1440611372"
}
```

The app also uses a static property and method together to store "fair random" entropy — randomness that's been made _less_ random on purpose, so that quiz questions don't repeat back to back:

```swift
private static var entropy = Int.random(in: 1...1000)

static func getEntropy() -> Int {
    entropy += 1
    return entropy
}
```

`entropy` starts off random, but increments by 1 every time `getEntropy()` is called, ensuring a fairer spread of questions. It's marked `private` so nothing outside the struct can access it directly — only `getEntropy()` can. And because it's static, it's shared across the entire app rather than belonging to any one instance.

Two interesting things to note here: first, the `Unwrap` struct doesn't actually need to be a struct — it should really be an `enum`with no cases, since you never want to create an instance of it. An enum makes that intent explicit. Second, this is a good real-world example of access control and static properties working together — two of today's topics combining naturally.

> 💡 Static properties and methods are only useful in a handful of occasions, but they are still a useful tool to have around.

---

## Checkpoint 6

Create a struct to store information about a car, including its model, number of seats, and current gear. Add a method to change gears up or down. Think carefully about access control and variables vs. constants.

**Things to consider:**

- A car's model and seat count don't change once built — they can be constants. Current gear does change — that should be a variable.
- The gear-changing method should validate its input — there's no gear 0, and you can safely assume a maximum of 10 gears.
- If you use `private` access control, you'll likely need a custom initializer. (Is `private` the best choice here? Try it and see!)
- Any method that changes a property needs the `mutating` keyword.

**Solution:**

```swift
struct Car {
    let model: String
    let numberOfSeats: Int
    private(set) var currentGear = 1

    mutating func changeGear(by amount: Int) {
        let newGear = currentGear + amount
        if newGear < 1 {
            print("Already in the lowest gear.")
        } else if newGear > 10 {
            print("Already in the highest gear.")
        } else {
            currentGear = newGear
            print("Changed to gear \(currentGear).")
        }
    }
}

var car = Car(model: "Tesla Model S", numberOfSeats: 5)
car.changeGear(by: 1)
car.changeGear(by: 2)
car.changeGear(by: -8)
print(car.currentGear)
```

> 💡 `private(set)` is a great fit for `currentGear` — anyone can read the current gear, but it can only be changed through `changeGear()`, ensuring the validation logic always runs.

---

**Topics:** [Access Control](https://www.hackingwithswift.com/quick-start/beginners/how-to-limit-access-to-internal-data-using-access-control) • [Static Properties and Methods](https://www.hackingwithswift.com/quick-start/beginners/static-properties-and-methods)