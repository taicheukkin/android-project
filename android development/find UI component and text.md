## **`findViewById()` - Find UI Elements**

### **Purpose:**
Get reference to a View (Button, TextView, etc.) from XML layout to control it in code.

### **Basic Usage:**
```kotlin
// Kotlin (old way - needs import)
import kotlinx.android.synthetic.main.activity_main.*
textView.text = "Hello"  // Auto-magically works

// Kotlin (explicit way)
val myButton = findViewById<Button>(R.id.btn_submit)
myButton.text = "Click Me"

// Java
Button myButton = findViewById(R.id.btn_submit);
myButton.setText("Click Me");
```

### **Modern Replacement:**
**View Binding** (recommended instead):
```kotlin
// In Activity
binding = ActivityMainBinding.inflate(layoutInflater)
setContentView(binding.root)
binding.textView.text = "Hello"  // Direct access, no findViewById!

// In Fragment
binding = FragmentHomeBinding.inflate(inflater, container, false)
binding.button.setOnClickListener { }
```

---

## **`getString()` - Get Text from Resources**

### **Purpose:**
Retrieve text from `strings.xml` file (for translations, consistency).

### **`strings.xml` File:**
```xml
<resources>
    <string name="app_name">My App</string>
    <string name="welcome">Hello %s!</string>
    <string name="count">You have %d items</string>
</resources>
```

### **Usage:**
```kotlin
// Basic
val appName = getString(R.string.app_name)  // "My App"

// With formatting (placeholders)
val name = "John"
val welcome = getString(R.string.welcome, name)  // "Hello John!"

val itemCount = 5
val message = getString(R.string.count, itemCount)  // "You have 5 items"
```

### **Multiple placeholders:**
```xml
<string name="order">Order %1$s: %2$d items at %3$.2f each</string>
```
```kotlin
getString(R.string.order, "ABC123", 5, 9.99f)
// "Order ABC123: 5 items at 9.99 each"
```

---

## **Key Differences:**

| Aspect | `findViewById()` | `getString()` |
|--------|-----------------|---------------|
| **Purpose** | Find UI elements | Get text resources |
| **Parameter** | View ID (`R.id.xxx`) | String ID (`R.string.xxx`) |
| **Returns** | View object (Button, TextView) | String text |
| **Common Use** | Change properties, set listeners | Display text, translations |
| **Modern Alternative** | **View Binding** | **String resources** (still best practice) |

---

## **When to Use:**

### **Use `findViewById()`/View Binding when:**
- Changing text: `textView.text = "New Text"`
- Setting click listeners: `button.setOnClickListener`
- Changing visibility: `imageView.visibility = View.GONE`
- Getting input: `editText.text.toString()`

### **Use `getString()` when:**
- Displaying any text to user
- Supporting multiple languages
- Reusing same text in multiple places
- Formatting text with variables

---

## **Best Practices:**

1. **NEVER hardcode text** - Always use `getString(R.string.xxx)`
2. **Use View Binding** instead of `findViewById()` (type-safe, null-safe)
3. **Store ALL user-facing text** in `strings.xml`
4. **Format strings** in XML, not code: `Welcome %s!` not `"Welcome " + name`
5. **Use string arrays** for lists:
```xml
<string-array name="colors">
    <item>Red</item>
    <item>Green</item>
</string-array>
```
```kotlin
val colors = resources.getStringArray(R.array.colors)
```

---

## **Common Patterns:**

```kotlin
// Button with text from resources
binding.button.text = getString(R.string.submit)

// Toast message
Toast.makeText(this, getString(R.string.saved), Toast.LENGTH_SHORT).show()

// AlertDialog title
AlertDialog.Builder(this)
    .setTitle(getString(R.string.confirm))
    .setMessage(getString(R.string.are_you_sure))
    .show()

// TextView with formatted text
val score = 100
binding.scoreText.text = getString(R.string.score_format, score)
```

**Pro Tip**: Use **Android Studio's "Extract string resource"** (Alt+Enter on hardcoded text) to automatically create entry in `strings.xml`!
