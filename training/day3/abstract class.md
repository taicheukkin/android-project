
```kotlin
// abstract class
// abstract class cannot create the instance
// particle use when there are many data api
// . It's used to define a common interface and
// provide partial implementation that subclasses can build upon.
fun main(args: Array<String>) {

}

abstract class vehicle(){
    abstract fun move()
    abstract fun stop()
}

class Car(var name:String, var color: String,val engine:Int,val doors:Int):vehicle(){

// click implement class
    override fun move() {

    }

    override fun stop(){

    }

}
```kotlin


# **Abstract Class vs Interface in Kotlin**

## **Quick Comparison Table:**
| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| **State** | ✅ Can have properties with state | ❌ Properties are abstract (no state) |
| **Constructors** | ✅ Can have constructors | ❌ No constructors allowed |
| **Multiple Inheritance** | ❌ Can extend only ONE class | ✅ Can implement MULTIPLE interfaces |
| **Method Implementation** | ✅ Can have both abstract & concrete methods | ✅ Can have default implementations (Kotlin) |
| **Initialization** | ✅ Can run initialization code | ❌ No initialization code |
| **Access Modifiers** | ✅ Can have private/protected members | ❌ All members are public by default |
| **When to Use** | **Is-A** relationship, shared base logic | **Can-Do** relationship, capabilities |

## **1. Abstract Class - "Is-A" Relationship**

### **Characteristics:**
- Represents **base class** with partial implementation
- Can't be instantiated directly
- Models **hierarchy** and **inheritance**

```kotlin
// Abstract Class Example
abstract class Vehicle(val name: String) {
    // Property WITH STATE (can have initial value)
    var speed: Double = 0.0
    
    // Regular property
    val type: String = "Transport"
    
    // Abstract method (must be implemented by subclasses)
    abstract fun move()
    
    // Concrete method (has implementation)
    fun stop() {
        speed = 0.0
        println("$name stopped")
    }
    
    // Can have constructor
    init {
        println("Vehicle $name created")
    }
    
    // Can have private/protected members
    private fun internalCheck() { }
    protected open fun maintain() { }
}

// Concrete implementation
class Car(name: String) : Vehicle(name) {
    override fun move() {
        speed = 60.0
        println("$name driving at $speed km/h")
    }
    
    override fun maintain() {
        println("Changing oil for $name")
    }
}

// Usage
val car = Car("Tesla")  // "Vehicle Tesla created"
car.move()              // "Tesla driving at 60.0 km/h"
car.stop()              // "Tesla stopped"
```

### **When to use Abstract Class:**
- When you need **shared state** across subclasses
- When you need **constructor parameters** for initialization
- When creating a **template** with partial implementation
- For **"is-a"** relationships (Car **is a** Vehicle)

## **2. Interface - "Can-Do" Relationship**

### **Characteristics:**
- Defines **contract** or **capability**
- No state (in Kotlin 1.8+, interfaces can have backing fields with restrictions)
- Multiple inheritance allowed

```kotlin
// Interface Example
interface Drivable {
    // Abstract property (no backing field by default)
    val maxSpeed: Int
    
    // Property with getter (interface can have computed properties)
    val isOperational: Boolean
        get() = maxSpeed > 0
    
    // Abstract method
    fun drive()
    
    // Default implementation (Kotlin feature)
    fun honk() {
        println("Beep beep!")
    }
    
    // Since Kotlin 1.8: Can have properties with backing fields
    @JvmDefault
    var lastMaintained: String
        get() = field
        set(value) { field = value }
}

interface Electric {
    fun charge()
    val batteryLevel: Int
}

// Class implements MULTIPLE interfaces
class ElectricCar : Drivable, Electric {
    override val maxSpeed = 200
    
    override fun drive() {
        println("Driving silently at up to $maxSpeed km/h")
    }
    
    override fun charge() {
        println("Charging battery...")
    }
    
    override val batteryLevel: Int
        get() = 85
}

// Usage
val tesla = ElectricCar()
tesla.drive()    // "Driving silently at up to 200 km/h"
tesla.honk()     // "Beep beep!" (default implementation)
tesla.charge()   // "Charging battery..."
```

## **3. Key Differences in Detail:**

### **State Management:**
```kotlin
// Abstract Class - CAN HOLD STATE
abstract class Animal {
    var age: Int = 0  // ✅ Can have state
    var name: String = "Unknown"
    
    fun birthday() {
        age++  // ✅ Can modify state
    }
}

// Interface - STATE RESTRICTIONS (Kotlin 1.8+)
interface Pet {
    // ❌ Cannot initialize properties
    // var age: Int = 0  // Error!
    
    // ✅ Can have abstract properties
    val owner: String
    
    // ✅ Can have properties with custom accessors
    val isFriendly: Boolean
        get() = true
    
    // Since Kotlin 1.8: Limited backing fields with @JvmDefault
    @JvmDefault
    var nickname: String
        get() = field
        set(value) { field = value }
}
```

