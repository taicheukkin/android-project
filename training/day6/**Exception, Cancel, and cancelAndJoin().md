# **Exception, Cancel, and cancelAndJoin()**

## **Exception Handling in Coroutines**

### **Key Principles:**
- Uncaught exceptions cancel parent and sibling coroutines
- Exceptions propagate up the hierarchy
- **Exception propagation differs by builder:**
  - `launch` - throws exceptions immediately to parent
  - `async` - defers exceptions until `await()` is called

### **Handlers:**
```kotlin
val handler = CoroutineExceptionHandler { _, exception ->
    println("Caught: $exception")
}
```

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
