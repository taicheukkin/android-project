# **Activity Lifecycle - Simple Explanation**
<img width="1082" height="904" alt="image" src="https://github.com/user-attachments/assets/60b1dc63-2a67-472a-b420-c0eb186e4113" />

## **1. BORN → onCreate()**
**When:** Screen first created (ONLY ONCE)
**Do:** Basic setup, load layout
```kotlin
override fun onCreate() {
    setContentView(R.layout.activity_main) // Load UI
    initViews() // Setup buttons, etc.
}
```

## **2. VISIBLE → onStart()**
**When:** Screen becomes visible (not interactive yet)
**Do:** Prepare animations, load light data

## **3. INTERACTIVE → onResume()**
**When:** User can NOW touch/use screen
**Do:** Start animations, sensors, camera
**Screen is now:** **RUNNING**

---

## **PAUSED (Partially Covered) → onPause()**
**When:** Another screen covers part (dialog, phone call)
**Do:** Pause animations, save draft
**Example:** User gets phone call notification

## **STOPPED (Not Visible) → onStop()**
**When:** Another FULL screen opens
**Do:** Save important data, release resources
**Example:** User opens Settings app

---

## **COMING BACK:**

### **If still in memory:**
`onRestart()` → `onStart()` → `onResume()`

### **If killed by system:**
`onCreate()` → `onStart()` → `onResume()` (fresh start)

## **DEAD → onDestroy()**
**When:** Screen closes forever
**Do:** Clean up resources, save final data

---

## **REAL-LIFE ANALOGY:**
1. **onCreate()** = Build restaurant kitchen
2. **onStart()** = Open doors, turn on lights  
3. **onResume()** = Start serving customers ✅
4. **onPause()** = Take break (phone call)
5. **onStop()** = Close for the night
6. **onDestroy()** = Demolish building

## **WHAT TO SAVE WHERE:**
```kotlin
// TEMPORARY (screen rotation):
override fun onSaveInstanceState(bundle: Bundle) {
    bundle.putInt("score", score) // Saved in onPause()
}

// PERMANENT (app close):
override fun onStop() {
    saveToDatabase() // User leaves screen
}
```

## **QUICK GUIDE:**
- **User opens screen:** `onCreate()` → `onStart()` → `onResume()`
- **User gets notification:** `onPause()` (still visible)
- **User opens another app:** `onPause()` → `onStop()`
- **User returns:** `onRestart()` → `onStart()` → `onResume()`
- **User presses back:** `onPause()` → `onStop()` → `onDestroy()`

**Remember:** `onResume()` = User can TOUCH, `onPause()` = User CAN'T touch
