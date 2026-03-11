**March 09, 2026** • [Day 018 - Project 1, part three](Day%20018%20-%20Project%201%2C%20part%20three.md) ← → [Day 020 - Project 2, part 1](Day%20020%20-%20Project%202,%20part%201.md)

Your first challenge day — build a complete unit converter app from scratch using everything learned in Project 1. No new concepts are introduced; this is purely about solidifying the basics.

---

## The Challenge

Build an app that handles unit conversions — users select an input unit and an output unit, enter a value, and see the output of the conversion.

Which units you choose are down to you, but you could choose one of these:

- **Temperature** — Celsius, Fahrenheit, Kelvin
- **Length** — meters, kilometers, feet, yards, miles
- **Time** — seconds, minutes, hours, days
- **Volume** — milliliters, liters, cups, pints, gallons

For example, a length converter might have a segmented control for the input unit, a second segmented control for the output unit, a text field where users enter a number, and a text view showing the result. So if you chose meters for source unit and feet for output unit, then entered 10, you'd see 32.81 as the output.

The only new thing you'll find is that you need to use a different format option for your text field — `.currency` and `.percent`don't make sense here. Use `.number` instead:

```swift
TextField("Enter value", value: $inputValue, format: .number)
```

---

## Tips

**Keep it simple** — go for the simplest solution you can find.

**Use a base unit** — don't write conversions between every pair of units. Instead, convert the input to a single base unit first, then convert from there to the output unit. For example, convert everything to milliliters first, then from milliliters to the target unit.

**Three `@State` properties** — one for the input number, one for the input unit, one for the output unit. You'll need a `TextField`, two pickers, and a text view to show your output.

**Store units as a string array** — loop over them with `ForEach(units, id: \.self)`.

**Format the output** — call `.formatted()` on your result `Double` to add thousands separators and remove unnecessary decimal places:

```swift
someDouble.formatted()
```

