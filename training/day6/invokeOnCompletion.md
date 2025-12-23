# `invokeOnCompletion` in Kotlin Coroutines

`invokeOnCompletion` is a method available on coroutine `Job` and `Deferred` objects that allows you to register a callback that will be invoked when the coroutine completes, regardless of whether it completes normally, with an exception, or is cancelled.

## Basic Usage

### **On Job (launch)**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val job = launch {
        delay(1000)
        println("Task completed")
    }
    
    job.invokeOnCompletion { cause: Throwable? ->
        when {
            cause == null -> println("Job completed successfully")
            cause is CancellationException -> println("Job was cancelled")
            else -> println("Job failed with exception: ${cause.message}")
        }
    }
    
    delay(500)
    job.cancel() // Try commenting this out to see normal completion
}
```

### **On Deferred (async)**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val deferred = async {
        delay(1000)
        42
    }
    
    deferred.invokeOnCompletion { cause: Throwable? ->
        println("Deferred completed with cause: $cause")
    }
    
    try {
        val result = deferred.await()
        println("Result: $result")
    } catch (e: Exception) {
        println("Caught: ${e.message}")
    }
}
```

## Complete Examples

### **Example 1: Different completion scenarios**
```kotlin
import kotlinx.coroutines.*

suspend fun main() {
    // Scenario 1: Normal completion
    println("=== Normal Completion ===")
    val job1 = GlobalScope.launch {
        delay(100)
        println("Job1: Task done")
    }
    
    job1.invokeOnCompletion { cause ->
        println("Job1 completed: ${if (cause == null) "Successfully" else "With error: $cause"}")
    }
    job1.join()

    // Scenario 2: Cancellation
    println("\n=== Cancellation ===")
    val job2 = GlobalScope.launch {
        try {
            delay(1000)
            println("Job2: This won't print")
        } finally {
            println("Job2: Finally block executed")
        }
    }
    
    job2.invokeOnCompletion { cause ->
        println("Job2 completed: $cause")
        println("Is cancellation? ${cause is CancellationException}")
    }
    delay(100)
    job2.cancel()
    job2.join()

    // Scenario 3: Exception
    println("\n=== Exception ===")
    val job3 = GlobalScope.launch {
        delay(100)
        throw RuntimeException("Something went wrong!")
    }
    
    job3.invokeOnCompletion { cause ->
        println("Job3 completed: $cause")
    }
    delay(200) // Give time for exception to propagate
}
```

### **Example 2: Resource cleanup**
```kotlin
import kotlinx.coroutines.*
import java.io.File

fun main() = runBlocking {
    val file = File("temp.txt")
    
    val job = launch {
        file.writeText("Processing data...")
        println("File created")
        
        // Simulate some processing
        delay(2000)
        
        // This might not execute if job is cancelled
        file.delete()
        println("File deleted normally")
    }
    
    // Guaranteed cleanup
    job.invokeOnCompletion { cause ->
        if (file.exists()) {
            println("Cleaning up file in invokeOnCompletion")
            file.delete()
        }
        println("Cleanup complete. Cause: $cause")
    }
    
    delay(1000)
    job.cancel() // Try with and without this line
    job.join()
}
```

### **Example 3: Multiple invokeOnCompletion handlers**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val job = launch {
        delay(1000)
        println("Main task")
    }
    
    // Multiple handlers can be registered
    job.invokeOnCompletion { 
        println("Handler 1: Job completed")
    }
    
    job.invokeOnCompletion {
        println("Handler 2: Cleanup started")
    }
    
    job.invokeOnCompletion { cause ->
        if (cause != null) {
            println("Handler 3: Error occurred - ${cause.message}")
        }
    }
    
    delay(500)
    job.cancelAndJoin()
}
```

### **Example 4: Using invokeOnCompletion with async/await**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val deferred = async {
        delay(500)
        if (System.currentTimeMillis() % 2 == 0L) {
            throw RuntimeException("Random failure!")
        }
        "Success"
    }
    
    deferred.invokeOnCompletion { cause ->
        println("Deferred completed. Cause: ${cause?.message ?: "None"}")
    }
    
    try {
        val result = deferred.await()
        println("Result: $result")
    } catch (e: Exception) {
        println("Caught exception: ${e.message}")
    }
}
```

