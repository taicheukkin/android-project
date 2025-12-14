# Android多窗口开发快速入门指南

## 🎯 **今日学习计划（3小时）**

### **第1小时：环境配置与基础认知**
- 0-15分钟：安装Android Studio
- 15-30分钟：配置Kotlin开发环境
- 30-45分钟：创建第一个项目
- 45-60分钟：了解Android项目结构（对比Python/JS项目）

### **第2小时：Kotlin速成与Android基础**
- 0-20分钟：Kotlin基础语法（对比Python）
- 20-40分钟：Android四大组件简介
- 40-60分钟：Activity生命周期理解

### **第3小时：第一个多窗口Demo**
- 0-30分钟：实现悬浮窗基础
- 30-50分钟：添加拖拽功能
- 50-60分钟：调试运行，问题解决

## 🛠️ **环境配置步骤**

### **1. 安装Android Studio**
```bash
# 下载地址：https://developer.android.com/studio
# 选择对应系统版本下载安装

# 安装时注意勾选：
# - Android SDK
# - Android Virtual Device (AVD)
# - Kotlin插件（通常默认包含）
```

### **2. 配置开发环境**
```bash
# 安装完成后：
1. 打开Android Studio
2. 选择"New Project"
3. 选择"Empty Activity"模板
4. 配置项目：
   - Name: MultiWindowDemo
   - Package name: com.yourname.multiwindow
   - Language: Kotlin
   - Minimum SDK: API 24 (Android 7.0) - 支持多窗口
```

### **3. 配置虚拟设备（AVD）**
```
1. 点击AVD Manager图标
2. 创建虚拟设备：
   - 选择Pixel 5或类似设备
   - 系统镜像：API 30+（Android 11+）
   - 确保启用多窗口支持
```

## 📱 **第一个多窗口Demo代码示例**

### **1. 创建主Activity**
```kotlin
// MainActivity.kt
package com.yourname.multiwindow

import android.content.Intent
import android.net.Uri
import android.os.Build
import android.os.Bundle
import android.provider.Settings
import android.widget.Button
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    
    private val OVERLAY_PERMISSION_REQUEST_CODE = 1001
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        // 检查悬浮窗权限
        checkOverlayPermission()
        
        // 设置按钮点击事件
        findViewById<Button>(R.id.btn_create_window).setOnClickListener {
            createFloatingWindow()
        }
        
        findViewById<Button>(R.id.btn_multi_window).setOnClickListener {
            enableMultiWindowMode()
        }
    }
    
    // 检查悬浮窗权限
    private fun checkOverlayPermission() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            if (!Settings.canDrawOverlays(this)) {
                val intent = Intent(
                    Settings.ACTION_MANAGE_OVERLAY_PERMISSION,
                    Uri.parse("package:$packageName")
                )
                startActivityForResult(intent, OVERLAY_PERMISSION_REQUEST_CODE)
            }
        }
    }
    
    // 创建悬浮窗口
    private fun createFloatingWindow() {
        // 启动悬浮窗服务
        val intent = Intent(this, FloatingWindowService::class.java)
        
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            startForegroundService(intent)
        } else {
            startService(intent)
        }
        
        Toast.makeText(this, "悬浮窗已创建", Toast.LENGTH_SHORT).show()
    }
    
    // 启用多窗口模式
    private fun enableMultiWindowMode() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
            if (!isInMultiWindowMode) {
                Toast.makeText(this, "请长按最近任务键进入分屏", Toast.LENGTH_LONG).show()
            }
        } else {
            Toast.makeText(this, "需要Android 7.0以上版本", Toast.LENGTH_SHORT).show()
        }
    }
    
    // 处理权限请求结果
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        if (requestCode == OVERLAY_PERMISSION_REQUEST_CODE) {
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
                if (Settings.canDrawOverlays(this)) {
                    Toast.makeText(this, "悬浮窗权限已获取", Toast.LENGTH_SHORT).show()
                }
            }
        }
    }
}
```

