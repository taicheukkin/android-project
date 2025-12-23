```kotlin
// object expression
//Object expressions are used to create anonymous objects (objects without a name)
// that can extend classes or implement interfaces on the fly

fun main(args: Array<String>){
    val loginButton = Button(
        text = "login", id = 1232,
        object : OnClickListener {
            override fun onClick() {
            } // you can make onclick function particular to each button
        })
    val signUpButton = Button(
        text = "Sign Up", id = 23435,
        object : OnClickListener {
            override fun onClick() {
            }
        })

}

class Button(val text: String, val id: Int, onClickListener: OnClickListener)

class ClickListener() : OnClickListener {
    override fun onClick() {
    }
}

interface OnClickListener {
    fun onClick()
}
```kotlin
