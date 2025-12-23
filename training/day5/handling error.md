# **Error Handling in Kotlin**

## **Core Philosophy:**
Kotlin promotes **explicit error handling** - no checked exceptions, but encourages handling errors at appropriate levels.

## **1. Basic Try-Catch-Finally**
```kotlin
try {
    val result = riskyOperation()
} catch (e: IOException) {
    // Handle specific exception
    println("IO Error: ${e.message}")
} catch (e: Exception) {
    // Handle general exception
    println("Error: ${e.message}")
} finally {
    // Always executes
    cleanup()
}
```

## **2. Try as Expression** (Kotlin-specific)
```kotlin
val result: ResultType = try {
    parseInput(input)  // Returns this if successful
} catch (e: NumberFormatException) {
    defaultValue       // Returns this on error
}

val number = try {
    "123".toInt()
} catch (e: NumberFormatException) {
    0  // Default value
}
```

## **3. `throw` Expression**
```kotlin
fun validate(age: Int) {
    if (age < 0) throw IllegalArgumentException("Invalid age")
    // Or as expression
    require(age >= 0) { "Age must be positive" }
}
```

## **4. Built-in Check Functions**
```kotlin
// Require - for argument validation
fun setScore(score: Int) {
    require(score in 0..100) { "Score must be 0-100" }
}

// Check - for state validation
fun withdraw(amount: Double) {
    check(amount <= balance) { "Insufficient funds" }
}

// Assert - for debugging (disabled unless -ea JVM flag)
assert(value > 0) { "Value should be positive" }

// Error/requireNotNull - for null checks
val name = requireNotNull(input) { "Input cannot be null" }
```

## **5. The `Result` Type** (Functional approach)
```kotlin
val result: Result<Int> = runCatching {
    "123".toInt()
}

// Working with Result
result.fold(
    onSuccess = { value -> println("Success: $value") },
    onFailure = { error -> println("Error: ${error.message}") }
)

// Alternative methods
result.getOrNull()        // Returns null on failure
result.getOrDefault(0)    // Returns default on failure
result.getOrElse { 0 }    // Returns lambda result on failure
result.exceptionOrNull()  // Returns exception or null
```

## **6. `runCatching` for Functional Error Handling**
```kotlin
val data = runCatching {
    fetchDataFromNetwork()
}.map { response ->
    parseResponse(response)
}.recover { error ->
    // Handle error and return alternative
    when (error) {
        is IOException -> getCachedData()
        else -> throw error  // Re-throw unknown errors
    }
}.getOrThrow()
```

## **7. Custom Exceptions**
```kotlin
class MyAppException(
    message: String,
    cause: Throwable? = null
) : Exception(message, cause)

// Usage
throw MyAppException("Custom error", originalException)
```

## **8. Kotlin vs Java Exception Handling**

| Aspect | Kotlin | Java |
|--------|--------|------|
| Checked exceptions | ❌ Not used | ✅ Used |
| Try-catch | Expression (returns value) | Statement |
| `throws` clause | ❌ Not needed | ✅ Required |
| Null safety | ✅ Built-in | ❌ Manual |

## **9. Best Practices**

### **Use Nullable Types Instead of Exceptions**
```kotlin
// Instead of throwing exception for "not found"
fun findUser(id: String): User?  // Returns null if not found

// Instead of:
fun findUser(id: String): User  // Throws if not found
```

### **Use Domain-Specific Result Types**
```kotlin
sealed class NetworkResult<out T> {
    data class Success<T>(val data: T) : NetworkResult<T>()
    data class Error(val message: String) : NetworkResult<Nothing>()
}

fun fetchData(): NetworkResult<String> {
    return try {
        NetworkResult.Success(apiCall())
    } catch (e: Exception) {
        NetworkResult.Error(e.message ?: "Unknown error")
    }
}
```

### **Avoid Catching Throwable**
```kotlin
try {
    // ...
} catch (e: Throwable) {  // ❌ Too broad
    // Catches everything including OutOfMemoryError
}

try {
    // ...
} catch (e: Exception) {  // ✅ Better
    // Catches only recoverable exceptions
}
```

## **10. Coroutine Exception Handling**
```kotlin
// Structured concurrency
val result = try {
    withTimeout(5000) {
        asyncOperation()
    }
} catch (e: TimeoutCancellationException) {
    // Handle timeout
}

// Coroutine exception handler
val handler = CoroutineExceptionHandler { _, exception ->
    println("Coroutine error: $exception")
}

GlobalScope.launch(handler) {
    throw IOException("Network failed")
}
```

## **Summary Table:**
| Approach | Use Case |
|----------|----------|
| `try-catch` | Immediate error handling, recover locally |
| `Result`/`runCatching` | Functional style, chainable operations |
| Nullable returns | For expected "not found" scenarios |
| Check functions (`require`, `check`) | Precondition validation |
| Custom exceptions | Domain-specific errors |
| Sealed class results | Type-safe error channels |

Kotlin's approach emphasizes **safety and explicitness** - making you think about errors but giving flexible tools to handle them appropriately.