### **2. 创建悬浮窗服务**
```kotlin
// FloatingWindowService.kt
package com.yourname.multiwindow

import android.app.Service
import android.content.Intent
import android.graphics.PixelFormat
import android.os.Build
import android.os.IBinder
import android.view.Gravity
import android.view.LayoutInflater
import android.view.MotionEvent
import android.view.View
import android.view.WindowManager
import android.widget.Button
import android.widget.Toast

class FloatingWindowService : Service() {
    
    private lateinit var windowManager: WindowManager
    private lateinit var floatingView: View
    private var initialX = 0
    private var initialY = 0
    private var initialTouchX = 0f
    private var initialTouchY = 0f
    
    override fun onBind(intent: Intent?): IBinder? = null
    
    override fun onCreate() {
        super.onCreate()
        // 创建前台服务通知（Android 8.0+需要）
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            startForeground(1, NotificationHelper.createNotification(this))
        }
        
        createFloatingWindow()
    }
    
    private fun createFloatingWindow() {
        windowManager = getSystemService(WINDOW_SERVICE) as WindowManager
        
        // 创建悬浮窗布局参数
        val params = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            WindowManager.LayoutParams(
                WindowManager.LayoutParams.WRAP_CONTENT,
                WindowManager.LayoutParams.WRAP_CONTENT,
                WindowManager.LayoutParams.TYPE_APPLICATION_OVERLAY,
                WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE,
                PixelFormat.TRANSLUCENT
            )
        } else {
            WindowManager.LayoutParams(
                WindowManager.LayoutParams.WRAP_CONTENT,
                WindowManager.LayoutParams.WRAP_CONTENT,
                WindowManager.LayoutParams.TYPE_PHONE,
                WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE,
                PixelFormat.TRANSLUCENT
            )
        }
        
        // 设置窗口位置和重力
        params.gravity = Gravity.TOP or Gravity.START
        params.x = 100
        params.y = 100
        
        // 加载布局
        val inflater = getSystemService(LAYOUT_INFLATER_SERVICE) as LayoutInflater
        floatingView = inflater.inflate(R.layout.floating_window, null)
        
        // 设置拖拽监听
        floatingView.setOnTouchListener(object : View.OnTouchListener {
            override fun onTouch(v: View, event: MotionEvent): Boolean {
                when (event.action) {
                    MotionEvent.ACTION_DOWN -> {
                        // 记录初始位置
                        initialX = params.x
                        initialY = params.y
                        initialTouchX = event.rawX
                        initialTouchY = event.rawY
                        return true
                    }
                    MotionEvent.ACTION_MOVE -> {
                        // 计算移动距离
                        params.x = initialX + (event.rawX - initialTouchX).toInt()
                        params.y = initialY + (event.rawY - initialTouchY).toInt()
                        windowManager.updateViewLayout(floatingView, params)
                        return true
                    }
                    MotionEvent.ACTION_UP -> {
                        // 点击事件处理
                        if (Math.abs(event.rawX - initialTouchX) < 10 &&
                            Math.abs(event.rawY - initialTouchY) < 10) {
                            // 点击事件
                            Toast.makeText(
                                this@FloatingWindowService,
                                "悬浮窗被点击",
                                Toast.LENGTH_SHORT
                            ).show()
                        }
                        return true
                    }
                }
                return false
            }
        })
        
        // 关闭按钮
        floatingView.findViewById<Button>(R.id.btn_close).setOnClickListener {
            stopSelf()
        }
        
        // 添加悬浮窗
        windowManager.addView(floatingView, params)
    }
    
    override fun onDestroy() {
        super.onDestroy()
        if (::floatingView.isInitialized) {
            windowManager.removeView(floatingView)
        }
    }
}
```

### **3. 布局文件**
```xml
<!-- res/layout/activity_main.xml -->
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp"
    android:gravity="center">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="多窗口演示"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="32dp"/>

    <Button
        android:id="@+id/btn_create_window"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="创建悬浮窗"
        android:layout_marginBottom="16dp"/>

    <Button
        android:id="@+id/btn_multi_window"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="启用分屏模式"/>

</LinearLayout>
```

