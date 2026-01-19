Oke gas! 🔥 Sekarang **PHASE-3-GUIDE.md**:

---

```markdown
# Phase 3: Advanced Features Development Guide

**Branch:** `phase-3-advanced-features`  
**Duration:** 8 weeks  
**Status:** 🔵 Not Started  
**Prerequisites:** Phase 1 & 2 completed

---

## 📋 **Overview**

Phase 3 transforms Mix Launcher from a functional dev tool into a fully-featured, customizable launcher. This phase adds custom notification system, advanced gestures, comprehensive theme engine, plugin architecture, and recent apps management.

---

## 🎯 **Milestone Objectives**

- ✅ Custom notification system with quick settings
- ✅ Advanced gesture controls (swipe, pinch, double tap)
- ✅ Full theme engine with icon pack support
- ✅ Plugin system for extensibility
- ✅ Recent apps / task switcher
- ✅ Material You dynamic colors support

---

## 🏗️ **Module Structure**

### **Feature Modules**

#### **feature/notifications**
```
feature/notifications/
└── src/main/java/com/mixlauncher/feature/notifications/
    ├── NotificationShade.kt
    ├── NotificationViewModel.kt
    ├── NotificationState.kt
    ├── NotificationEvent.kt
    │
    ├── 📂 listener/
    │   ├── LauncherNotificationListener.kt
    │   └── NotificationParser.kt
    │
    ├── 📂 ui/
    │   ├── NotificationCard.kt
    │   ├── QuickSettings.kt
    │   ├── NotificationActions.kt
    │   └── NotificationGroup.kt
    │
    └── 📂 manager/
        ├── NotificationManager.kt
        └── NotificationStorage.kt
```

#### **feature/gestures**
```
feature/gestures/
└── src/main/java/com/mixlauncher/feature/gestures/
    ├── GestureDetector.kt
    ├── GestureViewModel.kt
    ├── GestureState.kt
    │
    ├── 📂 types/
    │   ├── SwipeGesture.kt
    │   ├── PinchGesture.kt
    │   ├── DoubleTapGesture.kt
    │   └── LongPressGesture.kt
    │
    └── 📂 actions/
        ├── GestureAction.kt
        ├── ActionExecutor.kt
        └── ActionRegistry.kt
```

#### **feature/themes**
```
feature/themes/
└── src/main/java/com/mixlauncher/feature/themes/
    ├── ThemeManager.kt
    ├── ThemeViewModel.kt
    ├── ThemeState.kt
    │
    ├── 📂 engine/
    │   ├── ThemeEngine.kt
    │   ├── ColorScheme.kt
    │   ├── DynamicColors.kt
    │   └── ThemeApplier.kt
    │
    ├── 📂 presets/
    │   ├── DarkTheme.kt
    │   ├── LightTheme.kt
    │   ├── CustomTheme.kt
    │   └── MaterialYouTheme.kt
    │
    └── 📂 iconpacks/
        ├── IconPackManager.kt
        ├── IconPackParser.kt
        └── IconPackApplier.kt
```

#### **feature/recents**
```
feature/recents/
└── src/main/java/com/mixlauncher/feature/recents/
    ├── RecentsScreen.kt
    ├── RecentsViewModel.kt
    ├── RecentsState.kt
    │
    ├── 📂 ui/
    │   ├── TaskCard.kt
    │   ├── TaskSwitcher.kt
    │   ├── AppKiller.kt
    │   └── TaskPreview.kt
    │
    └── 📂 manager/
        ├── TaskManager.kt
        └── RecentAppsProvider.kt
```

#### **feature/plugins**
```
feature/plugins/
└── src/main/java/com/mixlauncher/feature/plugins/
    ├── PluginManager.kt
    ├── PluginViewModel.kt
    ├── PluginState.kt
    │
    ├── 📂 api/
    │   ├── PluginAPI.kt
    │   ├── PluginInterface.kt
    │   └── PluginContext.kt
    │
    ├── 📂 loader/
    │   ├── PluginLoader.kt
    │   ├── PluginValidator.kt
    │   └── PluginSandbox.kt
    │
    └── 📂 store/
        ├── PluginStore.kt
        ├── PluginDownloader.kt
        └── PluginRepository.kt
