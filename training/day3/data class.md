```kotlin

fun main(args: Array<String>) {
    val name1 = "Alex"
    val name2 = "Alex"
    println(name1 == name2) // checking structural quality (return true)
    println(name1 == name1) // check referential quality(return true)

    val user1 = User(firstName = "Alex", lastName = "Dobbin", age = 23)
    val user2 = User(firstName = "Alex", lastName = "Dobbin", age = 23)
    println(user1 == user2)

    // output is false if it is not data class, because they are distinct object
    // every class created is implicitly inherited from Any class
}

data class User(var firstName: String, var lastName: String, var age: Int) {

// data class implement the following logic to ensure structural equality of object
// override three function are inherited from any class




//    override fun equals(other: Any?): Boolean {
//        if (this === other) { // "this" means user1
//            return true
//        } // check structural equality
//        if (other is User) {
//            return this.firstName == other.firstName
//                    && this.lastName == other.lastName
//                    && this.age == other.age
//        }
//        return false
//    }
//
//
//    override fun hashCode(): Int {
//        return 0
//    }
//
//    override fun toString(): String {
//        return("User(firstName='$firstName', lastName='$lastName', age=$age)")
//    }
} ```kotlin
