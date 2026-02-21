**February 18, 2026** • [Day 007 - Functions, part one](Day%20007%20-%20Functions,%20part%20one.md) ← → [Day 009 - Closures](Day%20009%20-%20Closures.md)

No one wants things to go wrong, but they do — files don't exist, networks go down, passwords are too short. Swift makes you handle errors rather than ignore them, so your code is forced to be prepared. If you don't at least acknowledge that errors might happen, your code simply won't compile.

---

## Quick Reference

```swift
// Default parameter value
func printTimesTables(for number: Int, end: Int = 12) {
    for i in 1...end {
        print("\(i) x \(number) is \(i * number)")
    }
}
printTimesTables(for: 5)         // uses default end of 12
printTimesTables(for: 5, end: 20) // overrides default

// Define possible errors
enum PasswordError: Error {
    case short, obvious
}

// Throwing function
func checkPassword(_ password: String) throws -> String {
    if password.count < 5 { throw PasswordError.short }
    if password == "12345" { throw PasswordError.obvious }
    if password.count < 8 { return "OK" }
    else if password.count < 10 { return "Good" }
    else { return "Excellent" }
}

// Calling a throwing function
do {
    let result = try checkPassword("12345")
    print("Password rating: \(result)")
} catch PasswordError.short {
    print("Please use a longer password.")
} catch PasswordError.obvious {
    print("I have the same combination on my luggage!")
} catch {
    print("There was an error.")
}
```

---

## Default Parameter Values

Sometimes you want a function to be flexible, but most of the time you need the same value. Rather than forcing callers to always pass it in, you can set a default:

```swift
func printTimesTables(for number: Int, end: Int = 12) {
    for i in 1...end {
        print("\(i) x \(number) is \(i * number)")
    }
}

printTimesTables(for: 5, end: 20)  // custom end
printTimesTables(for: 8)           // uses default end of 12
```

The default value is used automatically when you don't provide one. When you do provide one, it overrides the default.

You've already seen this in action with `removeAll()`:

```swift
var characters = ["Lana", "Pam", "Ray", "Sterling"]
print(characters.count)
characters.removeAll()
print(characters.count)
```

By default, `removeAll()` frees the array's memory too. But there's a second form for when you're about to add lots of items back and want to keep the capacity:

```swift
characters.removeAll(keepingCapacity: true)
```

That `keepingCapacity` parameter defaults to `false` — sensible for most cases — but you can override it when you need to.

### When to use default parameters

Default parameters shine when a function has one or two options that rarely change. For example:

```swift
func findDirections(from: String, to: String, route: String = "fastest", avoidHighways: Bool = false) {
    // code here
}
```

Now you can call it in whatever way suits the situation:

```swift
findDirections(from: "London", to: "Glasgow")
findDirections(from: "London", to: "Glasgow", route: "scenic")
findDirections(from: "London", to: "Glasgow", route: "scenic", avoidHighways: true)
```

Shorter, simpler code most of the time — but the flexibility is there when you need it.

---

## Handling Errors

Swift error handling follows three steps:

1. **Define** the possible errors that can happen
2. **Write** a function that can throw those errors
3. **Call** that function and handle any errors that occur

### Step 1 — Define your errors

Create an enum that builds on Swift's built-in `Error` type:

```swift
enum PasswordError: Error {
    case short, obvious
}
```

This just defines what errors are possible — not what they mean or when they happen.

### Step 2 — Write a throwing function

Mark the function with `throws` before the return type, then use `throw` to trigger an error:

```swift
func checkPassword(_ password: String) throws -> String {
    if password.count < 5 {
        throw PasswordError.short
    }

    if password == "12345" {
        throw PasswordError.obvious
    }

    if password.count < 8 {
        return "OK"
    } else if password.count < 10 {
        return "Good"
    } else {
        return "Excellent"
    }
}
```

A few important things to know:

- `throws` means the function _might_ throw errors — not that it always will
- You don't specify _which_ error type the function throws, just that it can throw
- When `throw` is called, the function exits immediately — no value is returned
- If no error is thrown, the function must return its promised value as normal

