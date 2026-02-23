**February 23, 2026** • [Day 011 - Structs, part two](Day%20011%20-%20Structs,%20part%20two.md) ← → [Day 013 - Protocols and extensions](Day%20013%20-%20Protocols%20and%20extensions.md)

Classes seem similar to structs but differ in five key ways. SwiftUI uses structs for UI and classes for data — you will need both.

---

## Quick Reference

```swift
// Basic class
class Game {
    var score = 0 {
        didSet { print("Score is now \(score)") }
    }
}

var newGame = Game()
newGame.score += 10

// Inheritance + override
class Employee {
    let hours: Int
    init(hours: Int) { self.hours = hours }
    func printSummary() { print("I work \(hours) hours a day.") }
}

class Developer: Employee {
    override func printSummary() {
        print("I'm a developer who will sometimes work \(hours) hours a day, but other times spend hours arguing about whether code should be indented using tabs or spaces.")
    }
}

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

let teslaX = Car(isElectric: true, isConvertible: false)

// Deep copy
class User {
    var username = "Anonymous"
    func copy() -> User {
        let user = User()
        user.username = username
        return user
    }
}

// Deinitializer
class User {
    let id: Int
    init(id: Int) { self.id = id; print("User \(id): I'm alive!") }
    deinit { print("User \(id): I'm dead!") }
}
```

---

## Classes vs. Structs

Classes and structs share a lot — properties, methods, property observers, access control, custom initializers. But classes differ in five key ways:

|                            |Classes|Structs|
|---|---|---|
| **Inheritance**            |Can build on another class|Cannot inherit|
| **Memberwise initializer** |Never generated|Generated automatically|
| **Copying**                |All copies share the same data|Each copy is unique|
| **Deinitializer**          |Has one|Does not have one|
| **Constant instances**     |Variable properties can still change|Variable properties cannot change|

The most important for SwiftUI is copying — shared data means changing a user's name in one screen updates every other screen automatically.

A basic class looks identical to a struct except for the keyword:

```swift
class Game {
    var score = 0 {
        didSet { print("Score is now \(score)") }
    }
}
```

The other differences each have a reason:

- **Inheritance** was heavily used in UIKit where long class hierarchies were common (class A built on B, built on C, etc.). It's much less common in SwiftUI.
- **No memberwise initializer** makes sense because if a parent class added a new property, it would silently break every child class that relied on the generated initializer.
- **Constant classes can change variable properties** because the constant refers to the signpost — which object we're pointing at — not the data inside it. This is fundamentally different from structs, where each copy holds its own data directly.
- **Deinitializers** exist because a class instance can be referenced from many places at once. Swift needs to know when the _last_ reference disappears before it's safe to clean up.

### Why no memberwise initializer?

If Swift generated one and you later added a property to a parent class, it would break all initializers in every child class. By requiring you to write your own, changes are deliberate and their implications are clear.

---

## Inheritance

To inherit from a class, add a colon and the parent class name:

```swift
class Employee {
    let hours: Int
    init(hours: Int) { self.hours = hours }
}

class Developer: Employee {
    func work() { print("I'm writing code for \(hours) hours.") }
}

class Manager: Employee {
    func work() { print("I'm going to meetings for \(hours) hours.") }
}

let robert = Developer(hours: 8)
let joseph = Manager(hours: 10)
robert.work()
joseph.work()
```

Child classes inherit all properties and methods. Adding `printSummary()` to `Employee` makes it immediately available on `Developer`:

```swift
let novall = Developer(hours: 8)
novall.printSummary()
```

### Overriding methods

Use `override` to change an inherited method. Swift enforces this in both directions — forgetting it when needed, or using it when not needed, both cause a build error:

```swift
override func printSummary() {
    print("I'm a developer who will sometimes work \(hours) hours a day, but other times spend hours arguing about whether code should be indented using tabs or spaces.")
}
```

> 💡 If a parent has `work()` with no parameters and a child has `work(location: String)`, that is **not** an override — it's a new method with a different signature.

### Final classes

Mark a class `final` to prevent anyone subclassing it. The class can still inherit from others, but nothing can inherit from it. Apple does this with classes like `AVPlayerViewController` (subclassing causes undefined behavior) and `Timer` ("do not subclass"). In Swift, `final` makes this a compile-time rule rather than just a warning.

> 💡 `final` classes used to be more performant — that's no longer true, but many developers still use `final` to signal "don't subclass this unless I say so."

---
## Class Initializers

Swift never generates a memberwise initializer for classes. You must write your own, or give all properties default values:

```swift
class Vehicle {
    let isElectric: Bool

    init(isElectric: Bool) {
        self.isElectric = isElectric
    }
}
```

If a child class has its own properties, it must have its own initializer. This won't compile:

```swift
// ❌ Error: missing call to super.init()
class Car: Vehicle {
    let isConvertible: Bool

    init(isConvertible: Bool) {
        self.isConvertible = isConvertible
        // Swift refuses to build — Vehicle's isElectric has no value
    }
}
```

The fix: the child must set its own properties first, then call `super.init()` to let the parent handle its own:

```swift
// ✅ Correct
class Car: Vehicle {
    let isConvertible: Bool

    init(isElectric: Bool, isConvertible: Bool) {
        self.isConvertible = isConvertible   // own property first
        super.init(isElectric: isElectric)   // then parent
    }
}

let teslaX = Car(isElectric: true, isConvertible: false)
```

`super` works like `self` but for the parent class — you can use it to call any parent method, not just initializers.

> 💡 If a subclass has no custom initializers of its own, it automatically inherits the parent's.

