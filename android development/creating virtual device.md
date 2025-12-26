

## **Complete Process Summary: Creating Virtual Device in Android Studio**

### **Step 1: Open Device Manager**
- Click **Device Manager** icon in toolbar
- Or go to **Tools → Device Manager**
- Shows "No Devices" initially
<img width="670" height="283" alt="image" src="https://github.com/user-attachments/assets/54909603-fc36-49ae-86a3-fa85b8fd70f0" />

### **Step 2: Create Virtual Device**

<img width="382" height="520" alt="image" src="https://github.com/user-attachments/assets/2dc49fb9-fd08-4c28-b26a-ac210f18afaa" />

1. Click **"Create Device"** or **"Create Virtual Device"** button
2. Opens **"Select Hardware"** screen

### **Step 3: Choose Device Definition**

<img width="1479" height="990" alt="image" src="https://github.com/user-attachments/assets/5c37dd9d-3c4c-4057-9210-55eef61d72ca" />

- **Categories**: Phone, Tablet, Wear OS, TV, Automotive
- **Select device model** (e.g., Pixel 6 Pro)
- **View specifications**:
  - **Size**: Screen size (6.7")
  - **Resolution**: 1440x3120 pixels
  - **Density**: 560dpi (dots per inch)
- **Options**:
  - **Clone Device**: Copy settings
  - **New Hardware Profile**: Custom device
  - **Import Hardware Profiles**: Import existing

### **Step 4: Select System Image**

<img width="1777" height="998" alt="image" src="https://github.com/user-attachments/assets/70def686-b06e-44d3-9352-da2edf17a49f" />

- **Choose Android version** (API level):
  - **Tramisu** = Android 13 (API 33)
  - **S** = Android 12 (API 31-32)
  - **R** = Android 11 (API 30)
  - **Q** = Android 10 (API 29)
  - etc.
- **ABI Type**:
  - **x86_64**: For modern computers (recommended)
  - **x86**: Older computers
  - **arm**: For ARM processors
- **Target**:
  - **Google APIs**: Includes Google services
  - **Android (Google Play)**: For Play Store testing
  - **Vanilla Android**: Basic version

### **Step 5: Verify Configuration**

<img width="1820" height="1033" alt="image" src="https://github.com/user-attachments/assets/1b11b2e1-2c09-44e4-a6b9-40e063e31e66" />

- **AVD Name**: Pixel 6 Pro API 30 (auto-generated or custom)
- **Device**: Confirms selected hardware
- **System Image**: Confirms Android version
- **Advanced Settings**:
  - **Startup orientation**: Portrait/Landscape
  - **Graphics**: Automatic/Hardware/Software
  - **Camera**: Front/Back camera emulation
  - **Network**: Speed/latency simulation

### **Step 6: Download System Image (If Needed)**
- If selected image isn't downloaded:
  - Click **"Download"** link
  - Accept license agreement
  - Wait for download (500MB-2GB)
  - Automatically continues after download

### **Step 7: Finish Creation**
- Click **"Finish"** → AVD appears in Device Manager
- Shows:
  - Device name
  - API level
  - Size on disk (usually 1-4GB)
  - Action buttons (Play, Edit, Delete)

## **Key Concepts Explained:**

### **What is an AVD?**
Android Virtual Device = Software emulator that mimics:
- Real Android hardware
- Specific Android OS version
- Device characteristics (screen size, memory, sensors)

### **Why Create Virtual Devices?**
1. **Test without physical devices**
2. **Multiple device testing** (different screen sizes/Android versions)
3. **Debugging**: Easy screenshots, logs, testing
4. **Safe testing**: Won't crash your personal phone

### **Important Settings:**

**API Level Choice:**
- **Match your app's minSdk**: Test on lowest supported version
- **Latest API**: Test newest features
- **Multiple AVDs**: Create different versions for testing

**Graphics Options:**
- **Automatic**: Android Studio decides
- **Hardware**: Uses computer's GPU (faster)
- **Software**: CPU-based (slower but compatible)

**Storage on Disk:**
- **System Image**: 1-2GB (Android OS)
- **User Data**: Additional space for apps/files
- **Total**: Typically 3-6GB per AVD

## **Troubleshooting Tips:**

1. **Slow Performance**:
   - Use **x86/x86_64** images (not ARM)
   - Enable **VT-x/AMD-V** in BIOS
   - Allocate more RAM to AVD

2. **Not Showing in Device List**:
   - Wait for emulator to fully boot (2-5 minutes first time)
   - Check if **ADB** is connected
   - Restart Android Studio

3. **Download Failures**:
   - Check internet connection
   - Use SDK Manager to pre-download images
   - Try different mirror site

## **After Creation:**
<img width="592" height="561" alt="image" src="https://github.com/user-attachments/assets/36920a59-0948-43c2-b94d-fd4daa8dc54a" />

- Click **Play button (▶)** to start emulator
- First boot takes 2-5 minutes
- Subsequent starts: 30-60 seconds
- Use like a real phone: install apps, test features

**Pro Tip**: Create multiple AVDs for different test scenarios:
1. **Low-end device**: Small screen, low RAM
2. **High-end device**: Large screen, latest Android
3. **Tablet**: Different aspect ratio
4. **Old Android**: Test backward compatibility