```

### **Core Modules**

#### **core/plugin-api**
```
core/plugin-api/
└── src/main/java/com/mixlauncher/core/pluginapi/
    ├── LauncherPlugin.kt
    ├── PluginCallback.kt
    ├── PluginMetadata.kt
    └── PluginContext.kt
```

#### **core/analytics**
```
core/analytics/
└── src/main/java/com/mixlauncher/core/analytics/
    ├── AnalyticsManager.kt
    ├── EventTracker.kt
    └── 📂 events/
        ├── AppLaunchEvent.kt
        ├── ScreenViewEvent.kt
        └── UserActionEvent.kt
```

---

## 💻 **Key Implementation Files**

### **1. LauncherNotificationListener.kt**

```kotlin
// feature/notifications/src/main/java/com/mixlauncher/feature/notifications/listener/LauncherNotificationListener.kt

package com.mixlauncher.feature.notifications.listener

import android.content.Intent
import android.service.notification.NotificationListenerService
import android.service.notification.StatusBarNotification
import com.mixlauncher.feature.notifications.manager.NotificationManager
import dagger.hilt.android.AndroidEntryPoint
import timber.log.Timber
import javax.inject.Inject

@AndroidEntryPoint
class LauncherNotificationListener : NotificationListenerService() {
    
    @Inject
    lateinit var notificationManager: NotificationManager
    
    companion object {
        const val ACTION_NOTIFICATION_POSTED = "com.mixlauncher.NOTIFICATION_POSTED"
        const val ACTION_NOTIFICATION_REMOVED = "com.mixlauncher.NOTIFICATION_REMOVED"
    }
    
    override fun onCreate() {
        super.onCreate()
        Timber.d("NotificationListener created")
    }
    
    override fun onNotificationPosted(sbn: StatusBarNotification) {
        Timber.d("Notification posted: ${sbn.packageName}")
        
        val notification = NotificationParser.parse(sbn)
        notificationManager.addNotification(notification)
        
        // Broadcast to notification shade
        sendBroadcast(Intent(ACTION_NOTIFICATION_POSTED).apply {
            putExtra("notification_key", sbn.key)
        })
    }
    
    override fun onNotificationRemoved(sbn: StatusBarNotification) {
        Timber.d("Notification removed: ${sbn.packageName}")
        
        notificationManager.removeNotification(sbn.key)
        
        // Broadcast removal
        sendBroadcast(Intent(ACTION_NOTIFICATION_REMOVED).apply {
            putExtra("notification_key", sbn.key)
        })
    }
    
    override fun onListenerConnected() {
        super.onListenerConnected()
        Timber.d("NotificationListener connected")
        
        // Load existing notifications
        activeNotifications?.forEach { sbn ->
            val notification = NotificationParser.parse(sbn)
            notificationManager.addNotification(notification)
        }
    }
    
    override fun onListenerDisconnected() {
        super.onListenerDisconnected()
        Timber.d("NotificationListener disconnected")
    }
}
```

### **2. NotificationParser.kt**

```kotlin
// feature/notifications/src/main/java/com/mixlauncher/feature/notifications/listener/NotificationParser.kt

package com.mixlauncher.feature.notifications.listener

import android.app.Notification
import android.service.notification.StatusBarNotification
import com.mixlauncher.feature.notifications.model.LauncherNotification
import timber.log.Timber

object NotificationParser {
    
    fun parse(sbn: StatusBarNotification): LauncherNotification {
        val notification = sbn.notification
        
        return LauncherNotification(
            key = sbn.key,
            packageName = sbn.packageName,
            title = getTitle(notification),
            text = getText(notification),
            subText = getSubText(notification),
            timestamp = sbn.postTime,
            isOngoing = notification.flags and Notification.FLAG_ONGOING_EVENT != 0,
            isClearable = sbn.isClearable,
            category = notification.category,
            priority = notification.priority,
            actions = parseActions(notification)
        )
    }
    
    private fun getTitle(notification: Notification): String? {
        return notification.extras.getCharSequence(Notification.EXTRA_TITLE)?.toString()
    }
    
    private fun getText(notification: Notification): String? {
        return notification.extras.getCharSequence(Notification.EXTRA_TEXT)?.toString()
    }
    
