# Threads vs Coroutines in Kotlin

## 1. Threads vs Coroutines: Usage, Sample Code & Differences

### **Threads** (Traditional Approach)
```kotlin
fun main() {
    println("Main starts: ${Thread.currentThread().name}")
    
    // Creating a new thread (heavyweight)
    thread(name = "WorkerThread") {
        println("Thread work starts: ${Thread.currentThread().name}")
        Thread.sleep(2000) // BLOCKING call
        println("Thread work finished: ${Thread.currentThread().name}")
    }
    
    println("Main continues immediately")
    Thread.sleep(3000)
}
```

### **Coroutines** (Modern Approach)
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking { // Creates a coroutine scope
    println("Main starts: ${Thread.currentThread().name}")
    
    // Launching a coroutine (lightweight)
    launch {
        println("Coroutine work starts: ${Thread.currentThread().name}")
        delay(2000) // NON-BLOCKING suspension
        println("Coroutine work finished: ${Thread.currentThread().name}")
    }
    
    println("Main continues immediately")
    delay(3000)
}
```

### **Key Differences**

| Aspect | Threads | Coroutines |
|--------|---------|------------|
| **Creation Cost** | Heavy (1MB stack) | Light (few KB) |
| **Number** | Limited (1000s) | Millions possible |
| **Blocking** | `Thread.sleep()` blocks thread | `delay()` suspends coroutine only |
| **Context Switching** | Expensive (OS level) | Cheap (user level) |
| **Concurrency Model** | Preemptive | Cooperative |
| **Cancellation** | Complex | Built-in support |
| **Thread Usage** | 1 thread = 1 execution | Many coroutines can share 1 thread |

---

## 2. `coroutineScope()` Function

### Purpose: Structured Concurrency
Creates a new scope without completing until all launched children complete.

```kotlin
suspend fun fetchUserData(): String = coroutineScope {
    val userProfile = async { fetchProfile() }
    val userPosts = async { fetchPosts() }
    
    "Profile: ${userProfile.await()}, Posts: ${userPosts.await()}"
}
```

### Key Characteristics:
- **Structured**: All children must complete before returning
- **Cancellation**: If one child fails, all siblings are cancelled
- **Error propagation**: Exception in child cancels whole scope

---

## 3. Dispatchers: Default vs Main vs IO

### **Dispatchers.Default**
- For CPU-intensive work
- Thread pool size = CPU cores
```kotlin
CoroutineScope(Dispatchers.Default).launch {
    // Heavy computations, sorting, algorithms
    calculateFibonacci(1000)
}
```

### **Dispatchers.Main**
- For UI operations (Android, JavaFX)
- Main/UI thread access
```kotlin
CoroutineScope(Dispatchers.Main).launch {
    // Update UI safely
    textView.text = "Data loaded"
}
```

### **Dispatchers.IO**
- For I/O operations (network, file, DB)
- Creates more threads as needed
```kotlin
CoroutineScope(Dispatchers.IO).launch {
    // Network/file operations
    val data = downloadFile(url)
    withContext(Dispatchers.Main) {
        showData(data) // Switch to Main for UI
    }
}
```

### **Common Pattern: withContext Switching**
```kotlin
suspend fun loadAndDisplayData() {
    // Start on Main (optional)
    val result = withContext(Dispatchers.IO) {
        // Switch to IO for network
        apiService.fetchData()
    }
    // Automatically returns to original dispatcher (Main)
    updateUI(result)
}
```

---

## 4. `coroutineScope` with `delay()`

### Example: Concurrent Operations with Delays
```kotlin
suspend fun concurrentTasks() = coroutineScope {
    val job1 = launch {
        delay(1000)
        println("Task 1 completed")
    }
    
    val job2 = launch {
        delay(500)
        println("Task 2 completed")
    }
    
    // Both jobs run concurrently
    // coroutineScope waits for both to complete
}
```


    
    launch {
        delay(1000)
        println("This won't execute") // Cancelled due to sibling's exception
    }
}
```


### **Visualizing Your App Architecture:**

```
Main Thread (UI Operations)
├── Button clicks
├── Math operations  
└── Small logic

Background Thread Pool
├── Coroutine 1 (Dispatchers.IO) → File Upload
├── Coroutine 2 (Dispatchers.IO) → Network Operation
├── Coroutine 3 (Dispatchers.IO) → File Download
└── Coroutine 4 (Dispatchers.IO) → Database Queries
```

### **Best Practice Pattern:**
```kotlin
// ViewModel example
fun loadData() {
    viewModelScope.launch { // Parent in ViewModel
        // Show loading
        _isLoading.value = true
        
        try {
            coroutineScope { // Structured concurrency
                val userDeferred = async(Dispatchers.IO) { getUser() }
                val postsDeferred = async(Dispatchers.IO) { getPosts() }
                
                // Wait for both
                val user = userDeferred.await()
                val posts = postsDeferred.await()
                
                // Update UI on Main
                withContext(Dispatchers.Main) {
                    _userData.value = Pair(user, posts)
                }
            }
        } catch (e: Exception) {
            // Handle errors
        } finally {
            _isLoading.value = false
        }
    }
}
```

Coroutines provide **structured concurrency** where parent-child relationships ensure proper resource management and error handling, making async code safer and more maintainable than traditional threading.
