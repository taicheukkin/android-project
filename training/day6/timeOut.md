# **Timeout Function in Coroutines**

## **Overview**
Timeout functions allow you to **limit the execution time** of coroutines, automatically cancelling them if they exceed the specified duration.

---

## **1. `withTimeout()` - Throws TimeoutCancellationException**

### **Basic Usage:**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    try {
        val result = withTimeout(1300L) {
            repeat(1000) { i ->
                println("I'm sleeping $i...")
                delay(500L)
            }
            "Done" // Never reached
        }
        println("Result: $result")
    } catch (e: TimeoutCancellationException) {
        println("Timed out! ${e.message}")
    }
}
```

### **Behavior:**
- **Throws `TimeoutCancellationException`** when time expires
- **Cancels the coroutine** automatically
- **Exception propagates** up the coroutine hierarchy
- **Must be caught** if you don't want it to crash your coroutine

---

## **2. `withTimeoutOrNull()` - Returns Null on Timeout**

### **Safe Alternative:**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val result = withTimeoutOrNull(1300L) {
        repeat(1000) { i ->
            println("I'm sleeping $i...")
            delay(500L)
        }
        "Success" // Returned if completes in time
    }
    println("Result is: $result") // null if timeout
}
```

### **Behavior:**
- **Returns `null`** instead of throwing exception
- **Cleaner code** - no try-catch needed
- **Same cancellation mechanism** internally
- **Ideal for** cases where timeout is expected

---

## **3. Timeout with Resource Cleanup**

### **Proper Cleanup Example:**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    try {
        withTimeout(1000L) {
            try {
                repeat(10) { i ->
                    println("Processing $i...")
                    delay(300L)
                }
            } finally {
                // This will run even on timeout
                println("Cleaning up resources...")
                delay(200L) // Cleanup can take time
                println("Cleanup complete")
            }
        }
    } catch (e: TimeoutCancellationException) {
        println("Operation timed out")
    }
}
```

---

## **4. Nested Timeouts**

### **Different Timeout Levels:**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    withTimeout(3000L) { // Outer timeout: 3 seconds
        println("Starting outer operation")
        
        withTimeout(1000L) { // Inner timeout: 1 second
            delay(1500L) // This will timeout first
            println("Inner complete")
        }
        
        println("Outer continues") // Won't reach here
    }
}
```

---

## **Key Characteristics**

### **Timeout Cancellation:**
1. **Uses `TimeoutCancellationException`** (subclass of `CancellationException`)
2. **Cancellation is cooperative** - code must check cancellation
3. **Cleanup runs** in finally blocks
4. **Can be caught and handled** appropriately

### **Timeout vs Regular Cancellation:**
```kotlin
val job = launch {
    try {
        withTimeout(500) {
            delay(1000)
        }
    } catch (e: TimeoutCancellationException) {
        println("Specific timeout exception")
    } catch (e: CancellationException) {
        println("General cancellation")
    }
}
```

---

## **Best Practices**

### **1. Use `withTimeoutOrNull()` for Simplicity**
```kotlin
val data = withTimeoutOrNull(5000) {
    fetchFromNetwork() // Returns null if takes >5 seconds
}
if (data != null) {
    process(data)
}
```

### **2. Handle Cleanup Properly**
```kotlin
withTimeout(1000) {
    val resource = acquireResource()
    try {
        useResource(resource)
    } finally {
        releaseResource(resource) // Always runs
    }
}
```

### **3. Combine with Retry Logic**
```kotlin
suspend fun fetchWithRetry(maxRetries: Int = 3): String? {
    repeat(maxRetries) { attempt ->
        val result = withTimeoutOrNull(2000) {
            fetchData()
        }
        if (result != null) return result
        println("Attempt $attempt timed out, retrying...")
        delay(500 * (attempt + 1))
    }
    return null
}
```

### **4. Avoid Blocking Operations**
```kotlin
// BAD: Blocking call won't respect timeout
withTimeout(1000) {
    Thread.sleep(2000) // Will block for full 2 seconds!
}

// GOOD: Use delay() instead
withTimeout(1000) {
    delay(2000) // Will timeout after 1 second
}
```

---

## **Common Patterns**

### **1. Multiple Operations with Individual Timeouts**
```kotlin
suspend fun fetchUserData(userId: String): UserData {
    val profile = withTimeoutOrNull(1000) { fetchProfile(userId) }
    val orders = withTimeoutOrNull(2000) { fetchOrders(userId) }
    val preferences = withTimeoutOrNull(500) { fetchPreferences(userId) }
    
    return UserData(profile, orders, preferences)
}
```

### **2. Timeout with Fallback**
```kotlin
suspend fun loadData(): Data {
    return withTimeoutOrNull(3000) {
        fetchFromPrimarySource()
    } ?: fetchFromFallbackSource() // Use fallback on timeout
}
```

### **3. Timeout Monitoring**
```kotlin
val startTime = System.currentTimeMillis()
try {
    withTimeout(5000) {
        performLongOperation()
    }
} catch (e: TimeoutCancellationException) {
    val elapsed = System.currentTimeMillis() - startTime
    println("Operation timed out after ${elapsed}ms")
}
```

---

## **Important Considerations**

### **1. Timeout Granularity**
- Timeouts are **not real-time guarantees**
- **Minimum precision** depends on system
- **Cancellation checks** happen at suspension points

### **2. Non-cancellable Sections**
```kotlin
withTimeout(1000) {
    withContext(NonCancellable) {
        // This block won't be cancelled by timeout
        criticalCleanup()
    }
}
```

### **3. CPU-intensive Operations**
```kotlin
withTimeout(1000) {
    var i = 0
    while (i < 1_000_000_000) {
        i++
        ensureActive() // Check cancellation periodically
    }
}
```

### **4. Resource-intensive Timeouts**
```kotlin
// Setting many concurrent timeouts can be expensive
// Consider using a shared timer for many operations
```

---

## **Summary**

| Function | Returns | Throws | Use When |
|----------|---------|--------|----------|
| `withTimeout()` | T | `TimeoutCancellationException` | You need to handle timeout as error |
| `withTimeoutOrNull()` | T? | Nothing | Timeout is expected outcome |
| **Timeout value** | - | - | Should be reasonable for operation |
| **Cleanup** | - | - | Always runs in finally blocks |

### **Key Points:**
1. **Timeouts are cooperative** - code must check cancellation
2. **Always cleanup resources** in finally blocks
3. **Use `withTimeoutOrNull()`** for cleaner timeout handling
4. **Combine with retry logic** for robustness
5. **Avoid blocking calls** inside timeouts
