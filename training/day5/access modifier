In **Kotlin**, access modifiers work similarly to other languages but with some important Kotlin-specific characteristics and simplifications.

## **Main Access Modifiers:**

### **1. `private`** - Most restrictive
```kotlin
class Example {
    private val secret = "hidden"
    private fun hiddenMethod() { }
}
```
- Accessible **only within the same class or file** (for top-level declarations)

### **2. `protected`** 
```kotlin
open class Parent {
    protected val inherited = "accessible in subclasses"
}
class Child : Parent() {
    fun test() {
        println(inherited) // OK
    }
}
```
- Accessible in **the class and its subclasses**
- **Cannot** be used for top-level declarations (outside classes)

### **3. `internal`** - Kotlin-specific
```kotlin
internal class InternalClass {
    internal val visibleInModule = "module-wide"
}
```
- Accessible **anywhere within the same module**
- A **module** is a set of Kotlin files compiled together (e.g., IntelliJ module, Maven/Gradle project)

### **4. `public`** - Default
```kotlin
public class PublicClass {  // 'public' is optional
    val defaultIsPublic = "This is public"
}
```
- Accessible **from anywhere**
- **Default modifier** (can be omitted)

## **Key Kotlin Differences:**

### **Default Visibility**
- In Kotlin: **`public`** is default
- In Java: **package-private** is default

### **Package-Private is Gone**
- Kotlin **doesn't have** package-private visibility
- Use `internal` for module-level access instead

### **Top-Level Declarations**
```kotlin
private const val SECRET_CONSTANT = 42  // File-private
internal val MODULE_CONSTANT = 100     // Module-wide
val PUBLIC_CONSTANT = 1                // Everywhere (public)
```

### **Constructor Visibility**
```kotlin
class Person private constructor(val name: String) {
    // Private constructor
}

class Customer internal constructor(name: String) {
    // Internal constructor
}
```

### **Setter-Specific Visibility**
```kotlin
var name: String = ""
    private set  // Can't modify from outside

var counter: Int = 0
    internal set  // Can modify within module
```

## **Summary Table:**

| Modifier | Class Member | Top-Level Declaration |
|----------|--------------|----------------------|
| `private` | Same class only | Same file only |
| `protected` | Class + subclasses | **Not allowed** |
| `internal` | Same module | Same module |
| `public` | Everywhere (default) | Everywhere (default) |

## **Example:**
```kotlin
// File: Utils.kt
private fun helper() { }        // Only in this file
internal fun calculate() { }    // Module-wide
fun publicUtility() { }         // Everywhere (public)

open class Base {
    private val secret = 1      // Base class only
    protected val forKids = 2   // Base + subclasses
    internal val moduleData = 3 // Module-wide
    val publicData = 4          // Everywhere
}

class Derived : Base() {
    fun test() {
        // secret - ❌ Not accessible
        // forKids - ✅ Accessible
        // moduleData - ✅ Accessible
        // publicData - ✅ Accessible
    }
}
```

Kotlin's access modifiers promote better encapsulation with `internal` providing a practical middle ground between package-private and public, especially useful for library development.
