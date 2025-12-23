# **Examples for Each Kotlin Concept**

## **1. Generics Type Safety Example**
```kotlin
class Box<T>(val content: T)

val stringBox = Box("Hello")    // Type: Box<String>
val intBox = Box(42)            // Type: Box<Int>

// stringBox.content = 123      // ❌ Compile error: must be String
// intBox.content = "World"     // ❌ Compile error: must be Int

fun <T> printItem(item: T) {
    println(item)
}

printItem("Text")      // ✅ T = String
printItem(100)         // ✅ T = Int
```

## **2. Variance Examples**

### **Covariance (`out`) - Producer**
```kotlin
// Kotlin's List is covariant
val strings: List<String> = listOf("a", "b")
val objects: List<Any> = strings  // ✅ Covariant: List<String> → List<Any>

// Read-only producer
interface Producer<out T> {
    fun produce(): T  // ✅ T in OUTPUT position
}

class DogProducer : Producer<Dog> {
    override fun produce() = Dog()
}

val animalProducer: Producer<Animal> = DogProducer()  // ✅ Covariant
```

### **Contravariance (`in`) - Consumer**
```kotlin
// Comparator is contravariant
val anyComparator: Comparator<Any> = Comparator { a, b -> 
    a.toString().compareTo(b.toString()) 
}
val stringComparator: Comparator<String> = anyComparator  // ✅ Contravariant

// Write-only consumer
interface Consumer<in T> {
    fun consume(item: T)  // ✅ T in INPUT position
}

class AnimalConsumer : Consumer<Animal> {
    override fun consume(item: Animal) { println("Eating animal") }
}

val dogConsumer: Consumer<Dog> = AnimalConsumer()  // ✅ Contravariant
```

### **Invariant (No Variance) - Both Read/Write**
```kotlin
class MutableBox<T>(var content: T)  // No 'in' or 'out'

val stringBox = MutableBox("Hello")
// val anyBox: MutableBox<Any> = stringBox  // ❌ Not allowed
// val anyBox: MutableBox<Any> = stringBox as MutableBox<Any>  // ❌ Unsafe
```

## **3. `as` Keyword Examples**
```kotlin
val obj: Any = "Hello World"

// Unsafe cast (throws if wrong)
val str1: String = obj as String  // ✅ Works
// val num: Int = obj as Int      // ❌ ClassCastException

// Safe cast (null if wrong)
val str2: String? = obj as? String  // ✅ Returns "Hello World"
val num: Int? = obj as? Int         // ✅ Returns null

// Smart cast with 'is'
if (obj is String) {
    val length = obj.length  // ✅ Smart cast to String
    // No need for 'as' here
}

// Chained safe casts
val input: Any = 123
val result = input as? String ?: "default"  // Returns "default"
```

## **4. Reified Generics Example**
```kotlin
// Without reified (can't check type at runtime)
fun <T> isInstance(obj: Any): Boolean {
    // return obj is T  // ❌ Error: Cannot check for instance of erased type
    return false
}

// With reified (can check type)
inline fun <reified T> isInstance(obj: Any): Boolean {
    return obj is T  // ✅ Works because T is reified
}

// Usage
println(isInstance<String>("Hello"))      // ✅ true
println(isInstance<Int>("Hello"))         // ✅ false

// Practical use - filter by type
inline fun <reified T> filterByType(list: List<Any>): List<T> {
    return list.filterIsInstance<T>()  // Built-in uses reified
}

val mixed = listOf(1, "two", 3, "four")
val numbers: List<Int> = filterByType(mixed)  // [1, 3]
```

## **5. Star Projection Example**
```kotlin
// Accepts List of any type
fun printSize(items: List<*>) {
    println("List has ${items.size} elements")
}

printSize(listOf(1, 2, 3))        // ✅
printSize(listOf("a", "b"))       // ✅
printSize(listOf(1, "two", 3.0))  // ✅

// Star projection limitations
fun process(items: List<*>) {
    // val first = items[0]        // Type is Any? (not specific)
    val first: Any? = items[0]     // Must use Any?
    
    // items.add("new")            // ❌ Can't add (read-only)
}

// Type projection combinations
val list: MutableList<out Number> = mutableListOf(1, 2, 3)
// list.add(4)                     // ❌ Can't add (out projection)
val num: Number = list[0]          // ✅ Can read as Number

val list2: MutableList<in Number> = mutableListOf()
list2.add(3.14)                    // ✅ Can add Number or subtype
val item: Any? = list2[0]          // ✅ Can only read as Any?
```

## **6. Complete Working Example**
```kotlin
// Variance hierarchy
open class Animal
open class Dog : Animal()
class Puppy : Dog()

// Covariant producer
class Vet<out T : Animal> {
    fun treat(): T = TODO()
}

// Contravariant consumer  
class Trainer<in T : Animal> {
    fun train(animal: T) { println("Training $animal") }
}

// Usage
val puppyVet = Vet<Puppy>()
val dogVet: Vet<Dog> = puppyVet     // ✅ Covariant (Puppy → Dog)
val animalVet: Vet<Animal> = dogVet // ✅ Covariant (Dog → Animal)

val animalTrainer = Trainer<Animal>()
val dogTrainer: Trainer<Dog> = animalTrainer     // ✅ Contravariant
val puppyTrainer: Trainer<Puppy> = dogTrainer    // ✅ Contravariant

// Generic function with reified type
inline fun <reified T : Animal> findAnimals(animals: List<Any>): List<T> {
    return animals.filterIsInstance<T>()
}

val mixedList = listOf(Puppy(), Dog(), "Not an animal", 42, Animal())
val dogs: List<Dog> = findAnimals(mixedList)  // [Puppy(), Dog()]
```
