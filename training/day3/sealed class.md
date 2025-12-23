```kotlin
// sealed class


fun main() {
    val success = Result.Success("Success")
    val progress = Result.Progress(message = "Progress")
    getData(success)
}

fun getData(result: Result) {
    when (result) { // click the lightbulb and add remaining branches
        is Result.Success -> result.showMessage()
        is Result.Progress -> result.showMessage()
        is Result.Error.Recoverable -> result.showMessage()
        is Result.Error.Unrecoverable -> result.showMessage()
    }
}
//
////enum class  Result{
//    SUCCESS,
//    ERROR // you cannot create object in enum class
//}

sealed class Result(val message: String) {
    fun showMessage() {
        println("Show Message:$message")
    }
// subclass
    class Success(message: String) : Result(message)
    class Progress(message: String) : Result(message)
    sealed class Error(message: String) : Result(message) {
        class Recoverable(message: String) : Error(message)
        class Unrecoverable(message: String) : Error(message)
    }
}
```kotlin
