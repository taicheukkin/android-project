# Kotlin Scope Functions Summary

Scope functions (`let`, `run`, `with`, `apply`, `also`) execute a block of code on an object, providing temporary scope and simplifying code.

## Quick Comparison Table

| Function | Object Reference | Return Value | Use Case |
|----------|----------------|--------------|----------|
| **`let`** | `it` | Lambda result | Transform object, null safety |
| **`run`** | `this` | Lambda result | Configure object + compute result |
| **`with`** | `this` | Lambda result | Group function calls on object |
| **`apply`** | `this` | Context object | Initialize/configure objects |
| **`also`** | `it` | Context object | Side effects, logging, chaining |

## Detailed Breakdown

### 1. **`let`**
```kotlin
val result = object.let { 
    // it = object
    // do operations with 'it'
    transformedValue // returns lambda result
}
```
**Use**: Transform object, null-safe operations
```kotlin
val length = nullableString?.let { 
    println(it) // side effect
    it.length   // returns length
}
```

### 2. **`run`**
```kotlin
val result = object.run {
    // this = object
    // do operations with 'this'
    computedValue // returns lambda result
}
```
**Use**: Object configuration + computation
```kotlin
val result = service.run {
    port = 8080    // configure
    startService() // call method
    status         // return value
}
```

### 3. **`with`**
```kotlin
val result = with(object) {
    // this = object
    // do operations with 'this'
    computedValue // returns lambda result
}
```
**Use**: Group function calls (not extension function)
```kotlin
val description = with(person) {
    "$name, $age years old" // returns string
}
```

### 4. **`apply`**
```kotlin
val result = object.apply {
    // this = object
    // configure object
    // returns object itself (not lambda result)
}
```
**Use**: Object initialization/configuration
```kotlin
val button = Button().apply {
    text = "Click me"
    color = Color.RED
    onClick = { /* handler */ }
} // returns Button instance
```

### 5. **`also`**
```kotlin
val result = object.also {
    // it = object
    // perform side effects
    // returns object itself
}
```
**Use**: Side effects, logging, validation
```kotlin
val user = User().also {
    println("Creating user: $it")
    validateUser(it)
} // returns User instance
```

## Key Differences

### Return Value:
- **`apply`**, **`also`** → Return the **context object** itself
- **`let`**, **`run`**, **`with`** → Return **lambda result**

### Object Reference:
- **`this`** (`run`, `with`, `apply`) → Implicit receiver, can omit `this.` in calls
- **`it`** (`let`, `also`) → Explicit parameter, good for renaming

## Common Patterns

### 1. Null Safety Chain
```kotlin
val city = user?.address?.city?.let { 
    it.uppercase() 
}
```

### 2. Builder Pattern
```kotlin
val request = RequestBuilder().apply {
    url = "https://api.com"
    method = "GET"
    timeout = 5000
}.build()
```

### 3. Logging Chain
```kotlin
data.also { println("Received: $it") }
    .transform()
    .also { println("Transformed: $it") }
```

## Selection Guide

Choose based on what you need:

1. **Need to return modified object?** → `apply` (for configuration) or `also` (for side effects)
2. **Need to compute a result?** → `let`, `run`, or `with`
3. **Working with nullable?** → `let` (with safe call `?.`)
4. **Need implicit `this`?** → `run`, `with`, `apply`
5. **Need explicit `it`?** → `let`, `also`

## Mnemonic
- **ALso** and **Apply** return **A** (the object itself)
- **Let It** transform (uses `it`, returns result)
- **Run** with **this** receiver (implicit context)
- **With** is like run but not an extension

These functions reduce boilerplate, enable fluent APIs, and improve code readability when used appropriately.
