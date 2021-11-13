# Usage

## Array of constraints

```swift linenums="1"
import Validation

let password = "12345"
let validator = Validator()

// An array of constraints
try validator.validate(
    password,
    against: [NotBlankConstraint(), LengthConstraint(min: 6, max: 16)]
)
```

## Variadic list of constraints

```swift linenums="6"
// A variadic list of constraints
try validator.validate(
    password,
    against: NotBlankConstraint(), LengthConstraint(min: 6, max: 16)
)
```

## Convenience API

```swift linenums="6"
// A convenience API for the existing constraints
try validator.validate(
    password,
    against: [.notBlank(), .length(min: 6, max: 16)]
)
```

## Custom constraint

```swift linenums="6"
// A custom validator
struct CustomValidator: ConstraintValidator {
    func validate(_ value: String, against constraints: [Constraint]) throws {
        // A validation logic here.
    }
}

// A custom constraint
struct CustomConstraint: Constraint {
    let validator: ConstraintValidator = CustomValidator()
}

// An array of constraints
try validator.validate(
    password,
    against: [NotBlankConstraint(), LengthConstraint(min: 6, max: 16), CustomConstraint()]
)
```

## Validation group

### Default group

```swift linenums="1"
import Validation

let group = Group.default
```

### Custom group

```swift linenums="1"
import Validation

let custom = Group(stringLiteral: "custom")
```

```swift linenums="3"
let custom: Group = "custom"
```

```swift linenums="3"
let custom = Group("custom")
```

### Validation using groups

```swift linenums="4"
try validator.validate(
    password,
    against: NotBlankConstraint(), LengthConstraint(min: 6, max: 16)
    on: [custom]
)
```

```swift linenums="4"
try validator.validate(
    password,
    against: NotBlankConstraint(), LengthConstraint(min: 6, max: 16)
    on: [.default, custom]
)
```