> 💡 Apple provides dedicated unit conversion functionality via `Unit` and `Measurement` — see [How to convert units using Unit and Measurement](https://www.hackingwithswift.com/example-code/system/how-to-convert-units-using-unit-and-measurement). However, it is not required — simple arithmetic works perfectly fine, e.g. to convert liters to pints multiply by `2.11338`.

---

## My Solution

```swift
import SwiftUI

struct ContentView: View {

    @State private var inputTempUnit = "Celsius"
    @State private var outputTempUnit = "Fahrenheit"
    @State private var inputTempValue = 0.0
    @FocusState private var inputTempFocused: Bool

    @State private var inputLengthUnit = "Meters"
    @State private var outputLengthUnit = "Kilometers"
    @State private var inputLengthValue = 0.0
    @FocusState private var inputLengthFocused: Bool

    @State private var inputTimeUnit = "Seconds"
    @State private var outputTimeUnit = "Minutes"
    @State private var inputTimeValue = 0.0
    @FocusState private var inputTimeFocused: Bool

    @State private var inputVolumeUnit = "Milliliters"
    @State private var outputVolumeUnit = "Liters"
    @State private var inputVolumeValue = 0.0
    @FocusState private var inputVolumeFocused: Bool

    let tempUnits = ["Celsius", "Fahrenheit", "Kelvin"]
    let lengthUnits = ["Meters", "Kilometers", "Feet", "Yards", "Miles"]
    let timeUnits = ["Seconds", "Minutes", "Hours", "Days"]
    let volumeUnits = ["Milliliters", "Liters", "Cups", "Pints", "Gallons"]

    var convertedTempValue: Double {
        let celsius: Double

        switch inputTempUnit {
        case "Fahrenheit": celsius = (inputTempValue - 32) * 5.0 / 9.0
        case "Kelvin": celsius = inputTempValue - 273.15
        default: celsius = inputTempValue
        }

        switch outputTempUnit {
        case "Fahrenheit": return celsius * 9.0 / 5.0 + 32
        case "Kelvin": return celsius + 273.15
        default: return celsius
        }
    }

    var convertedLengthValue: Double {
        let meters: Double

        // Step 1: Convert Input -> Meters
        switch inputLengthUnit {
        case "Kilometers": meters = inputLengthValue * 1000
        case "Feet": meters = inputLengthValue / 3.2808
        case "Yards": meters = inputLengthValue / 1.0936
        case "Miles": meters = inputLengthValue / 0.00062137
        default: meters = inputLengthValue
        }

        // Step 2: Convert Meters -> Output
        switch outputLengthUnit {
        case "Kilometers": return meters / 1000
        case "Feet": return meters * 3.2808
        case "Yards": return meters * 1.0936
        case "Miles": return meters * 0.00062137
        default: return meters
        }
    }

    var convertedTimeValue: Double {
        let seconds: Double

        // Step 1: Convert Input -> Seconds
        switch inputTimeUnit {
        case "Minutes": seconds = inputTimeValue * 60
        case "Hours": seconds = inputTimeValue * 3600
        case "Days": seconds = inputTimeValue * 86400
        default: seconds = inputTimeValue
        }

        // Step 2: Convert Seconds -> Output
        switch outputTimeUnit {
        case "Minutes": return seconds / 60
        case "Hours": return seconds / 3600
        case "Days": return seconds / 86400
        default: return seconds
        }
    }

    var convertedVolumeValue: Double {
        let milliliters: Double

        // Step 1: Convert Input -> Milliliters
        switch inputVolumeUnit {
        case "Liters":  milliliters = inputVolumeValue * 1000
        case "Cups":    milliliters = inputVolumeValue * 240
        case "Pints":   milliliters = inputVolumeValue * 473.176
        case "Gallons": milliliters = inputVolumeValue * 3785.41
        default:        milliliters = inputVolumeValue
        }

        // Step 2: Convert Milliliters -> Output
        switch outputVolumeUnit {
        case "Liters":  return milliliters / 1000
        case "Cups":    return milliliters / 240
        case "Pints":   return milliliters / 473.176
        case "Gallons": return milliliters / 3785.41
        default:        return milliliters
        }
    }

    var body: some View {
        NavigationStack {
            Form {
                Section("Temperature Converter") {
                    HStack {
                        Text("From:").foregroundStyle(.secondary)
                        TextField("", value: $inputTempValue, format: .number)
                            .keyboardType(.decimalPad)
                            .focused($inputTempFocused)
                        Picker("", selection: $inputTempUnit) {
                            ForEach(tempUnits, id: \.self) { Text($0) }
                        }
                    }
                    HStack {
                        Text("To:").foregroundStyle(.secondary)
                        Text(convertedTempValue.formatted())
                        Picker("", selection: $outputTempUnit) {
                            ForEach(tempUnits, id: \.self) { Text($0) }
                        }
                    }
                }

                Section("Length Converter") {
                    HStack {
                        Text("From:").foregroundStyle(.secondary)
                        TextField("", value: $inputLengthValue, format: .number)
                            .keyboardType(.decimalPad)
                            .focused($inputLengthFocused)
                        Picker("", selection: $inputLengthUnit) {
                            ForEach(lengthUnits, id: \.self) { Text($0) }
                        }
                    }
                    HStack {
                        Text("To:").foregroundStyle(.secondary)
                        Text(convertedLengthValue.formatted())
                        Picker("", selection: $outputLengthUnit) {
                            ForEach(lengthUnits, id: \.self) { Text($0) }
                        }
                    }
                }

                Section("Time Converter") {
                    HStack {
                        Text("From:").foregroundStyle(.secondary)
                        TextField("", value: $inputTimeValue, format: .number)
                            .keyboardType(.decimalPad)
                            .focused($inputTimeFocused)
                        Picker("", selection: $inputTimeUnit) {
                            ForEach(timeUnits, id: \.self) { Text($0) }
                        }
                    }
                    HStack {
                        Text("To:").foregroundStyle(.secondary)
                        Text(convertedTimeValue.formatted())
                        Picker("", selection: $outputTimeUnit) {
                            ForEach(timeUnits, id: \.self) { Text($0) }
                        }
                    }
                }

                Section("Volume Converter") {
                    HStack {
                        Text("From:").foregroundStyle(.secondary)
                        TextField("", value: $inputVolumeValue, format: .number)
                            .keyboardType(.decimalPad)
                            .focused($inputVolumeFocused)
                        Picker("", selection: $inputVolumeUnit) {
                            ForEach(volumeUnits, id: \.self) { Text($0) }
                        }
                    }
                    HStack {
                        Text("To:").foregroundStyle(.secondary)
                        Text(convertedVolumeValue.formatted())
                        Picker("", selection: $outputVolumeUnit) {
                            ForEach(volumeUnits, id: \.self) { Text($0) }
                        }
                    }
                }
            }
            .navigationTitle("UnitFlip")
            .toolbar {
                ToolbarItemGroup(placement: .keyboard) {
                    Spacer()
                    Button("Done") {
                        inputTempFocused = false
                        inputLengthFocused = false
                        inputTimeFocused = false
                        inputVolumeFocused = false
                    }
                }
            }
        }
    }
}

#Preview {
    ContentView()
}
```