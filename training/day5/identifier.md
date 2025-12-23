# **Identifier Functions in Lambdas**

## **What is an Identifier Function?**
A **lambda with a single parameter** where you can omit the parameter declaration and use the implicit name `it`.

## **Basic Syntax**
```kotlin
// Regular lambda
val lambda1 = { x: Int -> x * 2 }

// Identifier function (using 'it')
val lambda2: (Int) -> Int = { it * 2 }
//          ^ Type needed when omitting parameter
```

## **When It Works:**
Only when **ONE parameter** exists:
```kotlin
// ✅ Single parameter
val square: (Int) -> Int = { it * it }

// ❌ Multiple parameters
val add = { it + it }  // Error: 'it' not available
val add = { a: Int, b: Int -> a + b }  // Must declare both
```

## **Examples:**

### **Collections (Common Use)**
```kotlin
val numbers = listOf(1, 2, 3)

numbers.map { it * 2 }        // [2, 4, 6]
numbers.filter { it % 2 == 0 } // [2]
numbers.forEach { println(it) } // Prints each
```

### **Custom Functions**
```kotlin
fun processNumber(action: (Int) -> String): String {
    return action(42)
}

// Using 'it'
val result = processNumber { "Value is $it" }  // "Value is 42"
```

### **Type Inference Required**
```kotlin
// Must specify type when using 'it'
val square = { it * it }           // ❌ Error: can't infer type
val square: (Int) -> Int = { it * it }  // ✅ Type specified
```

## **`it` vs Named Parameter:**
```kotlin
// With 'it' (concise)
list.filter { it > 5 }

// With named parameter (explicit)
list.filter { number -> number > 5 }

// Same function, different readability
```

## **When to Use `it`:**
- **Short, simple operations** (one-liners)
- **Clear context** (parameter type obvious)
- **Readability maintained** (doesn't obscure logic)

## **When NOT to Use `it`:**
- **Complex logic** needing descriptive names
- **Nested lambdas** (which `it` refers to?)
- **Multiple parameters** (can't use `it` anyway)

## **Nested Lambda Pitfall:**
```kotlin
listOfLists.flatMap { innerList ->
    innerList.map { it * 2 }  // Which 'it'? Inner list's elements
}
// Outer 'it' = innerList
// Inner 'it' = element of innerList
```

## **In Short:**
- `it` is the **implicit name** for a **single lambda parameter**
- Makes code **more concise** for simple operations
- Requires **type specification** when variable type isn't inferred
- Use judiciously - **named parameters** improve readability for complex logic
