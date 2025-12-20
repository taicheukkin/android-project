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


println(numbers.takeWhile{!it.startsWith("f")})  // Takes elements until first element starting with "f" → [one, two, three]

println(numbers.takeLastWhile{it!="three"})  // Takes elements from end until "three" is found → [four, five]

println(numbers.dropWhile{it.length==3})  // Drops elements while length is 3, stops at first length≠3 → [three, four, five]

println(numbers.dropLastWhile{it.contains("1")})  // Drops from end while containing "1" (none here) → keeps all

val numbersInt = (0..13).toList()  // Creates list of integers from 0 to 13

println(numbersInt.chunked(3))  // Splits list into chunks of size 3 → [[0, 1, 2], [3, 4, 5], [6, 7, 8], [9, 10, 11], [12, 13]]

println(numbersInt.chunked(3){it.sum()})  // Creates chunks of 3 elements, sums each chunk: (0+1+2)=3, (3+4+5)=12, (6+7+8)=21, (9+10+11)=30, (12+13)=25 → [3, 12, 21, 30, 25]

println("\n")  // Prints blank line for separation

val numbersString2=numbers  // Creates another reference to same list

println(numbersString2.windowed(3))  // Creates sliding windows of size 3 → [[one, two, three], [two, three, four], [three, four, five]]
```
