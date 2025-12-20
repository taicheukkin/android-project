```kotlin
fun main(){
    val numbers=listOf("one","two","three","four","five")  // Creates a list of 5 strings
    
    println(numbers.slice(indices= 1..3))  // Takes elements at indices 1, 2, 3 → [two, three, four]
    
    println(numbers.slice( indices = 0..4 step 2))  // Takes elements at indices 0, 2, 4 → [one, three, five]
    
    println(numbers.slice(setOf(3,5,1)))  // Takes elements at indices 3, 5, 1 → [four, two] (5 is out of bounds, ignored)
    
    println(numbers.take(3))  // Takes first 3 elements → [one, two, three]
    
    println(numbers.takeLast(3))  // Takes last 3 elements → [three, four, five]
    
    println(numbers.drop(1))  // Drops first 1 element → [two, three, four, five]
    
    println(numbers.dropLast(5))  // Drops last 5 elements → [] (empty list)
}
```
