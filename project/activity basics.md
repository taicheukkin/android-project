```kotlin
// 1. CLASS DECLARATION
// "MainActivity" is your main screen class
// "AppCompatActivity" = Modern Android Activity with backward compatibility
class MainActivity : AppCompatActivity() {
    
    // 2. onCreate() METHOD
    // Called when screen FIRST LOADS (only once)
    override fun onCreate(savedInstanceState: Bundle?) {
        
        // 3. SUPER CALL
        // MUST be first - calls parent class setup
        super.onCreate(savedInstanceState)
        
        // 4. SET CONTENT VIEW
        // Loads UI from XML file (activity_main.xml)
        // Links Kotlin code with XML layout
        setContentView(R.layout.activity_main)
        
        // 5. YOUR CODE GOES HERE
        // (Currently empty - add button clicks, data loading, etc.)
    }
}
```

## **ANALOGY:**
1. **Class** = Blueprint for a screen
2. **onCreate()** = Construction crew setting up the screen
3. **setContentView()** = Unpacking furniture (XML) into the room

## **FULL FLOW:**
```
User opens app → Android creates MainActivity → onCreate() runs → 
super.onCreate() → setContentView() loads XML → Screen appears
```


# **Bundle & SavedInstanceState Explained**

**Yes, exactly!**

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    //                    ↑
    // This IS the Bundle parameter!
    // Named "savedInstanceState"
}
```

## **Bundle = Parameter of onCreate()**

**Think of it as:**
```kotlin
// Android calls your onCreate() and passes:
// "Here's your saved data from before (if any)"
onCreate(savedDataFromLastTime: Bundle?)
```

## **What's actually happening:**

```kotlin
// WHEN SCREEN FIRST OPENS (fresh):
onCreate(null)  // Bundle is null - no previous state

// WHEN SCREEN ROTATES/recreates:
onCreate(bundleWithSavedData)  // Bundle has your saved data
```

## **Simple check:**
```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    
    if (savedInstanceState == null) {
        // First time opening screen
        // Load fresh data
    } else {
        // Screen was recreated (rotation)
        // Restore saved data
        val userName = savedInstanceState.getString("name")
    }
}
```

*

## **2. savedInstanceState**
**What:** Android's automatic "save file" for your activity
**When used:** When screen rotates OR app is killed in background
```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    
    if (savedInstanceState != null) {
        // RESTORE data after rotation/kill
        val score = savedInstanceState.getInt("score", 0)
    }
}

override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    // SAVE data before rotation/kill
    outState.putInt("score", 100)
}
```

## **Simple Analogy:**
- **Bundle** = Plastic bag for carrying groceries (data)
- **savedInstanceState** = Android's fridge (auto-saves/restores)


```

## **Key Points:**
- **savedInstanceState** = Temporary (lost when app fully closes)
- **For permanent storage** → Use SharedPreferences, Database, Files
- **Always call** `super.onSaveInstanceState()` first
- **Check for null** `savedInstanceState` in `onCreate()`
