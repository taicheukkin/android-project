# Local vs Global Coroutine Scope

## Key Differences

### **Global Scope (DANGEROUS - Avoid if possible)**
```kotlin
import kotlinx.coroutines.*

// ❌ GLOBAL - BAD PRACTICE (usually)
fun main() {
    // GlobalScope.launch creates a coroutine that lives for the entire app lifetime
    GlobalScope.launch {
        delay(1000)
        println("Global coroutine - may outlive the caller")
    }
    
    // Need to keep main thread alive
    Thread.sleep(2000)
}
```

### **Local/Structured Scope (RECOMMENDED)**
```kotlin
import kotlinx.coroutines.*

// ✅ LOCAL - GOOD PRACTICE
fun main() = runBlocking { // Local scope
    // Coroutine is bound to this scope
    launch {
        delay(1000)
        println("Local coroutine - will be cancelled when scope ends")
    }
    
    // No need for Thread.sleep - structured concurrency
    println("Main continues...")
    delay(1500) // Suspends, not blocks
}
```

## Detailed Comparison

| Aspect | Global Scope | Local/Structured Scope |
|--------|--------------|------------------------|
| **Lifetime** | Application lifetime | Bound to scope lifetime |
| **Cancellation** | Not automatic, must be tracked manually | Automatic when scope cancels |
| **Memory Leaks** | High risk | Low risk (properly managed) |
| **Exception Propagation** | Goes to global handler | Structured, can be handled locally |
| **Best For** | Long-lived background tasks (rare) | Most use cases (95%+) |
| **Android Lifecycle** | ❌ Causes leaks | ✅ Use lifecycleScope/viewModelScope |

## How to Choose?

### **Use LOCAL Scope When:**
1. **UI operations** (Android Activity/Fragment)
2. **Network requests** that should cancel when user navigates away
3. **Database operations** tied to a specific context
4. **Most business logic** - 95% of cases
5. **Short-lived tasks** - computations, API calls

### **Use GLOBAL Scope When (RARE):**
1. **App-level services** that must survive screen rotations
2. **Background sync** that should continue even if UI is destroyed
3. **Logging/analytics** that should never be cancelled
4. **Push notification handlers**

## Code Examples

### **Example 1: Local Scope Variations**
```kotlin
import kotlinx.coroutines.*

// Method 1: runBlocking (for main/test functions)
fun main() = runBlocking {
    println("Main starts")
    
    // Child coroutine
    val job = launch {
        delay(1000)
        println("Child coroutine")
    }
    
    job.join() // Wait for child
    println("Main ends - all coroutines complete")
}

// Method 2: coroutineScope builder
suspend fun fetchUserData(): String = coroutineScope {
    val user = async { getUser() }
    val posts = async { getPosts() }
    
    "User: ${user.await()}, Posts: ${posts.await().size}"
}

// Method 3: Custom scope
class UserService {
    private val scope = CoroutineScope(Dispatchers.IO + SupervisorJob())
    
    suspend fun processUser(id: String) = scope.launch {
        // Process user data
    }.join()
    
    fun cancelAll() {
        scope.cancel() // Cancels ALL coroutines in this scope
    }
}
```

### **Example 2: Android Local Scopes**
```kotlin
// Android Example
class MainActivity : AppCompatActivity() {
    // Activity-scoped - cancels when activity destroys
    private val activityScope = CoroutineScope(Dispatchers.Main + Job())
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        // Built-in Android scopes
        lifecycleScope.launch {
            // Cancels automatically when activity destroys
            fetchData()
        }
        
        // Or with ViewModel
        viewModel.viewModelScope.launch {
            loadUserData()
        }
    }
    
    override fun onDestroy() {
        super.onDestroy()
        activityScope.cancel() // Manual cancellation
    }
}
```

### **Example 3: Global Scope (with caution)**
```kotlin
import kotlinx.coroutines.*

class AnalyticsService {
    // Global scope for analytics that should never be cancelled
    fun logEvent(event: String) {
        GlobalScope.launch(Dispatchers.IO) {
            // Send to analytics server
            sendToAnalytics(event)
        }
        // No need to track - it's global
    }
    
    // But better: Use a controlled global scope
    private val analyticsScope = CoroutineScope(Dispatchers.IO + SupervisorJob())
    
    fun logEventBetter(event: String): Job {
        return analyticsScope.launch {
            sendToAnalytics(event)
        }
        // Can still cancel if needed: analyticsScope.cancel()
    }
}
```

## Practical Implementation Patterns

