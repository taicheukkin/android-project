# Coroutine Execution Patterns - Quick Summary

## 1. **Sequential (Default)**
```kotlin
// One after another
suspend fun sequential() {
    val result1 = doTask1()  // Waits for completion
    val result2 = doTask2()  // Then starts
}
```
**Key:** Suspend functions run sequentially by default. Each waits for previous to finish.

## 2. **Parallel (Concurrent)**
```kotlin
// Multiple tasks at once
suspend fun parallel() = coroutineScope {
    val deferred1 = async { doTask1() }
    val deferred2 = async { doTask2() }
    
    val results = awaitAll(deferred1, deferred2)
}
```
**Key:** Use `async` for concurrent execution + `await()`/`awaitAll()` for results.

## 3. **Lazy Execution**
```kotlin
// Deferred until needed
suspend fun lazyExample() = coroutineScope {
    val lazyJob = async(start = CoroutineStart.LAZY) {
        expensiveComputation()
    }
    
    // Job hasn't started yet
    // ...
    
    lazyJob.await()  // Starts here, then waits for result
}
```
**Key Characteristics:**
- **Starts only when needed** (first `await()` or `start()` call)
- **Computes once** (cached result)
- **Useful for expensive operations** you might not need

## Quick Comparison:

| Pattern | When to Use | Performance |
|---------|-------------|-------------|
| **Sequential** | Tasks depend on each other | Slower (serial) |
| **Parallel** | Independent tasks, need speed | Faster (concurrent) |
| **Lazy** | Expensive, maybe-needed tasks | Efficient (on-demand) |

## Common Pattern: Parallel + Sequential Combo
```kotlin
suspend fun loadUserData() = coroutineScope {
    // Parallel: Load independent data
    val profile = async { fetchProfile() }
    val settings = async { fetchSettings() }
    
    // Sequential: Process after both complete
    val user = User(profile.await(), settings.await())
    saveToDatabase(user)
}
```

**Rule of thumb:** 
- Default to sequential (simpler)
- Use parallel for independent work 
- Use lazy for expensive optional operations