    private fun getSubText(notification: Notification): String? {
        return notification.extras.getCharSequence(Notification.EXTRA_SUB_TEXT)?.toString()
    }
    
    private fun parseActions(notification: Notification): List<String> {
        return notification.actions?.map { it.title.toString() } ?: emptyList()
    }
}
```

### **3. ThemeEngine.kt**

```kotlin
// feature/themes/src/main/java/com/mixlauncher/feature/themes/engine/ThemeEngine.kt

package com.mixlauncher.feature.themes.engine

import android.content.Context
import android.content.Intent
import androidx.core.content.edit
import com.mixlauncher.feature.themes.model.LauncherTheme
import com.mixlauncher.feature.themes.iconpacks.IconPackParser
import com.mixlauncher.feature.themes.iconpacks.IconPack
import dagger.hilt.android.qualifiers.ApplicationContext
import timber.log.Timber
import javax.inject.Inject
import javax.inject.Singleton

@Singleton
class ThemeEngine @Inject constructor(
    @ApplicationContext private val context: Context
) {
    
    companion object {
        const val ACTION_THEME_CHANGED = "com.mixlauncher.THEME_CHANGED"
        private const val PREFS_NAME = "theme_preferences"
        private const val KEY_CURRENT_THEME = "current_theme"
        private const val KEY_PRIMARY_COLOR = "primary_color"
        private const val KEY_ACCENT_COLOR = "accent_color"
        private const val KEY_ICON_PACK = "icon_pack"
    }
    
    private val preferences = context.getSharedPreferences(PREFS_NAME, Context.MODE_PRIVATE)
    
    fun getCurrentTheme(): LauncherTheme {
        val themeId = preferences.getString(KEY_CURRENT_THEME, "dark") ?: "dark"
        return loadTheme(themeId)
    }
    
    fun applyTheme(theme: LauncherTheme) {
        Timber.d("Applying theme: ${theme.id}")
        
        preferences.edit {
            putString(KEY_CURRENT_THEME, theme.id)
            putString(KEY_PRIMARY_COLOR, theme.primaryColor)
            putString(KEY_ACCENT_COLOR, theme.accentColor)
        }
        
        // Broadcast theme change
        context.sendBroadcast(Intent(ACTION_THEME_CHANGED))
        
        Timber.d("Theme applied successfully")
    }
    
    private fun loadTheme(themeId: String): LauncherTheme {
        return when (themeId) {
            "light" -> LauncherTheme.Light
            "dark" -> LauncherTheme.Dark
            "material_you" -> LauncherTheme.MaterialYou
            else -> LauncherTheme.Dark
        }
    }
    
    fun loadIconPack(packageName: String): IconPack? {
        Timber.d("Loading icon pack: $packageName")
        
        return try {
            val iconPack = IconPackParser.parse(context, packageName)
            
            if (iconPack != null) {
                preferences.edit {
                    putString(KEY_ICON_PACK, packageName)
                }
                Timber.d("Icon pack loaded successfully")
            }
            
            iconPack
        } catch (e: Exception) {
            Timber.e(e, "Failed to load icon pack")
            null
        }
    }
    
    fun getCurrentIconPack(): String? {
        return preferences.getString(KEY_ICON_PACK, null)
    }
    
    fun clearIconPack() {
        preferences.edit {
            remove(KEY_ICON_PACK)
        }
    }
}
```

### **4. GestureDetector.kt**

```kotlin
// feature/gestures/src/main/java/com/mixlauncher/feature/gestures/GestureDetector.kt

package com.mixlauncher.feature.gestures

import android.view.MotionEvent
import androidx.compose.foundation.gestures.detectDragGestures
import androidx.compose.foundation.gestures.detectTapGestures
import androidx.compose.ui.geometry.Offset
import androidx.compose.ui.input.pointer.PointerInputScope
import com.mixlauncher.feature.gestures.actions.GestureAction
import com.mixlauncher.feature.gestures.types.*
import kotlinx.coroutines.coroutineScope
import timber.log.Timber
import kotlin.math.abs

