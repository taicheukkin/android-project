// it is going to convert to java complier
// and code to Java ByteCode, and run on JVM and convert to machine code
//  different machine code corresponding to different system (window,linux, Mac)

fun main(args: Array<String>){
    val userName: String ="Alex"// is going to store character
    // or userName ="Alex"//  it is treated as String type since type inference
    // userName ="John" result error, val cannot assign to the new variable
    var age: Int=0
    // or age=0//  it is treated as integer type since type inference
    age=10
    println("Hello $userName  your age is $age")
}
// var or val
// IF you cannot assign the type of variable different from the variable as you declared initially

