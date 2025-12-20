```kotlin
// Binary search algorithm demonstration
fun main(){
    // Binary search requires SORTED collection
    println(searchElement(27, mutableListOf(1, 2, 3, 4, 5, 6, 7, 8, 9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30)))
}

private fun searchElement(searchElement:Int, numbers:MutableList<Int>):Int // or using builtin function  =numbers[numbers.binarySearch(searchElement)]
{
     Binary search mechanism: repeatedly divides search space in half
    var low = 0                    // Start index of search range
    var high = numbers.size - 1    // End index of search range
    
    while (low <= high) {          // Continue while search space exists
        val mid = low + (high - low) / 2  // Calculate MIDDLE index (avoids overflow)
        
        // Compare middle element with target
        val comparison = numbers[mid].compareTo(searchElement)
        
        if(comparison < 0) {       // numbers[mid] < searchElement
            low = mid + 1           // Target is in RIGHT half, discard left half
        }
        else if(comparison > 0) {  // numbers[mid] > searchElement
            high = mid - 1          // Target is in LEFT half, discard right half
        }
        else {                      // numbers[mid] == searchElement (FOUND!)
            return numbers[mid]     // Return found element
        }
    }
    
    return -1  // Target not found (MUST BE OUTSIDE while loop)


//mechanism

// step 1
// mid = 0 + (29-0)/2 = 14
// numbers[14] = 15
// 15 < 27 → comparison < 0 → low = 14 + 1 = 15

// step 2
// low = 15, high = 29
// mid = 15 + (29-15)/2 = 15 + 7 = 22
// numbers[22] = 23
23 < 27 → comparison < 0 → low = 22 + 1 = 23

// low = 23, high = 29
// mid = 23 + (29-23)/2 = 23 + 3 = 26
// numbers[26] = 27
// 27 == 27 → comparison == 0 → FOUND!



 //var i=0
//    for(number in numbers){
//        i++
//        println("Searched number:$i")
//        if(number==searchElement){
//            return number
//        }
//    }
//    return -1
// the code occupied too many memory in case there are 10000 elements to search
}
``kotlin
