Here's a comprehensive breakdown based on your Android Studio screenshot content:

## **Image Caption:**
<img width="1200" height="717" alt="image" src="https://github.com/user-attachments/assets/ad59ab25-29d4-4aba-a60b-88dd13088eed" />

"Android Studio New Project Setup - Creating an Empty Activity with Kotlin"


## **Process of Creating a New Project in Android Studio:**

1. **Launch Android Studio** and select "New Project"
2. **Choose Template**: Select "Empty Activity" (simplest starting point)
3. **Configure Project**:
   - **Name**: Enter your app name (e.g., "MyApp")
   - **Package name**: Reverse domain format (e.g., com.company.appname)
   - **Save location**: Choose project directory
   - **Language**: Select Kotlin (or Java)
   - **Minimum SDK**: Select API level
4. **Click "Finish"** → Android Studio generates project structure and files

## **What is Minimum SDK?**
**Minimum SDK** is the lowest Android version your app will support. It determines:
- Which devices can install your app
- Which Android features you can use
- **Example**: API 21 = Android 5.0 (Lollipop)
- **Balance needed**: Lower API = more devices, but fewer modern features
- Your screenshot shows 98.8% device coverage with API 21

## **Android Studio Folder Structure (Left Side):**
<img width="1514" height="540" alt="image" src="https://github.com/user-attachments/assets/b8005fa4-99cf-42ae-a926-fab4f71e2006" />

### **Main Folders:**
1. **`manifests/`**
   - `AndroidManifest.xml`: App configuration, permissions, activities declaration

2. **`java/`** (or `kotlin/` for Kotlin projects)
   - Contains all Java/Kotlin source code
   - Organized by package name
   - `MainActivity.kt` = your main screen logic

3. **`res/`** (Resources) - Most important folder:
   - **`layout/`**: XML files for UI design
     - `activity_main.xml` = main screen layout
   - **`values/`**: Strings, colors, styles, dimensions
   - **`drawable/`**: Images, icons, shapes
   - **`mipmap/`**: App launcher icons (different resolutions)

4. **`Gradle Scripts/`**: Build configuration files

## **activity_main.xml vs MainActivity.kt - The Difference:**

### **`activity_main.xml`** (in `res/layout/` folder)
- **Purpose**: Defines the **UI/View** (what users see)
- **Format**: XML file
- **Contains**: Buttons, TextViews, Layouts, etc.
- **Visual**: Can edit in Design view or Code view
- **Example content**: Layout structure, widget properties

### **`MainActivity.kt`** (in `java/` or `kotlin/` folder)
- **Purpose**: Contains the **logic/behavior** (what the app does)
- **Format**: Kotlin/Java code file
- **Contains**: Event handlers, business logic, data processing
- **Connects to**: XML layout via `setContentView(R.layout.activity_main)`
- **Example content**: Button click listeners, data calculations

## **How to Find activity_main.xml:**

1. **In Android Studio Project View:**
   ```
   app → src → main → res → layout → activity_main.xml
   ```

2. **Quick Navigation:**
   - Use **Project tab** on left (set to "Android" view)
   - Expand `app` → `res` → `layout`
   - Double-click `activity_main.xml` to open

3. **Alternative Method:**
   - In `MainActivity.kt`, find line: `setContentView(R.layout.activity_main)`
   - **Ctrl+Click** (Cmd+Click on Mac) on `R.layout.activity_main`
   - Android Studio will open the XML file directly

4. **Search Shortcut**:
   - Press `Shift` twice → type "activity_main" → select file

**Key Relationship**: 
- `MainActivity.kt` **controls** the behavior
- `activity_main.xml` **displays** the interface
- They work together: Activity inflates (loads) the XML layout

**Tip**: When learning Android, start by modifying `activity_main.xml` for UI changes, and `MainActivity.kt` for functional changes.
