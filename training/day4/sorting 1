```kotlin
fun main(){
    val numbers=mutableListOf(2,5,1,40,20,100,60) // Mutable list of integers (can be modified)
    
    // sorted() creates a new sorted list (original remains unchanged)
    // Returns a sorted list, forEach prints each element
    numbers.sorted().forEach{print(it)} // Sorts ascending: 1 2 5 20 40 60 100
    
    val laptops=mutableListOf(
        Laptop(brand="Doll",year=2011, ram=4, price=600)  // data class instances
        ,Laptop(brand="Acer",year=2020,ram=8, price=800)   // with named parameters
        ,Laptop(brand="Apple",year=2022,ram=16, price=1000))

    // Uses compareTo() method (must be overridden in Comparable interface)
    // Returns sorted list based on custom compareTo() logic
    laptops.sorted().forEach{print(it)} // Sorts by price first, then year

    // sortedWith() uses Comparator to sort
    // Returns list sorted by ComparatorRam's compare() method
    laptops.sortedWith(ComparatorRam()).forEach{print(it)} // Sorts by RAM ascending

}

// data class automatically generates equals(), hashCode(), toString(), copy()
// Comparable<Laptop> interface requires overriding compareTo() method
data class Laptop(val brand:String, val year:Int, val ram: Int,var price:Int):Comparable<Laptop> {
    // override keyword indicates we're implementing/overriding a parent method
    // compareTo() must return: 1(this > other), -1(this < other), 0(equal)
    override fun compareTo(other: Laptop): Int {
        // return keyword exits function with specified value
        return if (this.price > other.price) {
            println("In if statement: Swapping ${this.brand} with ${other.brand}")
            1 // Return 1 when this laptop has higher price
        } else if (this.year > other.year) {
            println("In if statement: Swapping ${this.brand} with ${other.brand}")
            -1 // Return -1 when this laptop is older (higher year means newer)
        } else {
            0 // Return 0 when equal (same price and year)
        }
    }
}

// Custom comparator class implements Comparator interface
// Comparator requires overriding compare() method with two parameters
class ComparatorRam:Comparator<Laptop> {
    // Override compare() to define RAM-based sorting
    override fun compare(laptop1: Laptop, laptop2: Laptop): Int {
        return if (laptop1.ram>laptop2.ram){
            return 1 // return 1 when laptop1 has more RAM
            }
            else if(laptop1.ram<laptop2.ram){
             return -1 // return -1 when laptop2 has more RAM
        }
        else{return 0} // return 0 when RAM is equal
    }
}

// Another comparator for year-based sorting
class ComparatorYear:Comparator<Laptop> {
    override fun compare(laptop1: Laptop, laptop2: Laptop): Int {
        return if (laptop1.year>laptop2.year){
            return 1 // return 1 when laptop1 is newer (higher year)
        }
        else if(laptop1.year<laptop2.year){
            return -1 // return -1 when laptop2 is newer
        }
        else{return 0} // return 0 when same year
    }
}
```kotlin
