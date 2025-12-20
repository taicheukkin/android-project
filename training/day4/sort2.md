fun main(){
    val numbers=mutableListOf(2,5,1,40,20,100,60) // Creates mutable list of integers
    
    numbers.sorted().forEach{print(it)} // Sorts numbers naturally (ascending): 1 2 5 20 40 60 100
    
    val laptops=mutableListOf(
        Laptop(brand="Doll",year=2011, ram=4, price=600)
        ,Laptop(brand="Acer",year=2020,ram=8, price=800)
        ,Laptop(brand="Apple",year=2022,ram=16, price=1000))

    // sortedWith(compareBy{}) - Uses compareBy to create Comparator on the fly
    laptops.sortedWith(compareBy{it.price}).forEach{print(it)} // Sorts laptops by price ascending
    
    println("\n") // Prints newline for separation
    
    // sortedBy{} - Shorter syntax, sorts by single property
    laptops.sortedBy{it.ram}.forEach{print(it)} // Sorts laptops by RAM ascending (same result as sortedWith but concise)
    
    // sortedWith with multiple sort criteria using thenBy
    laptops.sortedWith(compareBy<Laptop>{it.year}.thenBy{it.price}) // Sorts by year first, then price
}

// data class automatically provides useful methods like toString(), equals(), hashCode(), copy()
data class Laptop(val brand:String, val year:Int, val ram: Int,var price:Int)