### **Constructor Difference:**
```kotlin
// Abstract Class - CAN HAVE CONSTRUCTOR
abstract class Person(val id: String) {
    init {
        println("Person $id initialized")
    }
}

class Student(id: String) : Person(id)  // Must call super constructor

// Interface - NO CONSTRUCTOR
interface Identifiable {
    val id: String
    // No constructors allowed
}

class Product : Identifiable {
    override val id: String = "P123"  // Must provide id
}
```

### **Multiple Inheritance:**
```kotlin
interface Flyable {
    fun fly()
}

interface Swimmable {
    fun swim()
}

// ✅ Class can implement MULTIPLE interfaces
class Duck : Flyable, Swimmable {
    override fun fly() = println("Flying")
    override fun swim() = println("Swimming")
}

// ❌ Cannot extend MULTIPLE abstract classes
abstract class Bird
abstract class AquaticAnimal

// class Penguin : Bird(), AquaticAnimal()  // ❌ ERROR!
```

### **Access Modifiers:**
```kotlin
abstract class SecureBase {
    private val secret = "hidden"      // ✅ Private allowed
    protected open val key = "123"     // ✅ Protected allowed
    internal val code = "abc"          // ✅ Internal allowed
}

interface Exposable {
    // val secret: String = "hidden"    // ❌ No private in interface
    // protected val key: String        // ❌ No protected in interface
    val code: String                   // ✅ Implicitly public
    // internal val internalCode        // ❌ Can't use internal in interface
}
```

## **4. Modern Kotlin Interfaces (Since 1.8):**

Kotlin interfaces now support more features:

```kotlin
interface ModernInterface {
    // Regular abstract member
    val name: String
    
    // Property with backing field (requires @JvmDefault)
    @JvmDefault
    var counter: Int
        get() = field
        set(value) { 
            if (value >= 0) field = value 
        }
    
    // Default implementation
    fun describe() {
        println("Interface with name: $name, counter: $counter")
    }
    
    // Private function (since Kotlin 1.4)
    private fun helper() {
        println("Private helper")
    }
    
    // Protected NOT allowed in interfaces
    // protected fun protect() {}  // ❌ Error
}
```

## **5. When to Choose Which:**

### **Choose Abstract Class when:**
```kotlin
// 1. Sharing common state
abstract class DatabaseEntity(val id: String) {
    var createdAt: LocalDateTime = LocalDateTime.now()
    var updatedAt: LocalDateTime = createdAt
    
    fun touch() {
        updatedAt = LocalDateTime.now()
    }
}

// 2. Template Method Pattern
abstract class ReportGenerator {
    // Template method with steps
    fun generateReport() {
        fetchData()
        processData()
        formatOutput()
    }
    
    abstract fun fetchData()
    abstract fun processData()
    abstract fun formatOutput()
}

// 3. Base class with partial implementation
abstract class PaymentProcessor {
    fun validate(amount: Double): Boolean {
        return amount > 0
    }
    
    abstract fun process(amount: Double)
}
```

### **Choose Interface when:**
```kotlin
// 1. Multiple capabilities
interface Printable {
    fun print()
}

interface Serializable {
    fun serialize(): String
}

class Document : Printable, Serializable {
    override fun print() { /* ... */ }
    override fun serialize(): String { /* ... */ }
}

// 2. Defining contracts/APIs
interface Repository<T> {
    fun save(item: T)
    fun findById(id: String): T?
    fun delete(id: String)
}

// 3. Strategy Pattern
interface SortingStrategy {
    fun sort(items: List<Int>): List<Int>
}

class BubbleSort : SortingStrategy { /* ... */ }
class QuickSort : SortingStrategy { /* ... */ }
```

## **6. Combined Example:**

```kotlin
// Abstract class for shared state/behavior
abstract class Notification(val recipient: String) {
    abstract fun send()
    
    protected fun log(message: String) {
        println("[LOG] $message")
    }
}

// Interfaces for capabilities
interface Retryable {
    var maxRetries: Int
    fun retry()
}

interface Prioritized {
    val priority: Int
}

// Concrete class using both
class EmailNotification(recipient: String) : 
    Notification(recipient), 
    Retryable, 
    Prioritized {
    
    override var maxRetries = 3
    override val priority = 2
    
    override fun send() {
        println("Sending email to $recipient")
        log("Email sent")
    }
    
    override fun retry() {
        println("Retrying email...")
    }
}
```

## **Simple Rule of Thumb:**
- **Abstract Class**: "What something **IS**" + Shared implementation
- **Interface**: "What something **CAN DO**" + Capabilities/Contracts

**When in doubt, prefer interfaces** - they're more flexible and support multiple inheritance. Use abstract classes only when you need state, constructors, or non-public members.
