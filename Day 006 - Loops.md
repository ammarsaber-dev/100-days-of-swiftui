**February 17, 2026** • [Day 005 - Conditions](Day%20005%20-%20Conditions.md) ← → [Day 007 - Functions, part one](Day%20007%20-%20Functions,%20part%20one.md)

Loops are one of the things that make computers so powerful — the ability to repeat work billions of times a second. Swift gives you two kinds: `for` loops when you know what you're iterating over, and `while` loops when you don't know how many times you'll need to go around.

---

## Quick Reference

```swift
// Loop over an array
for os in platforms {
    print("Swift works on \(os)")
}

// Closed range — 1 through 12, includes 12
for i in 1...12 {
    print("5 x \(i) = \(5 * i)")
}

// Half-open range — 1 up to 5, excludes 5
for i in 1..<5 {
    print(i)  // 1, 2, 3, 4
}

// When you don't need the loop variable
for _ in 1...5 {
    lyric += " hate"
}

// While loop
var countdown = 10
while countdown > 0 {
    print("\(countdown)…")
    countdown -= 1
}

// continue — skip this iteration, keep looping
for filename in filenames {
    if filename.hasSuffix(".jpg") == false { continue }
    print("Found picture: \(filename)")
}

// break — stop the loop entirely
for score in scores {
    if score == 0 { break }
    count += 1
}

// break out of nested loops with a label
outerLoop: for option1 in options {
    for option2 in options {
        if attempt == secret { break outerLoop }
    }
}
```

---

## For Loops

### Looping over an array

The most common use of a `for` loop — go through every item in a collection and do something with it:

```swift
let platforms = ["iOS", "macOS", "tvOS", "watchOS"]

for os in platforms {
    print("Swift works great on \(os).")
}
```

Swift takes one item at a time from `platforms`, puts it into `os`, runs the code inside the braces, then moves to the next item. The variable `os` only exists inside the loop — you're not creating it yourself, Swift does it for you.

The name you choose doesn't matter at all — `os`, `name`, `rubberChicken` — the code works the same. That said, Xcode is smart: if you type `for plat` near a `platforms` array, it'll suggest `for platform in platforms` automatically. Press Return to accept it.

### Ranges

Sometimes you want to loop a specific number of times rather than over a collection. That's what ranges are for:

```swift
for i in 1...12 {
    print("5 x \(i) is \(5 * i)")
}
```

Swift has two range operators and they're subtly different:

- `1...12` — **closed range**, means "1 through 12", includes both ends
- `1..<12` — **half-open range**, means "1 up to 12", excludes the upper bound

```swift
for i in 1...5 { print(i) }   // 1, 2, 3, 4, 5
for i in 1..<5 { print(i) }   // 1, 2, 3, 4
```

> 💡 The half-open range `..<` is perfect for arrays because arrays start at index 0. If an array has 5 items, their indexes are 0–4 — exactly what `0..<array.count` gives you.

You can also use **one-sided ranges** to read from a position to the end of an array:

```swift
let names = ["Piper", "Alex", "Suzanne", "Gloria"]
print(names[1...])   // ["Alex", "Suzanne", "Gloria"] — from index 1 to the end
print(names[1...3])  // ["Alex", "Suzanne", "Gloria"] — from index 1 through 3
```

> ⚠️ `names[1...3]` will crash if the array has fewer than 4 items. Use `names[1...]` when you want "from index 1 to the end" safely.

### Nested loops

You can put a loop inside another loop. This is useful when you need to combine values:

```swift
for i in 1...12 {
    print("The \(i) times table:")
    for j in 1...12 {
        print("  \(j) x \(i) is \(j * i)")
    }
    print()  // empty print() just adds a blank line
}
```

By convention, use `i` for the outer counter, `j` for the inner one, `k` for a third. If you need a fourth counter, that's a sign to use better variable names.

### When you don't need the loop variable

If you just want to run some code a fixed number of times and don't actually need the loop variable, replace it with `_`:

```swift
var lyric = "Haters gonna"

for _ in 1...5 {
    lyric += " hate"
}

print(lyric)  // "Haters gonna hate hate hate hate hate"
```

This isn't just a style thing — Swift will actually warn you to switch to `_` if you declare a loop variable and never use it inside the body.

---

## While Loops

A `while` loop is simpler in structure: give it a condition, and it keeps running until that condition is false.

