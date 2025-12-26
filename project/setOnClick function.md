# **setOnClickListener() Summary**

**Purpose:** Makes button/View respond to clicks

```kotlin
button.setOnClickListener {
    // Code here runs when button is clicked
    textView.text = "Hello, Ivan"
    textView.visibility = View.VISIBLE
}
```

## **Key Points:**
1. **Attach to any View** (Button, ImageView, Card, etc.)
2. **Lambda syntax** `{ }` = click handler code
3. **Inside lambda** = what happens on click
4. **Common uses:** Show/hide views, change text, navigate screens

## **In Your Code:**
- Button click → Shows TextView with "Hello, Ivan"
- Uses `View.VISIBLE` to make hidden TextView appear

**Alternative:** `android:onClick` in XML (but Kotlin lambda is better)