### Step 3 — Call the function and handle errors

Use `do`, `try`, and `catch` together:

```swift
let string = "12345"

do {
    let result = try checkPassword(string)
    print("Password rating: \(result)")
} catch {
    print("There was an error.")
}
```

- `do` starts the block of work that might throw
- `try` must be written before every call to a throwing function — it's a visual signal that execution might be interrupted here
- `catch` handles any errors that were thrown

You can also catch specific errors if you want to give more meaningful responses:

```swift
let string = "12345"

do {
    let result = try checkPassword(string)
    print("Password rating: \(result)")
} catch PasswordError.short {
    print("Please use a longer password.")
} catch PasswordError.obvious {
    print("I have the same combination on my luggage!")
} catch {
    print("There was an error.")
}
```

> ⚠️ You must always have a final `catch` block that can handle any remaining errors — you can't only catch specific ones and leave others unhandled.

> 💡 Swift automatically provides an `error` value inside every `catch` block. Use `error.localizedDescription` to get a readable message you can show to users.

### Why try before every throwing function?

Most languages only use `do` and `catch`. Swift adds `try` because it makes it immediately obvious which lines can actually throw. In a block with multiple function calls, you can see at a glance exactly where errors might come from:

```swift
do {
    try throwingFunction1()
    nonThrowingFunction1()
    try throwingFunction2()
    nonThrowingFunction2()
    try throwingFunction3()
} catch {
    // handle errors
}
```

The first, third, and fifth calls can throw — the second and fourth can't. `try` makes that crystal clear.

### try! — use with caution

There's an alternative, `try!`, that doesn't need `do` and `catch`:

```swift
let result = try! checkPassword("secretP@ssword")
```

If an error is thrown, your code will crash. Only use this when you're absolutely certain an error cannot happen.

### When should you write throwing functions?

When errors occur in a function, you have options: handle them inside the function, throw them back to the caller ("error propagation" or "bubbling up"), or do a mix of both. All are valid.

When starting out, keep the number of throwing functions low. They can feel "infectious" — once you add one, you need to handle errors wherever it's called, and if those errors bubble up further, more places need handling too. Start small and work outward as you get comfortable.

> 💡 For a different perspective on throwing functions, check out [this blog post by Donny Wals](https://www.donnywals.com/working-with-throwing-functions-in-swift/).

---

## Checkpoint 4

Write a function that accepts an integer from 1 through 10,000 and returns its integer square root. The catches:

1. You can't use Swift's `sqrt()` function — find the square root yourself
2. If the number is less than 1 or greater than 10,000, throw an "out of bounds" error
3. Only consider integer square roots — don't worry about decimals
4. If no integer square root exists, throw a "no root" error

> 💡 **Hints:**
> 
> - Brute force it — loop through numbers, multiplying each by itself, looking for a match
> - The square root of 10,000 is 100, so your loop only needs to go up to 100
> - If you reach the end of the loop without finding a match, throw the "no root" error
> - One "out of bounds" error case is enough — you don't need separate ones for too low and too high

```swift
enum SqrtError: Error {
    case outOfBounds, noRoot
}

func integerSqrt(_ number: Int) throws -> Int {
    if number < 1 || number > 10_000 {
        throw SqrtError.outOfBounds
    }

    for i in 1...100 {
        if i * i == number {
            return i
        }
    }

    throw SqrtError.noRoot
}

do {
    let root = try integerSqrt(25)
    print("Root: \(root)")
} catch SqrtError.outOfBounds {
    print("Number must be between 1 and 10,000.")
} catch SqrtError.noRoot {
    print("No integer square root found.")
} catch {
    print("There was an error: \(error.localizedDescription)")
}
```

---

**Topics:** [Default Parameters](https://www.hackingwithswift.com/quick-start/beginners/how-to-provide-default-values-for-parameters) • [Error Handling](https://www.hackingwithswift.com/quick-start/beginners/how-to-handle-errors-in-functions)