Here is a comprehensive summary of the most important and commonly used properties for the Android `TextView` widget, categorized by function:

## 📝 **Core Text & Content Properties**
| Property | XML Attribute | Description | Typical Value |
|:---|:---|:---|:---|
| **Text** | `android:text` | The text string to display | `"Hello World"`, `@string/resource` |
| **Text Color** | `android:textColor` | Color of the text | `#FF0000` (Red), `@color/primary` |
| **Text Size** | `android:textSize` | Size of the text (with unit) | `"16sp"`, `"18dp"` |
| **Hint** | `android:hint` | Hint text when TextView is empty | `"Enter your name"` |
| **Hint Color** | `android:textColorHint` | Color of the hint text | `#808080` (Gray) |

## 🎨 **Appearance & Styling**
| Property | XML Attribute | Description | Typical Value |
|:---|:---|:---|:---|
| **Background** | `android:background` | Background color/drawable | `@color/white`, `@drawable/rounded_bg` |
| **Typeface/Font** | `android:fontFamily` | Font family for the text | `"sans-serif"`, `"monospace"` |
| **Style** | `android:textStyle` | Text style (bold, italic, etc.) | `"bold"`, `"italic"`, `"normal"` |
| **Gravity** | `android:gravity` | Alignment of text within TextView bounds | `"center"`, `"start\|top"`, `"end"` |
| **Max Lines** | `android:maxLines` | Maximum number of lines to display | `"1"`, `"3"` |
| **Ellipsize** | `android:ellipsize` | How to handle overflow text (`end`, `start`, `middle`, `marquee`) | `"end"` |

## 📏 **Layout & Sizing**
| Property | XML Attribute | Description | Typical Value |
|:---|:---|:---|:---|
| **Width** | `android:layout_width` | Width of the TextView | `"match_parent"`, `"wrap_content"`, `"100dp"` |
| **Height** | `android:layout_height` | Height of the TextView | `"match_parent"`, `"wrap_content"`, `"50dp"` |
| **Padding** | `android:padding` | Space between content and TextView edges | `"8dp"`, `"16dp"` |
| **Compound Drawables** | `android:drawableStart`, `android:drawableTop`, etc. | Drawables around text (icons) | `@drawable/icon` |

## 🔗 **Interactive & Advanced**
| Property | XML Attribute | Description | Typical Value |
|:---|:---|:---|:---|
| **Clickable** | `android:clickable` | Whether TextView responds to clicks | `"true"`, `"false"` |
| **Links** | `android:autoLink` | Automatically detect and linkify (web, email, phone, etc.) | `"web"`, `"all"`, `"none"` |
| **Movement Method** | Programmatic (Java/Kotlin) | Allows scrolling, link clicking | `LinkMovementMethod.getInstance()` |
| **Text Appearance** | `android:textAppearance` | Predefined style reference | `"?android:attr/textAppearanceMedium"` |

## 💻 **Example in XML**
```xml
<TextView
    android:id="@+id/myTextView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Hello Android!"
    android:textSize="18sp"
    android:textColor="#FF5722"
    android:textStyle="bold"
    android:gravity="center"
    android:background="@drawable/rounded_bg"
    android:maxLines="2"
    android:ellipsize="end"
    android:drawableStart="@drawable/icon_info"
    android:padding="12dp" />
```

## 🚀 **Key Points to Remember**
1.  **Use `sp` for text sizes** (`android:textSize`) to respect user's system font scale preferences.
2.  **Always use string resources** (`@string/...`) instead of hardcoding text directly for internationalization.
3.  **For complex styling**, use `SpannableString` programmatically or HTML formatting.
4.  **Performance tip**: Avoid nesting multiple `TextView` widgets; use a single `TextView` with `SpannableString` for mixed styling.
5.  **Material Design**: Consider using `MaterialTextView` from the Material Components library for modern design features.

These properties cover about 90% of common `TextView` use cases. The actual full list is much longer, but mastering these will give you full control over text display in Android apps.
