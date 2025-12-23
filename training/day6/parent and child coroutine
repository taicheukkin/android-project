# **Coroutine Parent-Child & Execution Models**

## **Parent-Child Relationships**
```
Parent Coroutine (runBlocking)
├── Child 1 (launch)     → Cancels if parent cancels
├── Child 2 (async)      → Cancels if sibling fails
└── Child 3 (launch)     → Waits for parent to complete
```

### **Key Rules:**
1. **Parent waits** for all children before completing
2. **Parent cancellation** → All children cancelled
3. **Child exception** → Cancels parent & siblings
4. **Scope boundaries** define parent-child relationships

```kotlin
// Parent-child example
fun main() = runBlocking { // PARENT
    println("Parent starts")
    
    launch { // CHILD 1
        delay(1000)
        println("Child 1 done")
    }
    
    launch { // CHILD 2
        delay(500)
        println("Child 2 done")
    }
    
    // Parent automatically waits for children
    println("Parent ends - but only after children") // Prints after 1 second
}
```