class LauncherGestureDetector(
    private val onGestureDetected: (GestureAction) -> Unit
) {
    
    private var swipeStartY = 0f
    private var swipeStartX = 0f
    
    suspend fun detectGestures(pointerInputScope: PointerInputScope) {
        pointerInputScope.apply {
            coroutineScope {
                // Detect tap gestures
                detectTapGestures(
                    onDoubleTap = { offset ->
                        Timber.d("Double tap detected at $offset")
                        onGestureDetected(GestureAction.DoubleTap)
                    },
                    onLongPress = { offset ->
                        Timber.d("Long press detected at $offset")
                        onGestureDetected(GestureAction.LongPress)
                    },
                    onTap = { offset ->
                        Timber.d("Tap detected at $offset")
                        onGestureDetected(GestureAction.Tap)
                    }
                )
                
                // Detect drag/swipe gestures
                detectDragGestures(
                    onDragStart = { offset ->
                        swipeStartY = offset.y
                        swipeStartX = offset.x
                    },
                    onDragEnd = {
                        val gesture = detectSwipeDirection()
                        if (gesture != null) {
                            Timber.d("Swipe detected: $gesture")
                            onGestureDetected(gesture)
                        }
                    },
                    onDrag = { change, dragAmount ->
                        // Track drag
                    }
                )
            }
        }
    }
    
    private fun detectSwipeDirection(): GestureAction? {
        val deltaY = swipeStartY - 0f // Current Y position
        val deltaX = swipeStartX - 0f // Current X position
        
        val threshold = 100f
        
        return when {
            abs(deltaY) > abs(deltaX) && abs(deltaY) > threshold -> {
                if (deltaY > 0) GestureAction.SwipeUp else GestureAction.SwipeDown
            }
            abs(deltaX) > abs(deltaY) && abs(deltaX) > threshold -> {
                if (deltaX > 0) GestureAction.SwipeLeft else GestureAction.SwipeRight
            }
            else -> null
        }
    }
}
```

### **5. PluginManager.kt**

```kotlin
// feature/plugins/src/main/java/com/mixlauncher/feature/plugins/PluginManager.kt

package com.mixlauncher.feature.plugins

import android.content.Context
import com.mixlauncher.core.common.Result
import com.mixlauncher.core.pluginapi.LauncherPlugin
import com.mixlauncher.feature.plugins.loader.PluginLoader
import dagger.hilt.android.qualifiers.ApplicationContext
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.withContext
import timber.log.Timber
import java.io.File
import javax.inject.Inject
import javax.inject.Singleton

@Singleton
class PluginManager @Inject constructor(
    @ApplicationContext private val context: Context,
    private val pluginLoader: PluginLoader
) {
    
    private val plugins = mutableMapOf<String, LauncherPlugin>()
    private val pluginDir = File(context.filesDir, "plugins")
    
    init {
        pluginDir.mkdirs()
    }
    
    suspend fun loadPlugin(pluginPath: String): Result<LauncherPlugin> {
        return withContext(Dispatchers.IO) {
            try {
                Timber.d("Loading plugin from: $pluginPath")
                
                val plugin = pluginLoader.load(pluginPath)
                
                // Validate plugin
                if (!pluginLoader.validate(plugin)) {
                    return@withContext Result.Error(
                        Exception("Plugin validation failed")
                    )
                }
                
                // Initialize plugin
                plugin.onLoad(context)
                
                // Store plugin
                plugins[plugin.id] = plugin
                
                Timber.d("Plugin loaded successfully: ${plugin.name}")
                Result.Success(plugin)
                
            } catch (e: Exception) {
                Timber.e(e, "Failed to load plugin")
                Result.Error(e)
            }
        }
    }
    
    fun getPlugin(pluginId: String): LauncherPlugin? {
        return plugins[pluginId]
    }
    
    fun getAllPlugins(): List<LauncherPlugin> {
        return plugins.values.toList()
    }
    
    suspend fun unloadPlugin(pluginId: String): Result<Unit> {
        return withContext(Dispatchers.IO) {
            try {
                val plugin = plugins[pluginId]
                    ?: return@withContext Result.Error(
                        Exception("Plugin not found: $pluginId")
                    )
                
                // Call plugin cleanup
                plugin.onUnload()
                
                // Remove from map
                plugins.remove(pluginId)
                
                Timber.d("Plugin unloaded: $pluginId")
                Result.Success(Unit)
                
            } catch (e: Exception) {
                Timber.e(e, "Failed to unload plugin")
                Result.Error(e)
            }
        }
    }
    
    suspend fun reloadPlugin(pluginId: String): Result<LauncherPlugin> {
        val plugin = plugins[pluginId]
            ?: return Result.Error(Exception("Plugin not found"))
        
        // Unload
        unloadPlugin(pluginId)
        
        // Reload
        return loadPlugin(plugin.path)
    }
    
    fun getInstalledPlugins(): List<File> {
        return pluginDir.listFiles()?.filter { it.extension == "apk" } ?: emptyList()
    }
}
```

### **6. LauncherPlugin.kt (Plugin API)**

```kotlin
// core/plugin-api/src/main/java/com/mixlauncher/core/pluginapi/LauncherPlugin.kt