### **Example 5: With custom CoroutineExceptionHandler**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val exceptionHandler = CoroutineExceptionHandler { _, exception ->
        println("CoroutineExceptionHandler caught: ${exception.message}")
    }
    
    val job = launch(exceptionHandler) {
        println("Starting task")
        delay(100)
        throw RuntimeException("Test exception")
    }
    
    job.invokeOnCompletion { cause ->
        println("invokeOnCompletion: ${cause?.message}")
    }
    
    job.join()
}
```

## Important Parameters and Behavior

### **Signature:**
```kotlin
fun invokeOnCompletion(
    onCancelling: Boolean = false,
    invokeImmediately: Boolean = true,
    handler: CompletionHandler
)
```

### **Example 6: Using parameters**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    // 1. invokeImmediately = false
    val job1 = launch {
        delay(1000)
    }
    
    job1.invokeOnCompletion(invokeImmediately = false) { 
        println("Handler 1 - Only called on completion")
    }
    
    println("Job1 is active: ${job1.isActive}") // true
    job1.cancelAndJoin()
    
    // 2. onCancelling = true
    println("\n--- onCancelling example ---")
    val job2 = launch {
        delay(2000)
        println("Job2 completed normally")
    }
    
    job2.invokeOnCompletion(onCancelling = true) { cause ->
        println("Handler called with cause: $cause")
        println("Job is cancelling: ${cause is CancellationException}")
    }
    
    delay(500)
    job2.cancel("Manual cancellation")
    job2.join()
}
```

### **Example 7: Practical use case - tracking job lifecycle**
```kotlin
import kotlinx.coroutines.*
import java.util.concurrent.atomic.AtomicInteger

class TaskManager {
    private val activeTasks = AtomicInteger(0)
    
    fun launchTask(scope: CoroutineScope, block: suspend () -> Unit): Job {
        val job = scope.launch {
            activeTasks.incrementAndGet()
            println("Active tasks: ${activeTasks.get()}")
            
            try {
                block()
            } finally {
                activeTasks.decrementAndGet()
                println("Task ended. Active tasks: ${activeTasks.get()}")
            }
        }
        
        // Backup tracking in case of abrupt cancellation
        job.invokeOnCompletion {
            if (activeTasks.get() > 0) {
                activeTasks.decrementAndGet()
                println("Cleanup: Active tasks: ${activeTasks.get()}")
            }
        }
        
        return job
    }
}

fun main() = runBlocking {
    val manager = TaskManager()
    
    val jobs = List(5) { index ->
        manager.launchTask(this) {
            delay((index * 500).toLong())
            println("Task $index completed")
        }
    }
    
    // Cancel some tasks
    delay(800)
    jobs[2].cancel()
    
    jobs.forEach { it.join() }
}
```

## Key Points

1. **Invocation Guarantee**: The handler is **always** called when the coroutine completes
2. **Exception Handling**: 
   - `null` cause → successful completion
   - `CancellationException` → normal cancellation
   - Other `Throwable` → failure
3. **Thread Safety**: The handler can be called from any thread
4. **Multiple Handlers**: Multiple `invokeOnCompletion` calls can be registered
5. **Timing**: By default, handlers are invoked immediately if the job is already complete

## Common Use Cases

1. **Resource cleanup** (closing files, database connections)
2. **Logging and monitoring** job completion
3. **Updating UI** when background tasks complete
4. **Graceful shutdown** of services
5. **Tracking task lifecycle** in complex applications

## Comparison with `finally` block

| Feature | `finally` block | `invokeOnCompletion` |
|---------|----------------|---------------------|
| When called | Only when coroutine runs | Always, even if cancelled before start |
| Exception access | No | Yes (via `cause` parameter) |
| Multiple handlers | No | Yes |
| Thread context | Same as coroutine | Can be different |
| Order guarantee | After try/catch | No guaranteed order |

Use `invokeOnCompletion` when you need guaranteed cleanup regardless of how the coroutine ends, especially when you need to handle resources outside the coroutine's scope.
