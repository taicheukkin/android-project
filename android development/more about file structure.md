## **AndroidManifest.xml - The App's ID Card**

### **Location:**
```
app → src → main → manifests → AndroidManifest.xml
```

### **Purpose:**
The **AndroidManifest.xml** is the app's **configuration file** that tells Android system everything about your app.

### **Key Components:**

**1. Package Declaration:**
```xml
package="com.example.myapp"
```
- Unique identifier for your app (like social security number)

**2. Application Tag:**
```xml
<application
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:theme="@style/Theme.MyApp">
```
- Main app configuration
- Icon, name, theme settings

**3. Activities (Screens):**
```xml
<activity
    android:name=".MainActivity"
    android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```
- `MainActivity` = First screen when app opens
- `LAUNCHER` = Appears in app drawer
- `exported="true"` = Other apps can open it

**4. Permissions:**
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.CAMERA" />
```
- What your app needs (camera, internet, location, etc.)

**5. Minimum SDK:**
```xml
<uses-sdk android:minSdkVersion="21" />
```
- Lowest Android version your app supports

**Summary**: Manifest = App's birth certificate + passport + permission slip

---

## **mipmap/ - App Icons Folder**

### **Structure:**
```
mipmap/
├── ic_launcher.png (square icon)
├── ic_launcher_round.png (round icon)
├── mipmap-hdpi/ (different resolutions)
├── mipmap-mdpi/
├── mipmap-xhdpi/
├── mipmap-xxhdpi/
└── mipmap-xxxhdpi/
```

### **1. ic_launcher.png (Square Icon)**
- **Purpose**: Default app icon (square/adaptive icon)
- **Used for**: Most devices, app store listing
- **Size variations**: Different folders for different screen densities
- **Usage in Manifest**: `android:icon="@mipmap/ic_launcher"`

### **2. ic_launcher_round.png (Round Icon)**
- **Purpose**: Circular app icon
- **Used for**: Devices with circular icon styles
- **Auto-generated**: Android Studio creates this from square icon
- **Legacy**: Mostly for older Android versions (pre-8.0)

### **Why Multiple Folders?**
Android devices have different screen densities:
- **mdpi**: Medium (160dpi) - Baseline
- **hdpi**: High (240dpi)
- **xhdpi**: Extra High (320dpi)
- **xxhdpi**: Extra Extra High (480dpi) - Most common today
- **xxxhdpi**: Extra Extra Extra High (640dpi)

**Example**: If you create a 48x48 icon:
- mdpi: 48x48 pixels
- hdpi: 72x72 pixels (48 × 1.5)
- xhdpi: 96x96 pixels (48 × 2)
- xxhdpi: 144x144 pixels (48 × 3)

**Pro Tip**: Always provide all sizes for crisp icons on all devices!

---

## **values/ - App Resources Folder**

### **Structure:**
```
values/
├── colors.xml    - Color definitions
├── strings.xml   - Text content
├── styles.xml    - Themes and styles
└── themes.xml    - App themes (new projects)
```

### **1. colors.xml - Color Palette**
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="purple_200">#FFBB86FC</color>
    <color name="purple_500">#FF6200EE</color>
    <color name="purple_700">#FF3700B3</color>
    <color name="teal_200">#FF03DAC5</color>
    <color name="teal_700">#FF018786</color>
    <color name="black">#FF000000</color>
    <color name="white">#FFFFFFFF</color>
</resources>
```
- **Purpose**: Centralized color management
- **Format**: ARGB hex codes (Alpha-Red-Green-Blue)
- **Usage**: `@color/purple_500` in XML or `R.color.purple_500` in code
- **Benefits**: Change colors in one place, app-wide consistency

### **2. strings.xml - Text Resources**
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">My Application</string>
    <string name="hello_world">Hello World!</string>
    <string name="welcome_message">Welcome to %s!</string>
</resources>
```
- **Purpose**: Store all text strings
- **Benefits**:
  - Easy translation (create `values-es/` for Spanish)
  - Consistent text across app
  - No hardcoded strings in code
- **Usage**: `@string/app_name` in XML, `R.string.app_name` in Kotlin
- **Formatting**: `%s`, `%d` for dynamic content

### **3. themes.xml / styles.xml - App Appearance**
```xml
<!-- themes.xml (for newer projects) -->
<style name="Theme.MyApp" parent="Theme.MaterialComponents.DayNight">
    <item name="colorPrimary">@color/purple_500</item>
    <item name="colorPrimaryVariant">@color/purple_700</item>
    <item name="colorOnPrimary">@color/white</item>
</style>

<!-- styles.xml (for custom widget styles) -->
<style name="MyButtonStyle" parent="Widget.AppCompat.Button">
    <item name="android:textColor">@color/white</item>
    <item name="android:background">@color/purple_500</item>
</style>
```

**Difference:**
- **Themes**: Apply to entire app/activity (colors, fonts, etc.)
- **Styles**: Apply to individual widgets (button style, text style)

**Key Theme Attributes:**
- `colorPrimary`: App's primary brand color
- `colorSecondary`: Accent color
- `colorSurface`: Background color
- `colorOnPrimary`: Text color on primary background

---

## **Summary Table:**

| **File/Folder** | **Purpose** | **Analogy** | **Location** |
|-----------------|-------------|-------------|--------------|
| **AndroidManifest.xml** | App configuration, permissions, activities | Passport + ID card | `manifests/` |
| **ic_launcher.png** | Square app icon | Company logo | `mipmap-*/` |
| **ic_launcher_round.png** | Round app icon (legacy) | Round logo variant | `mipmap-*/` |
| **colors.xml** | Color definitions | Paint palette | `values/` |
| **strings.xml** | Text content | Dictionary | `values/` |
| **themes.xml** | App appearance | Interior design plan | `values/` |

## **Best Practices:**

1. **Never hardcode colors/strings** - Always use resources
2. **Provide all icon densities** - For crisp display on all devices
3. **Keep Manifest clean** - Only declare what you need
4. **Use meaningful names** - `button_primary_color` not `color1`
5. **Separate concerns** - Colors in colors.xml, text in strings.xml

## **Quick Reference Commands:**

**In XML files:**
- Colors: `@color/color_name`
- Strings: `@string/string_name`
- Icons: `@mipmap/ic_launcher`
- Styles: `@style/Theme.MyApp`

**In Kotlin/Java:**
- Colors: `R.color.color_name`
- Strings: `R.string.string_name`
- Icons: `R.mipmap.ic_launcher`

**Pro Tip**: Create separate `colors.xml` for light/dark themes in `values-night/` folder for automatic dark mode support!