```xml
<!-- res/layout/floating_window.xml -->
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="200dp"
    android:layout_height="150dp"
    android:orientation="vertical"
    android:background="@android:color/white"
    android:elevation="8dp">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:gravity="center"
        android:text="悬浮窗口"
        android:textSize="18sp"
        android:background="#2196F3"
        android:textColor="@android:color/white"/>

    <Button
        android:id="@+id/btn_close"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="关闭"
        android:layout_margin="8dp"/>

</LinearLayout>
```

### **4. 通知工具类（Android 8.0+需要）**
```kotlin
// NotificationHelper.kt
package com.yourname.multiwindow

import android.app.Notification
import android.app.NotificationChannel
import android.app.NotificationManager
import android.app.PendingIntent
import android.content.Context
import android.content.Intent
import android.os.Build

object NotificationHelper {
    
    private const val CHANNEL_ID = "FloatingWindowChannel"
    private const val CHANNEL_NAME = "悬浮窗服务"
    
    fun createNotification(context: Context): Notification {
        createNotificationChannel(context)
        
        val intent = Intent(context, MainActivity::class.java)
        val pendingIntent = PendingIntent.getActivity(
            context, 0, intent,
            PendingIntent.FLAG_IMMUTABLE or PendingIntent.FLAG_UPDATE_CURRENT
        )
        
        return if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            Notification.Builder(context, CHANNEL_ID)
                .setContentTitle("悬浮窗服务运行中")
                .setContentText("点击返回应用")
                .setSmallIcon(R.drawable.ic_launcher_foreground)
                .setContentIntent(pendingIntent)
                .build()
        } else {
            Notification.Builder(context)
                .setContentTitle("悬浮窗服务运行中")
                .setContentText("点击返回应用")
                .setSmallIcon(R.drawable.ic_launcher_foreground)
                .setContentIntent(pendingIntent)
                .build()
        }
    }
    
    private fun createNotificationChannel(context: Context) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel(
                CHANNEL_ID,
                CHANNEL_NAME,
                NotificationManager.IMPORTANCE_LOW
            ).apply {
                description = "悬浮窗服务通知频道"
            }
            
            val notificationManager = context.getSystemService(
                NotificationManager::class.java
            )
            notificationManager.createNotificationChannel(channel)
        }
    }
}
```

### **5. 清单文件配置**
```xml
<!-- AndroidManifest.xml 需要添加 -->
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
<uses-permission android:name="android.permission.FOREGROUND_SERVICE" />

<application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:supportsRtl="true"
    android:theme="@style/Theme.MultiWindowDemo"
    android:resizeableActivity="true">  <!-- 启用多窗口支持 -->
    
    <activity
        android:name=".MainActivity"
        android:exported="true"
        android:configChanges="orientation|screenSize|smallestScreenSize|screenLayout"
        android:launchMode="singleTask">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
    
    <service
        android:name=".FloatingWindowService"
        android:enabled="true"
        android:exported="false" />
    
</application>
```

## 🚨 **常见问题及解决方案**

### **问题1：悬浮窗权限被拒绝**
```kotlin
// 解决方案：引导用户手动开启
private fun requestOverlayPermission() {
    val intent = Intent(Settings.ACTION_MANAGE_OVERLAY_PERMISSION).apply {
        data = Uri.parse("package:$packageName")
        flags = Intent.FLAG_ACTIVITY_NEW_TASK
    }
    
    try {
        startActivity(intent)
    } catch (e: Exception) {
        Toast.makeText(this, "无法打开权限设置", Toast.LENGTH_SHORT).show()
        // 备用方案：打开应用详情页
        val appDetailsIntent = Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS).apply {
            data = Uri.parse("package:$packageName")
        }
        startActivity(appDetailsIntent)
    }
}
```