package com.mixlauncher.core.pluginapi

import android.content.Context

abstract class LauncherPlugin {
    
    abstract val id: String
    abstract val name: String
    abstract val version: String
    abstract val description: String
    abstract val author: String
    abstract val path: String
    
    /**
     * Called when plugin is loaded
     */
    abstract fun onLoad(context: Context)
    
    /**
     * Called when plugin is unloaded
     */
    abstract fun onUnload()
    
    /**
     * Called when launcher starts
     */
    open fun onLauncherStart() {}
    
    /**
     * Called when launcher stops
     */
    open fun onLauncherStop() {}
    
    /**
     * Called when app is launched
     */
    open fun onAppLaunched(packageName: String) {}
    
    /**
     * Get plugin metadata
     */
    fun getMetadata(): PluginMetadata {
        return PluginMetadata(
            id = id,
            name = name,
            version = version,
            description = description,
            author = author
        )
    }
}

data class PluginMetadata(
    val id: String,
    val name: String,
    val version: String,
    val description: String,
    val author: String
)
```

### **7. TaskManager.kt (Recents)**

```kotlin
// feature/recents/src/main/java/com/mixlauncher/feature/recents/manager/TaskManager.kt

package com.mixlauncher.feature.recents.manager

import android.app.ActivityManager
import android.content.Context
import android.content.pm.PackageManager
import com.mixlauncher.feature.recents.model.RecentTask
import dagger.hilt.android.qualifiers.ApplicationContext
import timber.log.Timber
import javax.inject.Inject
import javax.inject.Singleton

@Singleton
class TaskManager @Inject constructor(
    @ApplicationContext private val context: Context
) {
    
    companion object {
        private const val MAX_RECENT_TASKS = 20
    }
    
    private val activityManager = context.getSystemService(Context.ACTIVITY_SERVICE) as ActivityManager
    private val packageManager = context.packageManager
    
    fun getRecentTasks(): List<RecentTask> {
        return try {
            val tasks = activityManager.getRecentTasks(
                MAX_RECENT_TASKS,
                ActivityManager.RECENT_WITH_EXCLUDED
            )
            
            tasks.mapNotNull { taskInfo ->
                try {
                    val packageName = taskInfo.baseActivity?.packageName ?: return@mapNotNull null
                    
                    // Skip launcher itself
                    if (packageName == context.packageName) return@mapNotNull null
                    
                    val appInfo = packageManager.getApplicationInfo(packageName, 0)
                    val appName = packageManager.getApplicationLabel(appInfo).toString()
                    val icon = packageManager.getApplicationIcon(packageName)
                    
                    RecentTask(
                        id = taskInfo.id,
                        packageName = packageName,
                        appName = appName,
                        icon = icon,
                        timestamp = System.currentTimeMillis()
                    )
                } catch (e: PackageManager.NameNotFoundException) {
                    Timber.e(e, "Package not found")
                    null
                }
            }
        } catch (e: SecurityException) {
            Timber.e(e, "No permission to get recent tasks")
            emptyList()
        }
    }
    
    fun killTask(taskId: Int) {
        try {
            activityManager.removeTask(taskId)
            Timber.d("Task killed: $taskId")
        } catch (e: Exception) {
            Timber.e(e, "Failed to kill task")
        }
    }
    
    fun killAllTasks() {
        getRecentTasks().forEach { task ->
            killTask(task.id)
        }
    }
    
    fun switchToTask(taskId: Int) {
        try {
            activityManager.moveTaskToFront(taskId, 0)
            Timber.d("Switched to task: $taskId")
        } catch (e: Exception) {
            Timber.e(e, "Failed to switch to task")
        }
    }
}
```

---

## 🧪 **Testing Checklist**

### **Notification System**
- [ ] Notifications appear in shade
- [ ] Actions work (reply, dismiss)
- [ ] Quick settings toggle works
- [ ] Grouping works correctly
- [ ] Persistent notifications handled
- [ ] Service survives restart

### **Gestures**
- [ ] Swipe up opens app drawer
- [ ] Swipe down shows notifications
- [ ] Double tap locks screen
- [ ] Long press shows options
- [ ] Gestures customizable

### **Themes**
- [ ] Theme changes apply immediately
- [ ] Icon pack changes correctly
- [ ] Material You works on Android 12+
- [ ] Custom colors save/load
- [ ] Theme persists after restart

### **Plugins**
- [ ] Plugins load correctly
- [ ] Plugins can be enabled/disabled
- [ ] Plugins don't crash launcher
- [ ] Plugin API works as documented
- [ ] Malicious plugins blocked

### **Recents**
- [ ] Recent apps display correctly
- [ ] Task switching works
- [ ] Kill app works
- [ ] Locked apps persist
- [ ] Updates in real-time

---

## 📋 **Permissions Required**

### **AndroidManifest.xml Updates**

```xml
<!-- Notification Listener -->
<service
    android:name=".feature.notifications.listener.LauncherNotificationListener"
    android:permission="android.permission.BIND_NOTIFICATION_LISTENER_SERVICE"
    android:exported="true">
    <intent-filter>
        <action android:name="android.service.notification.NotificationListenerService" />
    </intent-filter>
