# Swift Style Guide

Code style guidelines for swift projects.
This style guide is based on the Swift standard library style and takes inspiration from other popular guides.

* [Swift.org - API Design Guidelines](https://www.swift.org/documentation/api-design-guidelines/)

* [Swift Standard Library Reference](https://developer.apple.com/documentation/swift)

# Fundamentals

* **Clarity is more important than brevity**: Brevity is not a primary goal. Code should be made more concise only if other good qualities - such as readability, simplicity, and clarity - remain equal or are improved.

* **Documentation is your friend**: Your future self and your teammates will thank you for writing clear documentation for your declarations. Include relevant information such as links to decision making & articles when additional context may be needed. Be judicious with in-line comments. [DocC](https://www.swift.org/documentation/docc/)

* **Don't fight the tools**: Xcode is considered the primary IDE for swift development. As such, we want to stick to as many default behaviors and expectations as possible.

# Table of Contents

* [Source Files](#source-files)
  
  * [Organization](#organization)
  
  * [File Names](#file-names)
  
  * [File Comments](#file-comments)
  
  * [Import Statements](#import-statements)
  
  * [Line Width](#line-width)
  
  * [Indentation](#indentation)
  
  * [Trailing Whitespace](#trailing-whitespace)
  
  * [Braces](#braces)

* [Type Declarations](#type-declarations)
  
  * [Naming](#naming)
  
  * [Attributes](#attributes)
  
  * [Line Wrapping](#line-wrapping)
  
  * [Generics](#generics)
  
  * [Inference](#inference)
  
  * [Comments](#comments)

* [Logic and Control Flow](#logic-and-control-flow)
  
  * [Guard](#guard)
  
  * [Defer](#defer)
  
  * [Unwrapping Optionals](#unwrapping-optionals)
  
  * [Ternary Operator](#ternary-operator)

---

## Source Files

### Organization

Folder structure can go a long way in helping to understand the purpose of a file. But, extensive structures become unmanageable and hard to navigate visually. Try to limit folder depth to two subfolders (from the root of the target). For example:

Swift Packages:

```
/ #Repo
  Sources/
    Target/ # Target Root
      {Grouping}/
        {Component}/
```

iOS Projects:

```
/ #Repo
  {Project Name}/ # Target Root
    {Views}/
      {Feature}/
```

### File Names

All Swift source files end with the extension `.swift`

In general, the name of a source file best describes the primary entity that it contains. A file that primarily contains a single type has the name of that type. A file that extends an existing type with protocol conformance is named with a combination of the type name and the protocol name, joined with a plus (`+`) sign. A file that provides a name-spaced subtype is named with a combination of the parent name and subtype name, joined with a period (`.`). For more complex situations, exercise your best judgment. For Example:

* A file containing a single type `AwesomeProtocol` is named: `AwesomeProtocol.swift`

* A file containing a single extension to the type `RadClass` that adds conformance to the protocol `AwesomeProtocol` is named: `RadClass+AwesomeProtocol.swift`.

* A file containing multiple extensions to the type `PowerfulStruct` is suffixed with the target in which the file is a member. If within the declaring package `HelperTools` the file would be named: `PowerfulStruct+HelperTools.swift`. If extending the definition within the local project `BirdWatcher`, the file would be named: `PowerfulStruct+BirdWatcher.swift`.

* A file containing a name-spaced type `Details` under the type `Appointment` would be named: `Appointment.Details.swift`.

* A file containing a single extension to the name-spaced `Patient.Chart` that adds conformance to the protocol `ExpressibleAsDictionary` would be named: `Patient.Chart+ExpressibleAsDictionary.swift`.

### File Comments

By default Xcode places the following comment block at the top of new Swift files:

```swift
//
//  {File Name}.swift
//  {Project Name}
//
//  Created by {Creator Name} on {Todays Date}.
//
```

**This is entirely a waste of space; remove them.**

* The file & project name can easily become out of sync during refactors. Also, this information is readily apparent within the context of folder structures and IDE information.

* The creator and date information is better represented through the source-control history.

### Import Statements

A source file imports exactly the top-level modules that it needs; nothing more and nothing less. Imports of whole modules are preferred to imports of individual declarations or submodules.

Specifically on Apple platforms, import only the modules a source file requires. For example, don't import `UIKit` when importing `Foundation` will suffice. Likewise, don't import `Foundation` if you must import `UIKit`.

There is no 'strict' guide for the ordering of imports and the compiler should be able to handle any order; the key should be to remain consistent within the project. Strategies that _could_ be used are:

* **Alphabetical**: All imports are ordered lexicographically (A to Z)

* **Order of Reference**: Imports are added as needed (typically automatically by the IDE)

* **Broad to Narrow**: Imports are order with the most broad to narrow usage:
  
  * `import Foundation` // One step above the 'standard library'
  
  * `import CoreGraphics` // A system library
  
  * `import Toolkit` // Some swift package
  
  * `@testable import Product` // Module undergoing testing

### Line Width

We live in a modern computing environment and wide screens are common. There is no need to stick to an _ancient_ 80 character guide. That being said, a max of around 160 characters should be respected with a few exceptions:

* Lines where obeying the column limit is not possible without breaking a meaningful unit of text that should not be broken (for example, a long URL in a comment).

* Code generated by another tool.

In general type initializers and functions will use a single line when under the character limit, except when clarity can ultimately be improved by adding line breaks.

### Indentation

Xcode automatically uses four (4) spaces to represent a tab in Swift files. JSON files and multiline literals can use two (2) space indentation.

### Trailing Whitespace

In general, whitespace should be removed from lines. But, indentation can be maintained between blocks of code within a type declaration. Again this follows the default settings in Xcode:

* [x] Automatically trim trailing whitespace
* [ ] Including whitespace-only lines

### Braces

In general, an opening brace (`{`) begins at the end of one line, and the matching closing brace (`}`) can be found at the opening line indentation level following a line break. Presented another way:

```swift
func example() {
    if condition {
    } else {
    }
}
```

There are several exceptions to this rule:

* **Empty Blocks**:
  
  It is common when prototyping of defining locally scoped types to not have an implementation. Under these conditions a closing brace can follow the opening brace:
  
  ```swift
  protocol FancyLad {}
  ...
  func example() throws {
      struct GenericError: Error {}
  
      throw GenericError()
  }
  ```

* **Property Requirements**:
  A protocol can require any conforming type to provide an instance property or type property with a particular name and type. The protocol doesn’t specify whether the property should be a stored property or a computed property—it only specifies the required property name and type. The protocol also specifies whether each property must be gettable or gettable *and* settable.
  
  ```swift
  protocol Picture {
      var monochrome: Bool { get }
      var printSize: Size { get set }
  }
  ```

* **Closure Expressions**:
  
  Methods that take a closure expression may often be short-handed to an inline function and often chained. For instance the Swift library provides a method called `sorted(by:)` which sorts an array of values of a know type based on the output of the closure provided.
  
  ```swift
  let names: [String] = ["Alice", "Sue", "Bob", "Niles"]
  let sorted = names.sorted { $0 < $1 }.map { $0.uppercased() }
  // sorted: ["ALICE", "BOB", "NILES", SUE"]
  ```

* **Computed Properties**:
  
  Often computed properties are simple mutations or logic based on another value.
  
  ```swift
  var age: Int
  var isToddler: Bool { age <= 3 }
  ```
  
  Prefer computed properties over empty parameter functions. A property that `throws`, `async` or `async throws` may be an exception, and should use best judgement in context.
  
  ```swift
  // Not-Preferred
  func isAvailable() -> Bool {
  }
  
  // Preferred
  var isAvailable: Bool {
  }
  
  // Exception (Best Judgment) - Throwing
  func isMaybeAvailable() throws -> Bool {
  }
  
  var isMaybeAvailable: Bool {
      get throws {
          try calcuateAvailability()
      }
  }
  
  // Exception (Best Judgment) - Async
  func isMaybeAvailable() async -> Bool {
  }
  
  var isMaybeAvailable: Bool {
      get async {
          await calcuateAvailability()
      }
  }
  ```

* **Setters/Getters**:
  
  Similar to the property requirements of a protocol, when setters and getters are defined for a type property, matching braces on a single line can be used for brevity.
  
  ```swift
  var protectedProperty: Int {
      get { _secureStorage.value }
      set { _secureStorage.value = newValue }
  }
  ```

## Type Declarations

### Naming

* Use 'PascalCase' for type and protocol names, and 'lowerCamelCase' for everything else.

* Acronyms are typically all-caps except when it comes at the start of a name that would otherwise be 'lowerCamelCase'.

* Booleans should include a verb prefix - `is`, `has`, `will`, `did` - to help make it clear the property is not another type.

### Attributes

There are two kinds of attributes in Swift—those that apply to declarations and those that apply to types. An attribute provides additional information about the declaration or type. For example, the discardableResult attribute on a function declaration indicates that, although the function returns a value, the compiler shouldn’t generate a warning if the return value is unused.

You specify an attribute by writing the @ symbol followed by the attribute’s name and any arguments that the attribute accepts:

```swift
@{attribute name}{(attribute arguments)}
```

For consistency, attributes should be declared on the same line as the rest of the type definition (that is unless the addition of the attributes causes the line length to exceed reasonable limits. _But, then you have other problems to consider._):

```swift
@MainActor @discardableResult func epicTaskOnTheMainThread() -> Bool {
}
...
// Not
@MainActor
@discardableResult
func notSoEpicTaskOnTheMainThread() -> Bool {
}
```

There is an exception to this rule, and that is the `@availabe` attribute:

```swift
@available(*, deprecated, renamed: "otherFunction()")
func thisFunction() {}


@available(macOS 12.0, iOS 15.0, tvOS 13.05, watchOS 8.0, *)
func usesLanguageFeaturesOnlyAvailableFromAPoint() {}
```

### Line Wrapping

In general, if a declaration fits on a single line, keep it on a single line. When multiple lines are needed or in-order to increase clarity, default indentation and bracing rules should be followed:

```swift
func generateStringFunction(
    _ initialValue: String,
    truncation: TruncationStrategy = .bothEnds,
    casing: CasingStrategy = .uppercase,
    fragmentParsing: (String) -> String
) throws -> String {
}
```

### Generics

Generic type parameters should be descriptive but when a type name doesn't have a meaningful relationship or role, the use of a traditional single letter is acceptable.

```swift
struct Egg<CookingMethod> { }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
```

### Inference

In general, let the compile do its thing and handle type inference for constants or variables. But add type declarations where clarity and comprehension are needed or required, such as empty array or dictionary initialization.

```swift
let value = 23.7
let names = ["Bob", "Mark", "Sue"]
let result: String = genericFunction()
var dictionary: [String: String] = [:]
```

### Comments

Comment blocks should use single line comments (`//` for inline comments and `///` for documentation comments). This is preferred over c-style comment blocks (`/* ... */`). When they are needed, use comments to explain why a particular piece of code does something. Comments should be kept up-to-date or deleted. Avoid block comments inline with code, as the code should be as self-documenting as possible.

Documentation comments begin with a brief **single-sentence** summary that describes the declaration. If more detail is needed than can be stated in the summary, additional paragraphs (each separated by a blank line) are added after it. Clearly document the parameters, return value, and thrown errors of functions using the `parameters`, `returns`, and `throws` tags.

```swift
/// Performs a network `Request` and decodes the response to a known type.
///
/// This will first perform the request, then attempt any decoding.
///
/// - parameters:
///   - request: The details of the request to perform.
///   - decoder: The `JSONDecoder` that should be used to deserialize the result data.
/// - returns: The decoded `Response` value.
/// - throws: `NetworkingError` or `DecodingError`.
func performRequest<Value>(_ request: Request, using decoder: JSONDecoder = JSONDecoder()) async throws -> Value where Value: Decodable {
}
```

## Logic and Control Flow

### Guard

A `guard` statement, compared to an `if` statement with an inverted condition, provides visual emphasis that the condition being tested is a special case that causes early exit from the enclosing scope.

Furthermore, `guard` statements improve readability by eliminating extra levels of nesting (the “pyramid of doom”); failure conditions are closely coupled to the conditions that trigger them and the main logic remains flush left within its scope.

Single line `guard` statements should be avoided primarily for consistency but also for clarity in calling out the conditional check. `guard` requires an `else`, there is often additional code executed within the method, so for consistency all `guard` statements should be written the same way.

```swift
func calculateTotal(amount: Double) throws -> Double {
    guard amount > 0.0 else {
        throw NegativeAmount()
    }

    return amount * 1.5
}
```

### Defer

Similar to `guard`, single line defers should be avoided to be consistent throughout a codebase.

```swift
func feedCat(dish: ServingDish) {
    let can = retrieveCan()
    defer {
        discard(can)
    }

    let food = can.open()
    dish.plop(food)
}
```

### Unwrapping Optionals

Forced unwrapping should be handled with care.

When unwrapping multiple optionals with `if let` or `guard let` syntax, it may be preferred to express them on a single line (`if`) or in multiple statements (`guard`), or possible using another control flow means. This is due to the odd line indentation and brace matching that can occur when spanning multiple lines. (Swift 5.7 will make this easier with the new `if let` shorthand.) Also, there can be a benefit of reduced cognitive load when processing the code. For example:

```swift
func shouldPetAnimal(animal: Animal?) throws -> Bool {
    guard let animal = animal else {
        throw NoAnimal()
    }

    guard animal.isAlive && !animal.isPosionous else {
        throw AreYouSure()
    }

    if let name = animal.name, animal.willRespondToName {
        return animal.callWithName(name)
    }

    return coinFlip()
}
```

### Ternary Operator

The Ternary operator - `?:` - should only be used to evaluate a single condition and when it increases clarity or code neatness.

---

## References

- [Google Swift Style Guide](https://google.github.io/swift/)

- [Airbnb Swift Style Guide](https://github.com/airbnb/swift)

- [Ray Wenderlich Swift Style Guide](https://github.com/raywenderlich/swift-style-guide)
