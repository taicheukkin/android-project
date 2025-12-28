## Toast.makeText() in Kotlin

The `Toast.makeText()` function is used to display short informational messages (toasts) to users in Android applications.

## Basic Syntax

```kotlin
Toast.makeText(context, message, duration).show()
```

## Parameters

1. **context**: The current context (Activity, Application, etc.)
2. **message**: The text to display (String or String resource)
3. **duration**: Toast length - either `Toast.LENGTH_SHORT` or `Toast.LENGTH_LONG`


Toast.makeText(this@MainActivity, "Hello!", Toast.LENGTH_SHORT).show()

// In Fragment
Toast.makeText(requireContext(), "Fragment toast", Toast.LENGTH_SHORT).show()

// In Application class
Toast.makeText(applicationContext, "App message", Toast.LENGTH_LONG).show()
```


```kotlin

package com.example.activity2

import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import android.widget.Toast

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        Toast.makeText(this@MainActivity,"onCreated called",Toast.LENGTH_SHORT).show()
    }
    override fun onStart(){
        super.onStart()
        Toast.makeText(this@MainActivity ,"onStart called",Toast.LENGTH_SHORT).show()
    }
    override fun onResume(){
        super.onResume()
        Toast.makeText(this@MainActivity ,"onResume called",Toast.LENGTH_SHORT).show()
    }
    override fun onPause(){
        super.onPause()
        Toast.makeText(this@MainActivity ,"onPause called",Toast.LENGTH_SHORT).show()
    }
    override fun onStop(){
        super.onStop()
        Toast.makeText(this@MainActivity ,"onStop called",Toast.LENGTH_SHORT).show()
    }
    override fun onDestroy(){
        super.onDestroy()
        Toast.makeText(this@MainActivity ,"onDestroy called",Toast.LENGTH_SHORT).show()
    }

}
```kotlin
