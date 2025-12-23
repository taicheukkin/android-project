# **Exception, Cancel, and cancelAndJoin()**

## **Exception Handling in Coroutines**

### **Key Principles:**
- Uncaught exceptions cancel parent and sibling coroutines
- Exceptions propagate up the hierarchy
- **Exception propagation differs by builder:**
  - `launch` - throws exceptions immediately to parent
  - `async` - defers exceptions until `await()` is called

# **Coroutine Exception Handler - Short Summary**

## **What is it?**
A **global catcher** for unhandled exceptions in coroutines.

## **Syntax:**
```kotlin
val handler = CoroutineExceptionHandler { _, exception ->
    println("Caught: $exception")
}
```

## **Usage:**
```kotlin
val scope = CoroutineScope(Dispatchers.IO + handler)

scope.launch {  // Uses handler
    throw RuntimeException("Boom!")
}
```

## **Key Rules:**
1. **Only works on root coroutines** (top-level `launch` or `async`)
2. **Doesn't catch** child coroutine exceptions directly
3. **Must be installed** in coroutine context
4. **Use with `SupervisorJob`** to isolate failures

## **When to use:**
- Global error logging
- Crash reporting
- Graceful failure handling

## **Simple Example:**
```kotlin
runBlocking {
    val handler = CoroutineExceptionHandler { _, e ->
        println("Global error: ${e.message}")
    }
    
    val job = GlobalScope.launch(handler) {
        throw Exception("Failed!")
    }
    
    job.join() // Prints: "Global error: Failed!"
}
```

## **SupervisorScope:**
```kotlin
supervisorScope {
    launch { /* Child 1 - failure won't cancel siblings */ }
    launch { /* Child 2 - isolated from Child 1 errors */ }
}
```

## **Key Differences:**
| Aspect | **Regular Scope** | **SupervisorScope** |
|--------|-------------------|---------------------|
| **Failure Propagation** | Child fails → cancels all | Child fails → only that child |
| **Error Handling** | Handler catches root failures | Each child handles own errors |
| **Use Case** | All-or-nothing tasks | Independent parallel tasks |

## **Handler + SupervisorScope Pattern:**
```kotlin
val handler = CoroutineExceptionHandler { _, e ->
    println("Root error: $e")
}

CoroutineScope(handler).launch {
    supervisorScope {  // Isolate children
        launch {
            throw Exception("Child 1 fails")  // Doesn't cancel others
        }
        launch {
            delay(100)
            println("Child 2 still runs!")  // ✅ Continues
        }
    }
}
```

## **When to Use:**
- **Handler**: Global error logging at root level
- **SupervisorScope**: Independent tasks (UI updates, parallel APIs)
- **Together**: Robust parallel processing with centralized error tracking
---

## **`cancel()` - Request Cancellation**

### **What it does:**
```kotlin
val job = launch {
    repeat(1000) { i ->
        println("Job: I'm sleeping $i...")
        delay(500)
    }
}
delay(1300)
job.cancel() // Request cancellation
```

### **Behavior:**
1. **Sets job state to "cancelling"**
2. **Throws `CancellationException`** inside the coroutine
3. **Does NOT wait** for coroutine to finish
4. **Children are cancelled** recursively
5. **Once cancelled, job cannot be restarted**

### **Important:**
- Cancellation is **cooperative** - coroutine must check cancellation status
- `cancel()` returns immediately (non-blocking)
- Blocking code won't respond unless explicitly checked

---

## **`cancelAndJoin()` - Cancel and Wait**

### **Combined Operation:**
```kotlin
val job = launch {
    repeat(1000) { i ->
        println("Job: I'm sleeping $i...")
        delay(500)
    }
}
delay(1300)
job.cancelAndJoin() // Cancel AND wait for completion
println("Main: Now I can quit")
```

### **What it does:**
1. **Calls `cancel()`** (requests cancellation)
2. **Calls `join()`** (waits for coroutine to finish)
3. **Returns only when** coroutine has completed cleanup/termination

---

## **Comparison Table**

| Aspect | `cancel()` | `cancelAndJoin()` | Exception |
|--------|------------|-------------------|-----------|
| **Behavior** | Requests cancellation | Requests cancellation + waits | Error propagation |
| **Blocking** | Non-blocking | Suspending (waits) | Varies |
| **Use Case** | Fire-and-forget cancellation | Ensure cleanup completes | Error handling |
| **Children** | Cancels recursively | Cancels recursively | May cancel parent |
| **Return** | Immediate | After coroutine ends | May throw |

---

## **Practical Examples**

### **1. Safe Cleanup with `cancelAndJoin()`**
```kotlin
val job = launch {
    try {
        repeat(1000) { i ->
            println("Processing $i")
            delay(500)
        }
    } finally {
        withContext(NonCancellable) {
            delay(1000) // Critical cleanup
            println("Cleanup completed")
        }
    }
}

delay(1300)
job.cancelAndJoin() // Wait for cleanup to finish
```

### **2. Exception vs Cancellation**
```kotlin
launch {
    try {
        delay(1000)
        throw RuntimeException("Error!") // Regular exception
    } catch (e: Exception) {
        println("Caught: ${e.javaClass.simpleName}")
    }
}

launch {
    try {
        delay(1000)
    } catch (e: Exception) {
        println("Cancelled: ${e.javaClass.simpleName}") // CancellationException
    }
}.cancel()
```

---

## **Best Practices**

### **Use `cancelAndJoin()` when:**
- You need to ensure cleanup completes
- Resources need proper disposal
- Orderly shutdown is required

### **Use `cancel()` when:**
- Immediate cancellation is acceptable
- No cleanup needed
- Fire-and-forget scenarios

### **Handle Exceptions with:**
- `try-catch` within coroutines
- `CoroutineExceptionHandler` for root coroutines
- `SupervisorJob` to isolate failures

---

## **Key Takeaways**

1. **Exceptions cancel** the coroutine hierarchy (unless supervised)
2. **`cancel()`** requests termination but doesn't wait
3. **`cancelAndJoin()`** ensures coroutine completes its termination
4. **Always check `isActive`** in long-running coroutines
5. **Clean up resources** in `finally` blocks or `NonCancellable` context

```kotlin
// Complete pattern
val job = launch {
    try {
        // Your work here
    } catch (e: CancellationException) {
        // Cancellation cleanup
        throw e // Re-throw
    } catch (e: Exception) {
        // Business exception handling
    } finally {
        // Final cleanup
    }
}

// Later...
job.cancelAndJoin() // Graceful shutdown
```