</service>

<!-- Permissions -->
<uses-permission android:name="android.permission.GET_TASKS" />
<uses-permission android:name="android.permission.REORDER_TASKS" />
<uses-permission android:name="android.permission.BIND_NOTIFICATION_LISTENER_SERVICE" />
```

---

## 🐛 **Common Issues & Solutions**

### **Issue: Notification listener not working**
**Solution:**
```kotlin
// Request notification listener permission
val intent = Intent("android.settings.ACTION_NOTIFICATION_LISTENER_SETTINGS")
startActivity(intent)

// Check if permission granted
val cn = ComponentName(context, LauncherNotificationListener::class.java)
val flat = Settings.Secure.getString(
    context.contentResolver,
    "enabled_notification_listeners"
)
val isEnabled = flat?.contains(cn.flattenToString()) == true
```

### **Issue: Gestures conflicting with system**
**Solution:**
- Use proper touch event handling
- Respect system gesture areas
- Add gesture sensitivity settings
- Allow users to customize gestures

### **Issue: Icon pack not applying**
**Solution:**
```kotlin
// Verify icon pack structure
val iconPackResources = packageManager.getResourcesForApplication(iconPackPackage)

// Check drawable resources
val drawableId = iconPackResources.getIdentifier(
    "icon_name",
    "drawable",
    iconPackPackage
)