### **Pattern 1: ViewModel with Local Scope**
```kotlin
import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.*
import kotlinx.coroutines.flow.*

class UserViewModel : ViewModel() {
    private val _userState = MutableStateFlow<User?>(null)
    val userState: StateFlow<User?> = _userState.asStateFlow()
    
    fun loadUser(userId: String) {
        viewModelScope.launch {
            try {
                _userState.value = null // Loading state
                val user = fetchUserFromApi(userId)
                _userState.value = user
            } catch (e: Exception) {
                // Handle error - automatically cancelled if ViewModel clears
            }
        }
    }
    
    // If ViewModel is cleared, all coroutines in viewModelScope cancel automatically
}
```

### **Pattern 2: Repository with Custom Scope**
```kotlin
class UserRepository(
    private val api: UserApi,
    private val ioDispatcher: CoroutineDispatcher = Dispatchers.IO
) {
    // Custom scope for repository operations
    private val repositoryScope = CoroutineScope(ioDispatcher + SupervisorJob())
    
    suspend fun getUserWithPosts(userId: String): UserWithPosts {
        // Using repository scope for parallel operations
        return repositoryScope.coroutineScope {
            val userDeferred = async { api.getUser(userId) }
            val postsDeferred = async { api.getUserPosts(userId) }
            
            UserWithPosts(
                user = userDeferred.await(),
                posts = postsDeferred.await()
            )
        }
    }
    
    fun cancelAllOperations() {
        repositoryScope.cancel() // Call when repository is no longer needed
    }
}
```

### **Pattern 3: Global Service (Rare Case)**
```kotlin
object PushNotificationService {
    // Global supervisor job for push handling
    private val pushScope = CoroutineScope(Dispatchers.IO + SupervisorJob())
    
    init {
        // Start long-running coroutine that should survive app lifecycle changes
        pushScope.launch {
            while (isActive) {
                val notification = waitForPushNotification()
                handleNotification(notification)
            }
        }
    }
    
    fun stopService() {
        // Only call when app is being terminated
        pushScope.cancel()
    }
}
```

## Error Handling Differences

### **Local Scope Error Handling**
```kotlin
fun main() = runBlocking {
    // Errors are contained within the scope
    val handler = CoroutineExceptionHandler { _, exception ->
        println("Caught $exception in local scope")
    }
    
    val scope = CoroutineScope(SupervisorJob() + handler)
    
    scope.launch {
        throw RuntimeException("Error 1") // Handled by handler
    }
    
    scope.launch {
        delay(100)
        println("This still runs") // Still runs due to SupervisorJob
    }
    
    delay(200)
    scope.cancel() // Clean shutdown
}
```

### **Global Scope Error Handling**
```kotlin
fun main() {
    // Global errors might crash the app
    GlobalScope.launch {
        throw RuntimeException("Uncaught in global scope!")
    }
    
    Thread.sleep(1000) // App might crash here
    
    // To handle errors in GlobalScope
    val handler = CoroutineExceptionHandler { _, exception ->
        println("Caught: $exception")
    }
    
    GlobalScope.launch(handler) {
        throw RuntimeException("Now handled")
    }
    
    Thread.sleep(1000)
}
```

## Memory Leak Examples

### **❌ BAD: Global scope causing leaks**
```kotlin
class LeakyActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        // ❌ LEAK: Coroutine references activity, lives forever
        GlobalScope.launch {
            delay(5000)
            updateUI() // Activity might be destroyed!
        }
    }
    
    fun updateUI() {
        // Might crash or leak
    }
}
```

### **✅ GOOD: Local scope prevents leaks**
```kotlin
class SafeActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        // ✅ SAFE: Cancels when activity destroys
        lifecycleScope.launch {
            delay(5000)
            if (isActive) { // Check if still alive
                updateUI()
            }
        }
    }
}
```

## Best Practices Summary

1. **Default to Local Scopes**: Use `viewModelScope`, `lifecycleScope`, `coroutineScope`
2. **Avoid GlobalScope**: Except for truly app-level, never-cancel tasks
3. **Use Structured Concurrency**: Let parent scope manage children
4. **Always Cancel Scopes**: When the component is destroyed
5. **Use SupervisorJob**: When you want sibling coroutines to be independent
6. **Propagate Errors Properly**: Local scopes give you control

## Quick Decision Flowchart

```
Start Coroutine
    ↓
Is it tied to UI/Component lifecycle?
    ↓
Yes → Use Local Scope (lifecycleScope/viewModelScope)
    ↓
No → Is it a short-lived operation?
    ↓
Yes → Use coroutineScope or withContext
    ↓
No → Is it truly app-level and must survive everything?
    ↓
Yes → Use controlled Global scope (with SupervisorJob)
    ↓
No → Go back to Local Scope
```

Remember: **When in doubt, use local scope.** Global scope should be the exception, not the rule.
