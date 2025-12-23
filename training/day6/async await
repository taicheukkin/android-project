## Async/Await in Kotlin

In Kotlin, the `async`/`await` pattern is implemented using coroutines. Unlike some languages that have direct `async`/`await` keywords, Kotlin uses suspending functions and structured concurrency.

## Basic Concept

### 1. **Coroutine Builders**
- `async` - Returns a `Deferred` (like a Promise/Future)
- `launch` - Fire-and-forget, returns a `Job`

### 2. **Suspending Functions**
Functions marked with `suspend` can be paused and resumed.

## Basic Examples

### **Example 1: Simple async/await**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    println("Start")
    
    // async starts a coroutine and returns a Deferred<T>
    val deferredResult: Deferred<Int> = async {
        delay(1000) // Simulate work
        42 // This is the result
    }
    
    // await() gets the result (suspends if not ready)
    val result: Int = deferredResult.await()
    println("Result: $result")
    
    println("End")
}
```

### **Example 2: Concurrent execution**
```kotlin
import kotlinx.coroutines.*
import kotlin.system.measureTimeMillis

fun main() = runBlocking {
    val time = measureTimeMillis {
        val deferred1 = async { fetchUserData() }
        val deferred2 = async { fetchUserProfile() }
        
        // Both tasks run concurrently
        val userData = deferred1.await()
        val userProfile = deferred2.await()
        
        println("User: $userData, Profile: $userProfile")
    }
    
    println("Completed in ${time}ms") // ~2000ms, not 3000ms
}

suspend fun fetchUserData(): String {
    delay(1000)
    return "User Data"
}

suspend fun fetchUserProfile(): String {
    delay(2000)
    return "User Profile"
}
```

### **Example 3: Error handling**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    try {
        val result = async { 
            mightFail() 
        }.await()
        
        println("Success: $result")
    } catch (e: Exception) {
        println("Failed: ${e.message}")
    }
}

suspend fun mightFail(): String {
    delay(500)
    if (System.currentTimeMillis() % 2 == 0L) {
        throw RuntimeException("Something went wrong")
    }
    return "Success"
}
```

## Advanced Patterns

### **Example 4: Structured concurrency**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    // Parent coroutine
    val parentJob = launch {
        // Child coroutines
        val userJob = async { getUser() }
        val postsJob = async { getPosts() }
        
        // Both must complete before continuing
        val user = userJob.await()
        val posts = postsJob.await()
        
        println("User: $user, Posts: ${posts.size}")
    }
    
    parentJob.join()
}

suspend fun getUser(): String {
    delay(1000)
    return "John Doe"
}

suspend fun getPosts(): List<String> {
    delay(1500)
    return listOf("Post 1", "Post 2", "Post 3")
}
```

### **Example 5: Lazy async**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    // Lazy start - only starts when await() is called
    val lazyDeferred = async(start = CoroutineStart.LAZY) {
        println("Starting computation...")
        delay(1000)
        "Lazy Result"
    }
    
    println("Before await")
    delay(2000) // Computation hasn't started yet
    
    val result = lazyDeferred.await() // Starts here
    println("Result: $result")
}
```

### **Example 6: Cancellation**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val job = launch {
        val deferred = async {
            repeat(10) { i ->
                println("Working $i...")
                delay(500)
            }
            "Done"
        }
        
        delay(1200) // Cancel after 1.2 seconds
        deferred.cancel() // Cancel the async task
    }
    
    job.join()
}
```

## Comparison with Other Languages

| Feature | Kotlin | JavaScript/TypeScript | C# |
|---------|---------|----------------------|-----|
| Keywords | `async()`/`await()` | `async`/`await` | `async`/`await` |
| Return type | `Deferred<T>` | `Promise<T>` | `Task<T>` |
| Error handling | try/catch | try/catch | try/catch |

## Best Practices

1. **Use structured concurrency**: Always launch coroutines within a scope
2. **Handle exceptions properly**: Wrap `await()` in try-catch
3. **Avoid global scope**: Use `coroutineScope` or custom scopes
4. **Consider cancellation**: Make coroutines cooperative with cancellation

### **Example 7: Using coroutineScope**
```kotlin
import kotlinx.coroutines.*

suspend fun fetchData(): Pair<String, List<String>> = coroutineScope {
    val userDeferred = async { getUser() }
    val postsDeferred = async { getPosts() }
    
    // If one fails, the other gets cancelled
    val user = userDeferred.await()
    val posts = postsDeferred.await()
    
    user to posts
}

// Helper functions from earlier...
```

### **Example 8: withContext for thread switching**
```kotlin
import kotlinx.coroutines.*
import kotlin.system.measureTimeMillis

fun main() = runBlocking {
    val result = withContext(Dispatchers.IO) {
        // Runs on IO thread pool
        delay(1000)
        "Data from network"
    }
    
    println(result) // Back on main thread (if on Android/UI)
}
```

## Key Points to Remember

1. **`async` returns a `Deferred<T>`** - a future/promise-like object
2. **`await()` is a suspending function** - doesn't block the thread
3. **Use `suspend` modifier** for functions that call other suspending functions
4. **Coroutines need a scope** - use `runBlocking`, `launch`, or `coroutineScope`
5. **Choose appropriate dispatchers** - `Dispatchers.IO`, `Dispatchers.Default`, `Dispatchers.Main`

This approach provides more flexibility and better error handling compared to traditional callback-based async programming.
