# **Timeout - Short Summary**

## **Functions:**
- **`withTimeout(time)`** - Throws `TimeoutCancellationException` if timeout
- **`withTimeoutOrNull(time)`** - Returns `null` if timeout

## **Key Points:**
- Timeouts **cancel** coroutines automatically
- Must be **caught or handled** (`try-catch` or `null` check)
- **Cleanup runs** in finally blocks
- **Cooperative** - checks at suspension points

---

## **Timeout vs Delay Behavior:**

### **If timeout > delay time:**
```kotlin
withTimeout(2000) {    // 2 second timeout
    delay(1000)        // 1 second delay
    println("Success!") // ✅ EXECUTES - completes before timeout
}
```

### **If timeout < delay time:**
```kotlin
withTimeout(500) {     // 0.5 second timeout  
    delay(1000)        // 1 second delay
    println("Success!") // ❌ NEVER REACHED - timeout cancels first
}
```

## **Simple Rule:**
- **Timeout > delay** → Code **completes normally**
- **Timeout < delay** → Code **cancels with TimeoutCancellationException**

## **Example:**
```kotlin
val result = withTimeoutOrNull(1500) {
    delay(1000)       // Takes 1 second
    "Done"            // ✅ Returns "Done" - completes in time
}
// result = "Done"

val result2 = withTimeoutOrNull(500) {
    delay(1000)       // Takes 1 second  
    "Done"            // ❌ Never reached
}
// result2 = null (timeout after 500ms)
```

## **⚠️ CRITICAL: Never Use Thread.sleep() with Timeout**
```kotlin
// ❌ WRONG - WILL IGNORE TIMEOUT!
withTimeout(100) {
    Thread.sleep(5000)  // Blocks for 5 seconds anyway!
    println("This still prints after 5s")
}

// ✅ CORRECT - RESPECTS TIMEOUT
withTimeout(100) {
    delay(5000)  // Cancelled after 100ms
    println("Never reached")
}
```

## **Why?**
- `Thread.sleep()` = **blocking** Java thread sleep
- `delay()` = **suspending** coroutine delay
- **Timeouts only work** with suspending functions
- **Blocking calls ignore** coroutine cancellation

## **Better Pattern:**
```kotlin
withTimeoutOrNull(1000) {
    // Do suspending work
    delay(500)    // ✅ Will timeout if >1000ms
    // fetchData() // ✅ Suspend functions work
    // Thread.sleep(500) // ❌ Ruins timeout mechanism!
}
```

**Key:** Always use `delay()` or other **suspend functions** inside timeouts.
**Conclusion:** Coroutine **completes successfully** if it finishes **before** timeout expires.
