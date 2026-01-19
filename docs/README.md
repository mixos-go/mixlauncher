# **MANDATORY** Do not use emojis/emoticons in code, files, or folders. For implementation, use ICONS (CUSTOM/DOWNLOAD)

## **ARCHITECTURE**

### **Core components that need to be there:**

```
 MIX Launcher
│
├── HOME SCREEN SYSTEM
│   ├── App Grid/Drawer
│   ├── Dock
│   ├── Widget Engine
│   ├── Wallpaper Manager
│   └── Search Bar
│
├── NOTIFICATION SYSTEM
│   ├── Notification Shade (custom)
│   ├── Quick Settings
│   └── Notification Manager
│
├── RECENT APPS / MULTITASKING
│   ├── Task Switcher
│   └── App Management
│
├── SETTINGS & CUSTOMIZATION
│   ├── Theme Engine
│   ├── Gesture Controls
│   ├── Icon Packs Support
│   └── Layout Options
│
├── DEV TOOLS (Unique Selling Point)
│   ├── Code Server Widget/Panel
│   ├── Terminal Widget/Panel
│   ├── File Manager
│   ├── Git Client
│   └── Package Manager
│
└── SYSTEM SERVICES
    ├── Background Service Manager
    ├── Watchdog Service
    ├── Auto-start Manager
    └── Battery Optimization Handler
```

---

## **DETAILED DESIGN DOCUMENT**

### **1. HOME SCREEN SYSTEM**

```kotlin
// Main Launcher Activity
class LauncherActivity : AppCompatActivity() {
    
    private lateinit var homeScreenManager: HomeScreenManager
    private lateinit var appDrawerManager: AppDrawerManager
    private lateinit var widgetHost: AppWidgetHost
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        // Set sebagai default launcher
        setContentView(R.layout.activity_launcher)
        
        // Initialize components
        initializeHomeScreen()
        initializeAppDrawer()
        initializeWidgets()
        initializeDock()
        initializeGestures()
    }
    
    override fun onBackPressed() {
        // Handle back button -> show home
        showHomeScreen()
    }
}
```

**Features:**
- ✅ Multiple home screen pages (swipeable)
- ✅ Customizable grid size (4x5, 5x6, dll)
- ✅ App shortcuts & folders
- ✅ Gesture support (swipe up = drawer, dll)
- ✅ Smart search (apps, contacts, web)

---

### **2. WIDGET ENGINE (ini yang penting untuk dev tools)**

```kotlin
class CustomWidgetHost : AppWidgetHost(context, HOST_ID) {
    
    // Support standard Android widgets
    fun addStandardWidget(widgetId: Int)
    
    // Custom widgets untuk dev tools
    fun addCodeServerWidget()
    fun addTerminalWidget()
    fun addFileManagerWidget()
}

// Custom Widget Base
abstract class DevToolWidget : FrameLayout {
    abstract fun initialize()
    abstract fun onResume()
    abstract fun onPause()
    abstract fun saveState()
}

// Terminal Widget Implementation
class TerminalWidget : DevToolWidget() {
    
    private lateinit var terminalView: TerminalView
    private lateinit var terminalSession: TerminalSession
    
    override fun initialize() {
        // Embed terminal emulator view
        terminalView = TerminalView(context)
        addView(terminalView)
        
        // Create shell session
        terminalSession = createShellSession()
        terminalView.attachSession(terminalSession)
    }
    
    // Support resize, fullscreen, minimize
    fun expandToFullscreen()
    fun minimizeToWidget()
}

// Code Server Widget
class CodeServerWidget : DevToolWidget() {
    
    private lateinit var webView: WebView
    private var serverUrl = "http://localhost:8080"
    
    override fun initialize() {
        // WebView untuk code-server interface
        webView = WebView(context).apply {
            settings.javaScriptEnabled = true
            settings.domStorageEnabled = true
            settings.allowFileAccess = true
        }
        
        addView(webView)
        webView.loadUrl(serverUrl)
    }
}
```

**Widget Modes:**

- **Widget Mode** - Small, di home screen
- **Panel Mode** - Slide dari side, overlay
- **Fullscreen Mode** - Full takeover screen
- **Floating Mode** - Draggable floating window

---

### **3. APP DRAWER & MANAGEMENT**

```kotlin
class AppDrawerManager {
    
    // Load all installed apps
    fun loadInstalledApps(): List<AppInfo>
    
    // Categorize apps
    fun categorizeApps(): Map<String, List<AppInfo>>
    
    // Search & filter
    fun searchApps(query: String): List<AppInfo>
    
    // Launch app
    fun launchApp(packageName: String)
    
    // App info & management
    fun showAppInfo(packageName: String)
    fun uninstallApp(packageName: String)
}

// Support fitur modern
class SmartAppDrawer {
    // Frequently used apps di atas
    // Alphabetical sorting
    // Category tabs (Social, Tools, Games, Dev)
    // Hidden apps
    // App suggestions based on time/location
}
```

---

### **4. NOTIFICATION SYSTEM (Advanced)**

```kotlin
// Custom Notification Listener
class LauncherNotificationListener : NotificationListenerService() {
    
    override fun onNotificationPosted(sbn: StatusBarNotification) {
        // Intercept notifications
        // Show di custom notification shade
        notificationManager.addNotification(sbn)
    }
    
    override fun onNotificationRemoved(sbn: StatusBarNotification) {
        notificationManager.removeNotification(sbn)
    }
}

// Custom Notification Shade
class NotificationShadeView : FrameLayout {
    
    // Pull down dari top
    fun showNotifications()
    
    // Quick settings tiles
    fun showQuickSettings()
    
    // Notification actions
    fun handleNotificationAction(action: NotificationAction)
}
```

