## **Create New Layout File:**
1. Right-click `res/layout` folder
2. **New → Layout Resource File**
3. Enter name, choose root element (ConstraintLayout)
4. Click **OK** → Creates `.xml` file

---

## **Change Layout Reference:**

### **In Activity (Kotlin/Java):**
```kotlin
// Current:
setContentView(R.layout.activity_main)

// Change to:
setContentView(R.layout.your_new_layout)
```

### **In Fragment:**
```kotlin
return inflater.inflate(R.layout.your_new_layout, container, false)
```

### **Include Layout (Reuse):**
```xml
<include layout="@layout/your_layout"/>
```

---

## **Key Points:**
- **File naming**: `activity_name.xml`, `fragment_name.xml`
- **Auto-generated**: `R.layout.filename` after creating file
- **View Binding** (modern): `ActivityNameBinding.inflate()`
- **Multiple versions**: Create `layout-land/`, `layout-sw600dp/` folders

**Shortcut**: `Alt+Insert` on `layout` folder → Layout Resource File
## **`inflater.inflate()` Method Explained**

### **What it does:**
Converts XML layout file into actual View objects in memory.

### **Three Parameters:**
```kotlin
inflater.inflate(R.layout.fragment_example, container, false)
```
1. **`R.layout.filename`** - Which XML file to load
2. **`container`** - Parent view that will contain this layout
3. **`false`** - Whether to attach to container immediately

---

## **`false` vs `true` Parameter:**

### **`attachToRoot = false`** (RECOMMENDED for Fragments)
- Layout NOT immediately added to container
- Fragment manages adding/removing
- **Use in**: `onCreateView()` of Fragments
```kotlin
return inflater.inflate(R.layout.fragment_my, container, false)
```

### **`attachToRoot = true`** 
- Layout immediately attached to container
- **Use in**: Custom Views, special cases
```kotlin
val view = inflater.inflate(R.layout.custom_view, parent, true)
// View is already added to parent
```

