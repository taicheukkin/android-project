# **Lambda Functions**

## **What is a Lambda?**
A **lambda** is an **anonymous function** (no name) that can be treated as a value.

## **Basic Syntax**
```kotlin
val lambdaName: (ParameterTypes) -> ReturnType = { parameters -> body }
// Simplified:
val lambdaName = { parameters -> body }
```

## **Key Characteristics:**
1. **No `fun` keyword** - uses `{ }` instead
2. **Last expression** is the return value (no `return` keyword)
3. **Can be passed** as arguments, stored in variables
4. **Single-expression** by nature

## **Examples:**
```kotlin
// 1. Basic lambda
val add = { a: Int, b: Int -> a + b }

// 2. Type inference
val multiply: (Int, Int) -> Int = { a, b -> a * b }

// 3. Single parameter - implicit `it`
val square: (Int) -> Int = { it * it }

// 4. No parameters
val greet = { println("Hello!") }

// 5. Multi-line
val process = { x: Int ->
    val doubled = x * 2
    doubled + 10
}
```

## **Common Uses:**
### **With Collections**
```kotlin
val numbers = listOf(1, 2, 3, 4)

// Map
val doubled = numbers.map { it * 2 }

// Filter  
val evens = numbers.filter { it % 2 == 0 }

// ForEach
numbers.forEach { println(it) }
```

### **As Function Parameters**
```kotlin
fun calculate(x: Int, y: Int, operation: (Int, Int) -> Int): Int {
    return operation(x, y)
}

// Usage
val result = calculate(5, 3) { a, b -> a + b }
```

### **Higher-Order Functions**
```kotlin
fun repeat(times: Int, action: (Int) -> Unit) {
    for (i in 0 until times) {
        action(i)
    }
}

repeat(3) { index ->
    println("Iteration $index")
}
```

## **Lambda vs Regular Function:**
| Lambda | Regular Function |
|--------|-----------------|
| `{ x -> x * 2 }` | `fun double(x: Int): Int = x * 2` |
| Anonymous, no name | Has a name |
| Can be stored in variables | Cannot be stored directly |
| Last expression returns | Explicit `return` or expression body |
| Implicit `it` for single param | Named parameters |

## **Trailing Lambda Syntax:**
```kotlin
// When last parameter is a lambda:
fun example(param: Int, action: () -> Unit) { }

// Can call with lambda outside parentheses:
example(5) { println("Trailing lambda") }
// Instead of:
example(5, { println("Regular") })
```

## **Capturing Variables:**
Lambdas can access variables from their surrounding scope:
```kotlin
val multiplier = 3
val multiplyBy = { x: Int -> x * multiplier }  // Captures 'multiplier'
```

## **Lambda vs Anonymous Function:**
```kotlin
// Lambda (preferred)
val lambda = { x: Int -> x * 2 }

// Anonymous function
val anonFun = fun(x: Int): Int { return x * 2 }
```

## **Key Benefits:**
1. **Conciseness** - less boilerplate code
2. **Functional programming** - enables map/filter/reduce
3. **Event handling** - onClick listeners, callbacks
4. **Deferred execution** - pass logic to be executed later
5. **DSL creation** - enables domain-specific languages

## **In Short:**
A lambda is a **short, anonymous function** that can be passed around and used for **concise, functional-style programming**.