---

### **5. RECENT APPS / MULTITASKING**

```kotlin
class RecentAppsManager {
    
    private val activityManager: ActivityManager
    
    // Get recent tasks
    fun getRecentTasks(): List<ActivityManager.RecentTaskInfo> {
        return activityManager.getRecentTasks(
            MAX_TASKS,
            ActivityManager.RECENT_WITH_EXCLUDED
        )
    }
    
    // Show recents UI (cards, list, grid)
    fun showRecentApps()
    
    // Kill app
    fun killApp(packageName: String)
    
    // Lock app (keep in memory)
    fun lockApp(packageName: String)
}
```

---

### **6. DEV TOOLS BACKEND**

```kotlin
// Background Services untuk Dev Tools
class DevToolsService : Service() {
    
    private lateinit var codeServerManager: CodeServerManager
    private lateinit var terminalManager: TerminalManager
    
    override fun onCreate() {
        super.onCreate()
        
        // Start as foreground service
        startForeground(NOTIF_ID, createNotification())
        
        // Initialize dev tools
        codeServerManager = CodeServerManager(this)
        terminalManager = TerminalManager(this)
        
        // Start servers
        codeServerManager.startServer()
    }
}

class CodeServerManager(private val context: Context) {
    
    private var serverProcess: Process? = null
    private val serverPort = 8080
    
    fun startServer() {
        val binaryPath = extractBinary("code-server")
        
        serverProcess = ProcessBuilder()
            .command(
                binaryPath,
                "--bind-addr", "127.0.0.1:$serverPort",
                "--auth", "none",
                "--user-data-dir", context.filesDir.absolutePath
            )
            .redirectErrorStream(true)
            .start()
        
        // Monitor process
        monitorServer()
    }
    
    fun stopServer() {
        serverProcess?.destroy()
    }
    
    private fun monitorServer() {
        // Restart if crashed
        // Log output
        // Notify launcher if server ready
    }
}
```

---

### **7. SETTINGS & CUSTOMIZATION**

```kotlin
class LauncherSettings {
    
    // Theme
    var theme: Theme = Theme.DARK
    var accentColor: Int = Color.BLUE
    var iconPack: String? = null
    
    // Layout
    var gridRows: Int = 5
    var gridColumns: Int = 4
    var iconSize: Float = 1.0f
    
    // Gestures
    var swipeUpAction: GestureAction = GestureAction.OPEN_DRAWER
    var swipeDownAction: GestureAction = GestureAction.NOTIFICATIONS
    var doubleTapAction: GestureAction = GestureAction.LOCK_SCREEN
    
    // Dev Tools
    var enableTerminal: Boolean = true
    var enableCodeServer: Boolean = true
    var codeServerPort: Int = 8080
    var defaultShell: String = "/system/bin/sh"
}
```

---

### **8. MANIFEST REQUIREMENTS**

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    
    <!-- Permissions -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
    <uses-permission android:name="android.permission.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS" />
    <uses-permission android:name="android.permission.BIND_NOTIFICATION_LISTENER_SERVICE" />
    <uses-permission android:name="android.permission.GET_TASKS" />
    <uses-permission android:name="android.permission.REORDER_TASKS" />
    
    <application>
        
        <!-- Main Launcher Activity -->
        <activity
            android:name=".LauncherActivity"
            android:launchMode="singleTask"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.HOME" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        
        <!-- Services -->
        <service
            android:name=".services.DevToolsService"
            android:foregroundServiceType="specialUse"
            android:exported="false" />
            
        <service
            android:name=".services.LauncherNotificationListener"
            android:permission="android.permission.BIND_NOTIFICATION_LISTENER_SERVICE"
            android:exported="true">
            <intent-filter>
                <action android:name="android.service.notification.NotificationListenerService" />
            </intent-filter>
        </service>
        
        <!-- Widget Host -->
        <receiver android:name=".widgets.WidgetReceiver">
            <intent-filter>
                <action android:name="android.appwidget.action.APPWIDGET_UPDATE" />
            </intent-filter>
        </receiver>
        
    </application>
</manifest>
```

---

## **UNIQUE SELLING POINTS**

What makes this launcher different?:

1. **Dev-First Launcher** - Built for developers
2. **Integrated Terminal** - No need separate terminal app
3. **Built-in Code Server** - Code on your phone
4. **File Manager** - Direct file access
5. **Git Integration** - Commit, push, pull dari launcher
6. **Package Manager** - Install dev tools (npm, pip, dll)
7. **Productivity Focus** - Quick access to dev tools

---

## **TECH STACK**

```
- Language: Kotlin
- UI: Jetpack Compose / Material 3
- Architecture: MVVM + Clean Architecture
- DI: Hilt/Koin
- Storage: Room + DataStore
- Terminal: Termux libraries
- WebView: ChromeWebView
- Gestures: Custom gesture detector
```

---

##  **DEVELOPMENT PHASES**

**Complete Structure By Phase:**

- Phase 1: `docs/PHASE-1-GUIDE.md`
- Phase 2: `docs/PHASE-2-GUIDE.md`
- Phase 3: `docs/PHASE-3-GUIDE.md`
- Phase 4: `docs/PHASE-4-GUIDE.md`

**Mandatory target of the desired final structure:** read on `docs/TARGET-STRUCTURE.md`

---