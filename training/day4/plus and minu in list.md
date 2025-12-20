
```kotlin
fun main(){
    val numbers= mutableListOf("one","two","three","four")
    val plusList=numbers+"five" // [one, two, three,four.five]
    val minusList=numbers-mutableListOf("three","four") // [one, two]
   println(plusList)
    println(minusList)
}// plus and minu in list
`` kotlin