```swift
var countdown = 10

while countdown > 0 {
    print("\(countdown)…")
    countdown -= 1
}

print("Blast off!")
```

### When to use while instead of for

Use `while` when you don't know ahead of time how many iterations you'll need. A good example is rolling dice — you can't know how many rolls it'll take to hit 20:

```swift
var roll = 0

while roll != 20 {
    roll = Int.random(in: 1...20)
    print("I rolled a \(roll)")
}

print("Critical hit!")
```

`Int.random(in:)` gives you a random integer within a range. `Double.random(in:)` does the same for decimals:

```swift
let id = Int.random(in: 1...1000)
let amount = Double.random(in: 0...1)
```

The rule of thumb: use `for` when you have a known sequence to work through (array, range, dictionary). Use `while` when you need to keep going until something happens — a condition is met, a user makes a choice, a server responds, etc.

---

## Break and Continue

These two keywords let you control a loop mid-flight. They sound similar but do very different things.

### `continue` — skip this iteration

`continue` says "I'm done with this particular item, move on to the next one." The loop itself keeps going:

```swift
let filenames = ["me.jpg", "work.txt", "sophie.jpg", "logo.psd"]

for filename in filenames {
    if filename.hasSuffix(".jpg") == false {
        continue
    }
    print("Found picture: \(filename)")
}
```

When `filename` is `"work.txt"`, `continue` fires and Swift skips the `print()`, jumping straight to the next filename. The loop still runs for all four items — it just skips the ones that don't pass the test.

### `break` — stop the loop entirely

`break` says "I'm done with this loop altogether." Swift exits immediately and skips everything that was left:

```swift
let scores = [1, 8, 4, 3, 0, 5, 2]
var count = 0

for score in scores {
    if score == 0 {
        break
    }
    count += 1
}

print("You had \(count) scores before you got 0.")
```

Here's a more practical example — finding the first 10 common multiples of two numbers, then stopping:

```swift
let number1 = 4
let number2 = 14
var multiples = [Int]()

for i in 1...100_000 {
    if i.isMultiple(of: number1) && i.isMultiple(of: number2) {
        multiples.append(i)
        if multiples.count == 10 {
            break
        }
    }
}

print(multiples)
```

Once we have 10 multiples, there's no point looping through the remaining 99,990 numbers — `break` gets us out immediately.

### Labeled statements — breaking out of nested loops

By default, `break` and `continue` only affect the innermost loop. If you're inside nested loops and want to break all the way out, you need a **labeled statement**:

```swift
let options = ["up", "down", "left", "right"]
let secretCombination = ["up", "up", "right"]

outerLoop: for option1 in options {
    for option2 in options {
        for option3 in options {
            let attempt = [option1, option2, option3]
            if attempt == secretCombination {
                print("The combination is \(attempt)!")
                break outerLoop  // exits all three loops at once
            }
        }
    }
}
```

Without `break outerLoop`, the `break` would only exit the innermost loop — the outer two would keep running even after the answer was already found. The label tells Swift exactly which loop to exit.

> 💡 You can use `continue` with labels too, though it's extremely rare in practice.

---

## Checkpoint 3

Write a `for` loop from 1 through 100. For each number:

- Print **"FizzBuzz"** if it's a multiple of both 3 and 5
- Print **"Fizz"** if it's a multiple of 3
- Print **"Buzz"** if it's a multiple of 5
- Otherwise print the number

> ⚠️ Check for FizzBuzz first — if you check for 3 first, you'll print "Fizz" for numbers like 15 and never reach the FizzBuzz case.

```swift
for i in 1...100 {
    if i.isMultiple(of: 3) && i.isMultiple(of: 5) {
        print("FizzBuzz")
    } else if i.isMultiple(of: 3) {
        print("Fizz")
    } else if i.isMultiple(of: 5) {
        print("Buzz")
    } else {
        print(i)
    }
}
```

---

**Topics:** [For Loops](https://www.hackingwithswift.com/quick-start/beginners/how-to-use-a-for-loop-to-repeat-work) • [While Loops](https://www.hackingwithswift.com/quick-start/beginners/how-to-use-a-while-loop-to-repeat-work) • [Break & Continue](https://www.hackingwithswift.com/quick-start/beginners/how-to-skip-loop-items-with-break-and-continue)