---

## Copying Classes

All copies of a class instance share the same data:

```swift
class User {
    var username = "Anonymous"
}

var user1 = User()
var user2 = user1
user2.username = "Taylor"

print(user1.username) // "Taylor"
print(user2.username) // "Taylor"
```

This is a feature, not a bug — it's how SwiftUI shares data across your app. If `User` were a struct, `user1` would still print "Anonymous".

### Value types vs. reference types

Structs are **value types** — data is stored directly in the variable, so every copy is independent. Classes are **reference types** — the variable is a signpost pointing to data in memory. Copying gives you a new signpost pointing to the same place. As Chris Eidhof says about value types: "this is so natural it seems like stating the obvious." Choosing a class over a struct signals that you want shared data, not independent copies.

### Deep copies

To get a truly independent copy, handle it yourself:

```swift
class User {
    var username = "Anonymous"

    func copy() -> User {
        let user = User()
        user.username = username
        return user
    }
}
```

---

## Deinitializers

A deinitializer runs when the last copy of a class instance is destroyed. No `func`, no parameters, no return value, never called directly. Structs don't have deinitializers because you can't copy them.

```swift
class User {
    let id: Int
    init(id: Int) { self.id = id; print("User \(id): I'm alive!") }
    deinit { print("User \(id): I'm dead!") }
}
```

### Scope

Variables are destroyed when they leave scope — functions, `if` conditions, and `for` loops all create local scopes using braces. For a class, losing one copy doesn't trigger the deinitializer — only losing the _last_ copy does.

```swift
for i in 1...3 {
    let user = User(id: i)
    print("User \(user.id): I'm in control!")
} // user destroyed here — deinit runs each iteration
```

If you store instances elsewhere, they live until that storage is cleared:

```swift
var users = [User]()
for i in 1...3 {
    let user = User(id: i)
    print("User \(user.id): I'm in control!")
    users.append(user)
}
print("Loop is finished!")
users.removeAll() // deinit runs here
print("Array is clear!")
```

### Why structs don't have deinitializers

Swift uses **Automatic Reference Counting (ARC)** to track class instances — adding 1 when a copy is made, subtracting 1 when destroyed. When the count hits 0, the deinitializer runs and memory is freed. Structs hold their own data independently, so no tracking is needed.

> 💡 Anne Cahalan: "Code should read like sentences, which makes me think my classes should read like chapters. So the deinitializer goes at the end, it's the ~fin~ of the class!"

---

## Variables Inside Classes

A constant class instance can still have its variable properties changed — because the constant is the signpost, not the data it points to:

```swift
class User {
    var name = "Paul"
}

let user = User()
user.name = "Taylor" // allowed — signpost is constant, data is not
print(user.name)     // "Taylor"
```

If `name` were `let`, it couldn't change — like writing the name in permanent ink. Making both instance and property `var` lets you point at a different object entirely:

```swift
var user = User()
user.name = "Taylor"
user = User()
print(user.name) // "Paul" — whole object replaced
```

The four combinations:

|Instance|Property|Effect|
|---|---|---|
|`let`|`let`|Same user, name never changes|
|`let`|`var`|Same user, name can change|
|`var`|`let`|Can point to different users, names never change|
|`var`|`var`|Can point to different users, names can change|

Variable properties on a shared class instance mean other parts of your code could change that value by surprise. Constant properties guarantee they can't.

Structs work differently — they hold data directly, so changing a property on a constant struct means implicitly changing the struct itself, which isn't allowed. That's also why structs need `mutating` on methods that change data, but classes don't — Swift just checks whether the property itself is `var` or `let`.

---

## Checkpoint 7

Build a class hierarchy for animals:

1. `Animal` base class with a `legs` integer property
2. `Dog` subclass with a `speak()` method printing a generic bark
3. `Cat` subclass with a `speak()` method and an `isTame` Boolean provided via initializer
4. `Corgi` and `Poodle` subclasses of `Dog`, each overriding `speak()`
5. `Persian` and `Lion` subclasses of `Cat`, each overriding `speak()`

> 💡 All subclasses have four legs — pass that up to `Animal` via `super.init()`.

**Solution:**

```swift
class Animal {
    let legs: Int
    init(legs: Int) { self.legs = legs }
}

class Dog: Animal {
    init() { super.init(legs: 4) }
    func speak() { print("Woof!") }
}

class Cat: Animal {
    let isTame: Bool
    init(isTame: Bool) {
        self.isTame = isTame
        super.init(legs: 4)
    }
    func speak() { print("Meow!") }
}

class Corgi: Dog {
    override func speak() { print("Yip yip!") }
}

class Poodle: Dog {
    override func speak() { print("Woof woof!") }
}

class Persian: Cat {
    override func speak() { print("Purrr...") }
}

class Lion: Cat {
    override func speak() { print("Roar!") }
}
```

---

**Topics:** [Classes](https://www.hackingwithswift.com/quick-start/beginners/how-to-create-your-own-classes) • [Inheritance](https://www.hackingwithswift.com/quick-start/beginners/how-to-make-one-class-inherit-from-another) • [Class Initializers](https://www.hackingwithswift.com/quick-start/beginners/how-to-add-initializers-for-classes) • [Copying Classes](https://www.hackingwithswift.com/quick-start/beginners/how-to-copy-classes) • [Deinitializers](https://www.hackingwithswift.com/quick-start/beginners/how-to-create-a-deinitializer-for-a-class) • [Variables in Classes](https://www.hackingwithswift.com/quick-start/beginners/how-to-work-with-variables-inside-classes)