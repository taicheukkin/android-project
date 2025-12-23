# **CancellationException - Short Summary**

## **What is it?**
- **Normal termination** signal (not an error)
- Thrown on: `.cancel()`, timeout, parent failure

## **Usage Examples:**

### **1. Detect Cancellation for Cleanup:**
```kotlin
launch {
    try {
        repeat(100) { i ->
            delay(100)
            println(i)
        }
    } catch (e: CancellationException) {
        // 🔹 Cleanup resources before exit
        println("Cleaning up...")
        saveState()  // Final save
        closeConnections()
        throw e  // Must rethrow!
    }
}
```

### **2. Handle Timeout Gracefully:**
```kotlin
try {
    withTimeout(1000) {
        fetchData()  // Takes too long
    }
} catch (e: CancellationException) {
    // 🔹 Differentiate timeout from other cancellation
    if (e is TimeoutCancellationException) {
        println("Request timed out")
    }
    showFallbackUI()
}
```

### **3. Check Status in Loops:**
```kotlin
launch {
    for (item in items) {
        ensureActive()  // Throws CancellationException if cancelled
        
        // OR:
        if (!isActive) {
            // Handle cancellation without exception
            break
        }
        
        process(item)
    }
}
```

### **4. Ignore in Critical Sections:**
```kotlin
launch {
    try {
        criticalOperation()
    } catch (e: CancellationException) {
        // 🔹 Suppress for non-cancellable work
        withContext(NonCancellable) {
            mustCompleteCleanup()
        }
        throw e
    }
}
```

## **Key Rules:**
1. **Never swallow** - always rethrow after cleanup
2. **Use for resource cleanup** (close files, DB connections)
3. **Differentiate from real errors** (timeout vs crash)
4. **Not logged** by exception handlers

## **Simple Pattern:**
```kotlin
val job = launch {
    try {
        doWork()  // Main logic
    } catch (e: CancellationException) {
        cleanup()  // Optional cleanup
        throw e    // Required rethrow
    } catch (e: Exception) {
        handleError(e)  // Real error handling
    }
}

// Later...
job.cancel()  // Triggers CancellationException in coroutine
```
