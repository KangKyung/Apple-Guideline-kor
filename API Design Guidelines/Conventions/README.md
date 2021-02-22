# Conventions

## General Conventions

* **Document the complexity of any computed property that is not O(1).**
<br>

* **Prefer methods and properties to free functions.**
<br>

    1. When there’s no obvious self: `min(x, y, z)`
    2. When the function is an unconstrained generic: `print(x)`
    3. When function syntax is part of the established domain notation: `sin(x)`

* **Follow case conventions.** 
<br>test

    test
<br>test

    ```swift
    var utf8Bytes: [UTF8.CodeUnit]
    var isRepresentableAsASCII = true
    var userSMTPServer: SecureSMTPServer
    ```

    test
<br>test

    ```swift
    var radarDetector: RadarScanner
    var enjoysScubaDiving = true
    ```
    
* **Methods can share a base name**
<br>test

    test

    ```swift
    ✅
    
    extension Shape {
        /// Returns `true` iff `other` is within the area of `self`.
        func contains(_ other: Point) -> Bool { ... }

        /// Returns `true` iff `other` is entirely within the area of `self`.
        func contains(_ other: Shape) -> Bool { ... }

        /// Returns `true` iff `other` is within the area of `self`.
        func contains(_ other: LineSegment) -> Bool { ... }
    }
    ```

    test

    ```swift
    ✅

    extension Collection where Element : Equatable {
        /// Returns `true` iff `self` contains an element equal to
        /// `sought`.
        func contains(_ sought: Element) -> Bool { ... }
    }
    ```
    
    test

    ```swift
    ❌

    extension Database {
        /// Rebuilds the database's search index
        func index() { ... }

        /// Returns the `n`th row in the given table.
        func index(_ n: Int, inTable: TableID) -> TableRow { ... }
    }
    ```

    test

    ```swift
    extension Box {
        /// Returns the `Int` stored in `self`, if any, and
        /// `nil` otherwise.
        func value() -> Int? { ... }

        /// Returns the `String` stored in `self`, if any, and
        /// `nil` otherwise.
        func value() -> String? { ... }
    }
    ```

<br>

## Parameters
`func move(from start: Point, to end: Point)`

* **Choose parameter names to serve documentation.** 
<br>test

    test
<br>test

    ```swift
    ✅

    /// Return an `Array` containing the elements of `self`
    /// that satisfy `predicate`.
    func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]

    /// Replace the given `subRange` of elements with `newElements`.
    mutating func replaceRange(_ subRange: Range, with newElements: [E])
    ```

    test

    ```swift
    ❌

    /// Return an `Array` containing the elements of `self`
    /// that satisfy `includedInResult`.
    func filter(_ includedInResult: (Element) -> Bool) -> [Generator.Element]

    /// Replace the range of elements indicated by `r` with
    /// the contents of `with`.
    mutating func replaceRange(_ r: Range, with: [E])
    ```

* **Take advantage of defaulted parameters.**
<br>test

    test
<br>test

    ```swift
    ❌

    let order = lastName.compare(
        royalFamilyName, options: [], range: nil, locale: nil)
    ```

    test

    ```swift
    ✅

    let order = lastName.compare(royalFamilyName)
    ```

    test
    ```swift
    ✅

    extension String {
        /// ...description...
        public func compare(
            _ other: String, options: CompareOptions = [],
            range: Range? = nil, locale: Locale? = nil
        ) -> Ordering
    }
    ```

    test

    ```swift
    ❌
    
    extension String {
        /// ...description 1...
        public func compare(_ other: String) -> Ordering
        /// ...description 2...
        public func compare(_ other: String, options: CompareOptions) -> Ordering
        /// ...description 3...
        public func compare(
            _ other: String, options: CompareOptions, range: Range) -> Ordering
        /// ...description 4...
        public func compare(
            _ other: String, options: StringCompareOptions,
            range: Range, locale: Locale) -> Ordering
    }
    ```

    test
<br>test

* **Prefer to locate parameters with defaults toward the end**
<br>test

<br>

## Argument Labels
```swift
func move(from start: Point, to end: Point)
x.move(from: x, to: y) 
```

* **Omit all labels when arguments can’t be usefully distinguished**
<br>ex> `min(number1, number2)`, `zip(sequence1, sequence2)`

* **In initializers that perform value preserving type conversions, omit the first argument label**
<br>ex> `Int64(someUInt32)`

    test
    ```swift
    extension String {
        // Convert `x` into its textual representation in the given radix
        init(_ x: BigInt, radix: Int = 10)   ← Note the initial underscore
    }

    text = "The value is: "
    text += String(veryLargeNumber)
    text += " and in hexadecimal, it's"
    text += String(veryLargeNumber, radix: 16)
    ```

    test
    ```swift
    extension UInt32 {
        /// Creates an instance having the specified `value`.
        init(_ value: Int16)         // ← Widening, so no label
        /// Creates an instance having the lowest 32 bits of `source`.
        init(truncating source: UInt64)
        /// Creates an instance having the nearest representable
        /// approximation of `valueToApproximate`.
        init(saturating valueToApproximate: UInt64)
    }
    ```

    test
<br>test

    test

* **When the first argument forms part of a prepositional phrase, give it an argument label.** 
<br>test
<br>ex> `x.removeBoxes(havingLength: 12)`

    test
    ```swift
    ❌

    a.move(toX: b, y: c)
    a.fade(fromRed: b, green: c, blue: d)
    ```

    test

    ```swift
    ✅

    a.moveTo(x: b, y: c)
    a.fadeFrom(red: b, green: c, blue: d)
    ```

* **Otherwise, if the first argument forms part of a grammatical phrase, omit its label**
<br>test
<br> ex> `x.addSubview(y)`

    test

    ```swift
    ✅

    view.dismiss(animated: false)
    let text = words.split(maxSplits: 12)
    let studentsByName = students.sorted(isOrderedBefore: Student.namePrecedes)
    ```

    test

    ```swift
    ❌

    view.dismiss(false)   Don't dismiss? Dismiss a Bool?
    words.split(12)       Split the number 12?
    ```
    
    test

* **Label all other arguments.**