if (drawableId != 0) {
    val icon = iconPackResources.getDrawable(drawableId)
}
```

### **Issue: Plugin loading fails**
**Solution:**
- Validate plugin signature
- Check API version compatibility
- Sandbox plugin execution
- Implement proper error handling
- Log detailed error messages

---

## 🎯 **Phase 3 Deliverables**

Upon completion:

- ✅ Custom notification system with quick settings
- ✅ Advanced gesture system (6+ gestures)
- ✅ Complete theme engine with 3+ presets
- ✅ Icon pack support
- ✅ Working plugin system with API
- ✅ Recent apps task switcher
- ✅ Material You support (Android 12+)
- ✅ All features tested and documented

---

## 📊 **Performance Targets**

| Feature | Target | Metric |
|---------|--------|--------|
| Notification shade open | < 200ms | Animation complete |
| Theme switch | < 500ms | Full UI update |
| Gesture detection | < 16ms | Touch to action |
| Plugin load time | < 2s | First execution |
| Recent apps load | < 300ms | Full list display |

---

## 🔧 **Build Configuration Updates**

### **feature/notifications/build.gradle.kts**

```kotlin
dependencies {
    implementation(project(":core:common"))
    implementation(project(":core:domain"))
    implementation(project(":core:ui"))
    
    // Compose
    implementation(platform(Deps.composeBom))
    implementation(Deps.composeUi)
    implementation(Deps.composeMaterial3)
    
    // Hilt
    implementation(Deps.hiltAndroid)
    kapt(Deps.hiltCompiler)
    
    // Coroutines
    implementation(Deps.coroutinesAndroid)
}
```

### **feature/plugins/build.gradle.kts**

```kotlin
dependencies {
    implementation(project(":core:common"))
    implementation(project(":core:plugin-api"))
    
    // For plugin loading
    implementation("androidx.core:core-ktx:1.12.0")
    
    // Security for plugin validation
    implementation("androidx.security:security-crypto:1.1.0-alpha06")
}
```

---

## 📚 **Documentation to Create**

### **For Users**
- [ ] How to enable notification access
- [ ] Gesture customization guide
- [ ] Theme selection guide
- [ ] Icon pack installation
- [ ] Plugin installation guide

### **For Developers**
- [ ] Plugin development guide
- [ ] Theme creation guide
- [ ] Icon pack format specification
- [ ] API reference documentation

---

## 🔐 **Security Considerations**

### **Plugin Security**
```kotlin
// Validate plugin signature
fun validatePlugin(plugin: File): Boolean {
    // Check signature
    val signatures = getPackageSignatures(plugin)
    
    // Verify against known signatures
    return signatures.any { it in trustedSignatures }
}

// Sandbox plugin execution
fun executePlugin(plugin: LauncherPlugin) {
    // Run in isolated classloader
    val classLoader = PluginClassLoader(plugin.path, parent = null)
    
    // Limit permissions
    // Monitor resource usage
    // Set execution timeout
}
```

### **Notification Access**
- Only read notification content
- Don't log sensitive data
- Respect user privacy
- Clear data on uninstall

---

## 🎨 **UI/UX Guidelines**

### **Notification Shade**
- Material Design 3 components
- Smooth pull-down animation
- Clear notification grouping
- Easy dismiss gestures
- Quick settings accessible

### **Theme Picker**
- Visual theme previews
- Live preview mode
- Easy color customization
- Save custom themes
- Import/export themes

### **Recent Apps**
- Card-based layout
- Smooth transitions
- Clear app screenshots
- Easy swipe to dismiss
- Lock/unlock functionality

---

## 🚀 **Optimization Tips**

### **Notification System**
```kotlin
// Batch notification updates
private val updateQueue = mutableListOf<Notification>()

fun queueNotificationUpdate(notification: Notification) {
    updateQueue.add(notification)
    
    // Process batch every 100ms
    handler.postDelayed({
        processBatch()
    }, 100)
}
```

### **Theme Application**
```kotlin
// Cache theme resources
private val themeCache = LruCache<String, Theme>(10)

fun applyTheme(themeId: String) {
    val theme = themeCache[themeId] ?: loadTheme(themeId)
    
    // Apply in background
    scope.launch(Dispatchers.IO) {
        applyThemeResources(theme)
        withContext(Dispatchers.Main) {
            refreshUI()
        }
    }
}
```

---

## ➡️ **Next Steps**

After Phase 3:
1. Merge `phase-3-advanced-features` to `develop` branch
2. Tag as `v0.3.0-rc1` (Release Candidate 1)
3. Internal testing and bug fixes
4. Create `phase-4-polish` branch
5. Begin Phase 4 (Performance, Testing, Documentation)

---

## 📞 **Need Help?**

- Review [ARCHITECTURE.md](../docs/ARCHITECTURE.md)
- Check [API.md](../docs/API.md) for plugin API
- Open issue with `phase-3` label
- Join Discord for real-time support
- Email: dev@mixlauncher.com

---

**Last Updated:** January 2026  
**Phase Status:** 🔵 Not Started  
**Target Completion:** 8 weeks  
**Next Phase:** Phase 4 - Polish & Optimization

---

## 🎉 **Congratulations!**

After completing Phase 3, Mix Launcher will have:
- ✨ Professional notification system
- 🎨 Complete theming capabilities
- 👆 Advanced gesture controls
- 🔌 Extensible plugin architecture
- 📱 Modern recent apps interface

**Almost ready for release!** Just one more phase to go! 🚀
```