### **问题2：悬浮窗在Android 10+不显示**
```kotlin
// 原因：TYPE_PHONE在Android 8.0+被废弃
// 解决方案：使用TYPE_APPLICATION_OVERLAY
val params = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
    WindowManager.LayoutParams(
        WindowManager.LayoutParams.WRAP_CONTENT,
        WindowManager.LayoutParams.WRAP_CONTENT,
        WindowManager.LayoutParams.TYPE_APPLICATION_OVERLAY,  // 关键更改
        WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE,
        PixelFormat.TRANSLUCENT
    )
} else {
    WindowManager.LayoutParams(
        WindowManager.LayoutParams.WRAP_CONTENT,
        WindowManager.LayoutParams.WRAP_CONTENT,
        WindowManager.LayoutParams.TYPE_PHONE,
        WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE,
        PixelFormat.TRANSLUCENT
    )
}
```

### **问题3：服务被系统杀死**
```kotlin
// 解决方案：使用前台服务并添加保活策略
class FloatingWindowService : Service() {
    
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        // 返回START_STICKY使服务被杀死后重启
        return START_STICKY
    }
    
    override fun onCreate() {
        super.onCreate()
        // 创建前台服务通知
        startForeground(1, NotificationHelper.createNotification(this))
        
        // 设置为前台服务
        val notification = NotificationHelper.createNotification(this)
        startForeground(1, notification)
    }
}
```

### **问题4：多窗口生命周期问题**
```kotlin
// 在Activity中处理多窗口变化
class MainActivity : AppCompatActivity() {
    
    override fun onMultiWindowModeChanged(isInMultiWindowMode: Boolean) {
        super.onMultiWindowModeChanged(isInMultiWindowMode)
        
        if (isInMultiWindowMode) {
            // 进入多窗口模式
            Toast.makeText(this, "已进入多窗口模式", Toast.LENGTH_SHORT).show()
            // 调整UI适配小窗口
            adjustLayoutForMultiWindow()
        } else {
            // 退出多窗口模式
            Toast.makeText(this, "已退出多窗口模式", Toast.LENGTH_SHORT).show()
            restoreFullScreenLayout()
        }
    }
    
    private fun adjustLayoutForMultiWindow() {
        // 简化UI，调整布局
        findViewById<Button>(R.id.btn_create_window).text = "浮窗"
    }
    
    private fun restoreFullScreenLayout() {
        // 恢复完整UI
        findViewById<Button>(R.id.btn_create_window).text = "创建悬浮窗"
    }
}
```

### **问题5：输入法遮挡悬浮窗**
```kotlin
// 解决方案：调整窗口参数
val params = WindowManager.LayoutParams(
    WindowManager.LayoutParams.WRAP_CONTENT,
    WindowManager.LayoutParams.WRAP_CONTENT,
    WindowManager.LayoutParams.TYPE_APPLICATION_OVERLAY,
    WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE or
    WindowManager.LayoutParams.FLAG_LAYOUT_NO_LIMITS or  // 允许超出边界
    WindowManager.LayoutParams.FLAG_NOT_TOUCH_MODAL,     // 允许触摸穿透
    PixelFormat.TRANSLUCENT
)
```

## 📝 **今日学习总结**

### **你已经学会的：**
1. ✅ 配置Android开发环境
2. ✅ 理解Kotlin基础语法
3. ✅ 创建悬浮窗并实现拖拽
4. ✅ 处理Android权限系统
5. ✅ 了解多窗口生命周期

### **明日学习建议：**
1. 实现多个悬浮窗共存
2. 学习AIDL进程间通信
3. 尝试控制外部App
4. 优化内存管理

### **Python/JS开发者注意：**
```python
# Python思维迁移：
# 1. Android Activity ≈ Flask/Django的View
# 2. Intent ≈ HTTP Request
# 3. Service ≈ 后台任务/Worker
# 4. WindowManager ≈ 浏览器window对象

# JavaScript思维迁移：
# 1. Kotlin协程 ≈ async/await
# 2. Activity生命周期 ≈ React生命周期
# 3. Layout XML ≈ HTML/CSS
# 4. View系统 ≈ DOM操作
```

**恭喜！** 你在3小时内完成了Android多窗口开发的第一步。明天可以继续深入，实现更复杂的多窗口管理功能！ 🚀

遇到问题时，随时可以用这个模式向AI提问：
```
问题描述：[具体问题]
代码：[相关代码]
日志：[错误信息]
我已经尝试：[你的尝试]
```
