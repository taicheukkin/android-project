## **Image Caption:**
"Android Studio Layout Editor - Designing UI in activity_main.xml with ConstraintLayout"
<img width="1667" height="1130" alt="image" src="https://github.com/user-attachments/assets/1d406a6f-4175-43b3-818f-e374fefef3a4" />

## **Preview Pane in Android Studio:**

The **Preview Pane** (right side of your screenshot) shows:
1. **Visual representation** of how your UI will look on different devices
2. **Real-time updates** as you edit the XML
3. **Device selector** (Pixel, Nexus, etc.) to test different screen sizes
4. **Theme/Language variants** (App theme, en-us, etc.)

**What you see in Preview:**
- Pixel device mockup
- "Hello World!" TextView displayed
- Blue constraint lines showing how elements are positioned

## **TextView in activity_main.xml:**

**TextView** is a UI widget that displays text to users.

**Key Properties in your layout:**
```xml
<TextView
    android:id="@+id/textView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Hello World!" />
```

**Attributes Explained:**
- `id`: Unique identifier (textView)
- `layout_width/height`: Size ("wrap_content" = size matches text)
- `text`: The displayed text ("Hello World!")

## **What are Constraints?**

**Constraints** are rules that position UI elements relative to:
- Parent container edges
- Other UI elements
- Guidelines or barriers

**Think of constraints as "magnet points"** that stick your view to specific positions.

## **How to Constrain a TextView (Step-by-Step):**

### **From Palette (Left Sidebar):**
1. Drag **TextView** from Palette to Preview
2. Release it in the desired area



### **Adding Constraints Manually:**
1. **Select TextView** in Preview or Component Tree

<img width="1129" height="580" alt="image" src="https://github.com/user-attachments/assets/fd150e0f-f199-400f-bb73-d08e2ed5094b" />

2. **See constraint handles** (circles on each side):
   - Top circle: Constrain to top
   - Bottom circle: Constrain to bottom  
   - Left circle: Constrain to left
   - Right circle: Constrain to right

3. **Click and drag** a constraint handle:
   - Drag to parent edge → sticks to screen edge
   - Drag to another view → sticks to that view
   - Drag to guideline → aligns to guideline



