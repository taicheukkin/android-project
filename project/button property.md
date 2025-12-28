Here is a comprehensive summary of all properties available for the Android `Button` widget, categorized for clarity. Remember that **`Button` extends `TextView`**, so it inherits all `TextView` properties while adding its own visual and behavioral enhancements.

## ✅ **Core Button-Specific Properties**
These are properties that are particularly important or unique to `Button` functionality:

| Property | XML Attribute | Description | Typical Values |
|:---|:---|:---|:---|
| **Text** | `android:text` | Label displayed on the button | `"Submit"`, `@string/button_label` |
| **Background** | `android:background` | Visual background (color, shape, selector) | `@drawable/button_selector`, `#2196F3` |
| **Text Color** | `android:textColor` | Color of the button's text label | `#FFFFFF`, `@color/white` |
| **Click Handler** | `android:onClick` | Method to call in Activity when clicked | `"onButtonClick"` |
| **State List Animator** | `android:stateListAnimator` | Elevation/animation changes for different states | `@animator/button_state_animator` |
| **All Caps** | `android:textAllCaps` | Forces text to uppercase (deprecated in API 26+) | `true`, `false` |

## 🔄 **Visual States (Using Background Selectors)**
A `Button`'s appearance typically changes based on its state (pressed, disabled, focused). This is controlled via a **drawable selector** (`button_background.xml` in `res/drawable/`):

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true" 
          android:drawable="@drawable/button_pressed" /> <!-- Pressed state -->
    <item android:state_enabled="false" 
          android:drawable="@drawable/button_disabled" /> <!-- Disabled state -->
    <item android:drawable="@drawable/button_normal" /> <!-- Default state -->
</selector>
```
Then apply it: `android:background="@drawable/button_background"`

## 📋 **Inherited TextView Properties (Most Important)**
Since `Button` is a subclass of `TextView`, all these properties work identically:

| Category | Key Properties | Example Usage |
|:---|:---|:---|
| **Text Styling** | `textSize`, `textStyle`, `fontFamily`, `typeface` | `android:textSize="14sp"`, `android:textStyle="bold"` |
| **Text Appearance** | `textAppearance` | `android:textAppearance="?attr/textAppearanceButton"` |
| **Layout** | `layout_width`, `layout_height`, `layout_margin`, `padding` | `android:layout_width="wrap_content"` |
| **Gravity** | `gravity` (text alignment within button) | `android:gravity="center"` |
| **Compound Drawables** | `drawableStart`, `drawableTop`, `drawableEnd`, `drawableBottom`, `drawablePadding` | `android:drawableStart="@drawable/icon"` |

## 🎨 **Material Button Properties (Recommended Modern Approach)**
For modern apps, use `com.google.android.material.button.MaterialButton` (add `implementation 'com.google.android.material:material:1.x.x'` to dependencies):

| Property | XML Attribute | Description |
|:---|:---|:---|
| **Icon** | `app:icon` | Adds an icon to the button |
| **Icon Gravity** | `app:iconGravity` | Icon position (`start`, `end`, `textStart`, `textEnd`) |
| **Icon Size** | `app:iconSize` | Size of the icon |
| **Icon Padding** | `app:iconPadding` | Space between icon and text |
| **Corner Radius** | `app:cornerRadius` | Rounded corners radius |
| **Stroke Color** | `app:strokeColor` | Border/outline color |
| **Stroke Width** | `app:strokeWidth` | Border/outline thickness |
| **Ripple Color** | `app:rippleColor` | Color of the touch ripple effect |

## 💻 **Complete XML Example (Traditional Button)**
```xml
<Button
    android:id="@+id/submitButton"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="SUBMIT"
    android:textSize="16sp"
    android:textColor="#FFFFFF"
    android:textAllCaps="false"
    android:background="@drawable/button_background_selector"
    android:onClick="onSubmitClicked"
    android:paddingStart="24dp"
    android:paddingEnd="24dp"
    android:paddingTop="12dp"
    android:paddingBottom="12dp"
    android:gravity="center"
    android:drawableStart="@drawable/ic_check"
    android:drawablePadding="8dp" />
```

## 💡 **Key Best Practices**
1.  **Use MaterialButton**: For modern, consistent design with built-in elevation, ripple effects, and accessibility.
2.  **Provide visual feedback**: Always implement different visual states (pressed, disabled) using selectors.
3.  **Handle clicks properly**: Prefer `setOnClickListener` in code over `android:onClick` for better control and testing.
4.  **Mind touch targets**: Ensure buttons are at least **48dp x 48dp** in size for accessibility, even if visually smaller.
5.  **Use strings.xml**: Never hardcode button text (`android:text="@string/submit_button"`).
6.  **Consider shape**: Use `shape` drawables or MaterialButton's `cornerRadius` for rounded buttons.
7.  **Test states**: Always test your button in enabled, disabled, pressed, and focused states.
