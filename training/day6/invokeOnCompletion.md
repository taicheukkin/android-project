# `invokeOnCompletion()` - Quick Summary

**Purpose:** Attaches a callback that executes when a coroutine job completes (successfully, with cancellation, or with an exception).

## Key Characteristics:
```kotlin
job.invokeOnCompletion { cause: Throwable? ->
    // cause == null → Job completed successfully
    // cause == CancellationException → Job was cancelled
    // cause is other Exception → Job failed with exception
}
```

## Main Use Cases:

1. **Cleanup/Teardown Logic**
   ```kotlin
   val job = launch {
       // work
   }
   job.invokeOnCompletion {
       releaseResources()
   }
   ```

2. **Logging/Monitoring**
   ```kotlin
   job.invokeOnCompletion { cause ->
       when {
           cause == null -> log("Completed successfully")
           cause is CancellationException -> log("Cancelled")
           else -> log("Failed: $cause")
       }
   }
   ```

3. **Trigger Follow-up Actions**
   ```kotlin
   job.invokeOnCompletion {
       startNextProcess()
   }
   ```

## Important Notes:
- **Runs once** when job reaches final state (completed/cancelled/failed)
- **Thread-safe** - can be called from any thread
- **Immediate execution** if job is already completed
- **Not a suspend function** - callback runs in context of caller
- **Multiple handlers** can be attached; they all run

## Comparison with `join()`:
- `join()` → **waits** for completion (blocking caller)
- `invokeOnCompletion()` → **registers callback** (non-blocking, fire-and-forget)

**Best for:** Side effects when job finishes, without blocking current execution.
