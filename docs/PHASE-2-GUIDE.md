Oke lanjut! 🔥 Sekarang **PHASE-2-GUIDE.md**:

---

```markdown
# Phase 2: Dev Tools Integration Guide

**Branch:** `phase-2-dev-tools`  
**Duration:** 8 weeks  
**Status:** 🔵 Not Started  
**Prerequisites:** Phase 1 completed

---

## 📋 **Overview**

Phase 2 adds the core development tools that make Mix Launcher unique: an integrated terminal emulator, VS Code via code-server, and a powerful file manager. All tools are accessible as widgets or full-screen interfaces.

---

## 🎯 **Milestone Objectives**

- ✅ Terminal widget & fullscreen mode
- ✅ Code-server integration with WebView
- ✅ File manager with preview support
- ✅ Background services that survive system kills
- ✅ Widget configuration & customization

---

## 🏗️ **Module Structure**

### **Feature Modules**

#### **feature/terminal**
```
feature/terminal/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/feature/terminal/
    ├── TerminalScreen.kt
    ├── TerminalViewModel.kt
    ├── TerminalState.kt
    ├── TerminalEvent.kt
    │
    ├── 📂 widget/
    │   ├── TerminalWidget.kt
    │   ├── TerminalWidgetProvider.kt
    │   └── TerminalWidgetConfig.kt
    │
    ├── 📂 emulator/
    │   ├── TerminalView.kt
    │   ├── TerminalSession.kt
    │   ├── TerminalEmulator.kt
    │   ├── ShellManager.kt
    │   └── TerminalRenderer.kt
    │
    ├── 📂 ui/
    │   ├── TerminalScreen.kt
    │   ├── TerminalKeyboard.kt
    │   ├── TerminalSettings.kt
    │   └── SessionTabs.kt
    │
    └── 📂 service/
        ├── TerminalService.kt
        └── SessionManager.kt
```

#### **feature/codeserver**
```
feature/codeserver/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/feature/codeserver/
    ├── CodeServerScreen.kt
    ├── CodeServerViewModel.kt
    ├── CodeServerState.kt
    │
    ├── 📂 widget/
    │   ├── CodeServerWidget.kt
    │   └── CodeServerWidgetProvider.kt
    │
    ├── 📂 server/
    │   ├── CodeServerManager.kt
    │   ├── ServerProcess.kt
    │   ├── ServerConfig.kt
    │   ├── BinaryExtractor.kt
    │   └── ServerMonitor.kt
    │
    ├── 📂 webview/
    │   ├── CodeServerWebView.kt
    │   ├── CodeWebViewClient.kt
    │   └── WebViewSettings.kt
    │
    └── 📂 service/
        ├── CodeServerService.kt
        └── CodeServerWatchdog.kt
```

#### **feature/filemanager**
```
feature/filemanager/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/feature/filemanager/
    ├── FileManagerScreen.kt
    ├── FileManagerViewModel.kt
    ├── FileManagerState.kt
    │
    ├── 📂 browser/
    │   ├── FileBrowser.kt
    │   ├── FileList.kt
    │   ├── FileItem.kt
    │   └── BreadcrumbNav.kt
    │
    ├── 📂 operations/
    │   ├── FileOperations.kt
    │   ├── CopyPaste.kt
    │   └── SearchFiles.kt
    │
    └── 📂 preview/
        ├── TextPreview.kt
        ├── ImagePreview.kt
        ├── CodePreview.kt
        └── PdfPreview.kt
```

#### **feature/devtools**
```
feature/devtools/
└── src/main/java/com/mixlauncher/feature/devtools/
    ├── DevToolsPanel.kt
    ├── DevToolsViewModel.kt
    │
    ├── 📂 quickaccess/
    │   ├── QuickAccessPanel.kt
    │   └── QuickLaunchButtons.kt
    │
    └── 📂 service/
        ├── DevToolsService.kt
        └── ServiceManager.kt
```

### **Core Modules**

#### **core/service**
```
core/service/
└── src/main/java/com/mixlauncher/core/service/
    ├── BaseForegroundService.kt
    ├── ServiceNotification.kt
    ├── WatchdogService.kt
    └── ServiceManager.kt
```

#### **core/native**
```
core/native/
└── src/main/
    ├── cpp/
    │   ├── native-lib.cpp
    │   └── CMakeLists.txt
    │
    └── java/com/mixlauncher/core/native/
        └── NativeLibrary.kt
```

### **Binaries**
```
binaries/
├── 📂 code-server/
│   ├── arm64-v8a/
│   │   └── code-server
│   ├── armeabi-v7a/
│   │   └── code-server
│   └── README.md
│
└── 📂 node/
    ├── arm64-v8a/
    └── armeabi-v7a/
```

---

## 💻 **Key Implementation Files**

### **1. TerminalService.kt**

```kotlin
// feature/terminal/src/main/java/com/mixlauncher/feature/terminal/service/TerminalService.kt

package com.mixlauncher.feature.terminal.service

import android.app.Service
import android.content.Intent
import android.os.Binder
import android.os.IBinder
import com.mixlauncher.core.service.BaseForegroundService
import com.mixlauncher.feature.terminal.emulator.ShellManager
import com.mixlauncher.feature.terminal.emulator.TerminalSession
import dagger.hilt.android.AndroidEntryPoint
import timber.log.Timber
import javax.inject.Inject

@AndroidEntryPoint
class TerminalService : BaseForegroundService() {
    
    @Inject
    lateinit var shellManager: ShellManager
    
    private val sessions = mutableMapOf<String, TerminalSession>()
    private val binder = TerminalBinder()
    
    override fun onCreate() {
        super.onCreate()
        Timber.d("TerminalService created")
    }
    
    override fun onBind(intent: Intent): IBinder {
        return binder
    }
    
    fun createSession(sessionId: String): TerminalSession {
        Timber.d("Creating terminal session: $sessionId")
        
        val session = shellManager.createShellSession()
        sessions[sessionId] = session
        
        return session
    }
    
    fun getSession(sessionId: String): TerminalSession? {
        return sessions[sessionId]
    }
    
    fun destroySession(sessionId: String) {
        Timber.d("Destroying terminal session: $sessionId")
        
        sessions[sessionId]?.destroy()
        sessions.remove(sessionId)
    }
    
    fun getAllSessions(): List<TerminalSession> {
        return sessions.values.toList()
    }
    
    override fun onDestroy() {
        Timber.d("TerminalService destroyed")
        
        // Clean up all sessions
        sessions.values.forEach { it.destroy() }
        sessions.clear()
        
        super.onDestroy()
    }
    
    inner class TerminalBinder : Binder() {
        fun getService(): TerminalService = this@TerminalService
    }
}
```

### **2. ShellManager.kt**

```kotlin
// feature/terminal/src/main/java/com/mixlauncher/feature/terminal/emulator/ShellManager.kt

package com.mixlauncher.feature.terminal.emulator

import android.content.Context
import dagger.hilt.android.qualifiers.ApplicationContext
import timber.log.Timber
import java.io.File
import javax.inject.Inject
import javax.inject.Singleton

@Singleton
class ShellManager @Inject constructor(
    @ApplicationContext private val context: Context
) {
    
    private val homeDir: File = context.filesDir
    private val shellPath: String = "/system/bin/sh"
    
    fun createShellSession(): TerminalSession {
        Timber.d("Creating new shell session")
        
        val env = buildEnvironment()
        val cwd = homeDir.absolutePath
        
        return TerminalSession(
            shellPath = shellPath,
            cwd = cwd,
            env = env
        )
    }
    
    private fun buildEnvironment(): Array<String> {
        return arrayOf(
            "TERM=xterm-256color",
            "HOME=${homeDir.absolutePath}",
            "TMPDIR=${context.cacheDir.absolutePath}",
            "PATH=/system/bin:/system/xbin",
            "ANDROID_ROOT=${System.getenv("ANDROID_ROOT")}",
            "ANDROID_DATA=${System.getenv("ANDROID_DATA")}",
            "EXTERNAL_STORAGE=${System.getenv("EXTERNAL_STORAGE")}"
        )
    }
    
    fun getAvailableShells(): List<String> {
        val shells = mutableListOf<String>()
        
        // Check common shell locations
        listOf(
            "/system/bin/sh",
            "/system/bin/bash",
            "/system/xbin/bash"
        ).forEach { path ->
            if (File(path).exists()) {
                shells.add(path)
            }
        }
        
        return shells
    }
}
```

### **3. TerminalSession.kt**

```kotlin
// feature/terminal/src/main/java/com/mixlauncher/feature/terminal/emulator/TerminalSession.kt

package com.mixlauncher.feature.terminal.emulator

import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.launch
import timber.log.Timber
import java.io.IOException
import java.io.OutputStream
import java.util.UUID

class TerminalSession(
    private val shellPath: String,
    private val cwd: String,
    private val env: Array<String>
) {
    
    val sessionId: String = UUID.randomUUID().toString()
    
    private var process: Process? = null
    private var outputStream: OutputStream? = null
    
    private val _output = MutableStateFlow("")
    val output: StateFlow<String> = _output
    
    private val _isRunning = MutableStateFlow(false)
    val isRunning: StateFlow<Boolean> = _isRunning
    
    private val scope = CoroutineScope(Dispatchers.IO)
    
    init {
        startShell()
    }
    
    private fun startShell() {
        try {
            Timber.d("Starting shell: $shellPath")
            
            process = ProcessBuilder()
                .command(shellPath)
                .directory(java.io.File(cwd))
                .redirectErrorStream(true)
                .apply {
                    environment().clear()
                    env.forEach { envVar ->
                        val parts = envVar.split("=", limit = 2)
                        if (parts.size == 2) {
                            environment()[parts[0]] = parts[1]
                        }
                    }
                }
                .start()
            
            outputStream = process?.outputStream
            _isRunning.value = true
            
            // Read output
            scope.launch {
                readOutput()
            }
            
            Timber.d("Shell started successfully")
            
        } catch (e: IOException) {
            Timber.e(e, "Failed to start shell")
            _isRunning.value = false
        }
    }
    
    private suspend fun readOutput() {
        process?.inputStream?.bufferedReader()?.use { reader ->
            try {
                while (_isRunning.value) {
                    val line = reader.readLine() ?: break
                    _output.value += line + "\n"
                }
            } catch (e: IOException) {
                Timber.e(e, "Error reading shell output")
            }
        }
        _isRunning.value = false
    }
    
    fun write(text: String) {
        scope.launch {
            try {
                outputStream?.write(text.toByteArray())
                outputStream?.flush()
            } catch (e: IOException) {
                Timber.e(e, "Error writing to shell")
            }
        }
    }
    
    fun destroy() {
        Timber.d("Destroying session: $sessionId")
        
        _isRunning.value = false
        
        try {
            outputStream?.close()
            process?.destroy()
        } catch (e: Exception) {
            Timber.e(e, "Error destroying session")
        }
    }
}
```

### **4. CodeServerManager.kt**

```kotlin
// feature/codeserver/src/main/java/com/mixlauncher/feature/codeserver/server/CodeServerManager.kt

package com.mixlauncher.feature.codeserver.server

import android.content.Context
import com.mixlauncher.core.common.Result
import dagger.hilt.android.qualifiers.ApplicationContext
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.delay
import kotlinx.coroutines.withContext
import timber.log.Timber
import java.io.File
import java.net.HttpURLConnection
import java.net.URL
import javax.inject.Inject
import javax.inject.Singleton

@Singleton
class CodeServerManager @Inject constructor(
    @ApplicationContext private val context: Context,
    private val binaryExtractor: BinaryExtractor
) {
    
    private var serverProcess: Process? = null
    private var isServerRunning = false
    
    suspend fun startServer(config: ServerConfig): Result<String> {
        return withContext(Dispatchers.IO) {
            try {
                Timber.d("Starting code-server...")
                
                if (isServerRunning) {
                    Timber.w("Server already running")
                    return@withContext Result.Success("http://127.0.0.1:${config.port}")
                }
                
                // Extract binary
                val binaryPath = binaryExtractor.extractCodeServer()
                Timber.d("Binary extracted to: $binaryPath")
                
                // Make executable
                Runtime.getRuntime().exec("chmod 755 $binaryPath").waitFor()
                
                // Start process
                serverProcess = ProcessBuilder()
                    .command(
                        binaryPath,
                        "--bind-addr", "127.0.0.1:${config.port}",
                        "--auth", "none",
                        "--user-data-dir", config.dataDir,
                        "--disable-telemetry",
                        "--disable-update-check"
                    )
                    .redirectErrorStream(true)
                    .start()
                
                Timber.d("Process started, waiting for server ready...")
                
                // Wait for server to be ready
                val serverUrl = waitForServerReady(config.port, maxAttempts = 30)
                
                if (serverUrl != null) {
                    isServerRunning = true
                    Timber.d("Code-server started successfully at $serverUrl")
                    Result.Success(serverUrl)
                } else {
                    stopServer()
                    Result.Error(Exception("Server failed to start"))
                }
                
            } catch (e: Exception) {
                Timber.e(e, "Failed to start code-server")
                Result.Error(e)
            }
        }
    }
    
    private suspend fun waitForServerReady(
        port: Int,
        maxAttempts: Int = 30
    ): String? {
        val url = "http://127.0.0.1:$port"
        
        repeat(maxAttempts) { attempt ->
            try {
                val connection = URL(url).openConnection() as HttpURLConnection
                connection.connectTimeout = 1000
                connection.connect()
                
                if (connection.responseCode == 200) {
                    connection.disconnect()
                    Timber.d("Server ready after ${attempt + 1} attempts")
                    return url
                }
                
                connection.disconnect()
            } catch (e: Exception) {
                // Server not ready yet
            }
            
            delay(1000)
        }
        
        Timber.e("Server failed to respond after $maxAttempts attempts")
        return null
    }
    
    fun stopServer() {
        Timber.d("Stopping code-server...")
        
        serverProcess?.destroy()
        serverProcess = null
        isServerRunning = false
        
        Timber.d("Code-server stopped")
    }
    
    fun isRunning(): Boolean = isServerRunning
    
    fun getServerUrl(port: Int): String {
        return "http://127.0.0.1:$port"
    }
}
```

### **5. BinaryExtractor.kt**

```kotlin
// feature/codeserver/src/main/java/com/mixlauncher/feature/codeserver/server/BinaryExtractor.kt

package com.mixlauncher.feature.codeserver.server

import android.content.Context
import android.os.Build
import dagger.hilt.android.qualifiers.ApplicationContext
import timber.log.Timber
import java.io.File
import java.io.FileOutputStream
import javax.inject.Inject
import javax.inject.Singleton

@Singleton
class BinaryExtractor @Inject constructor(
    @ApplicationContext private val context: Context
) {
    
    private val binaryDir: File = File(context.filesDir, "binaries")
    
    fun extractCodeServer(): String {
        val arch = getArchitecture()
        val binaryName = "code-server"
        
        val targetFile = File(binaryDir, binaryName)
        
        // If already extracted, return path
        if (targetFile.exists()) {
            Timber.d("Binary already extracted")
            return targetFile.absolutePath
        }
        
        // Create directory
        binaryDir.mkdirs()
        
        // Copy from assets
        val assetPath = "binaries/$arch/$binaryName"
        
        try {
            context.assets.open(assetPath).use { input ->
                FileOutputStream(targetFile).use { output ->
                    input.copyTo(output)
                }
            }
            
            Timber.d("Binary extracted successfully")
            return targetFile.absolutePath
            
        } catch (e: Exception) {
            Timber.e(e, "Failed to extract binary")
            throw e
        }
    }
    
    private fun getArchitecture(): String {
        val abi = Build.SUPPORTED_ABIS[0]
        
        return when {
            abi.contains("arm64") -> "arm64-v8a"
            abi.contains("armeabi") -> "armeabi-v7a"
            abi.contains("x86_64") -> "x86_64"
            else -> throw UnsupportedOperationException("Unsupported architecture: $abi")
        }
    }
}
```

### **6. DevToolsService.kt**

```kotlin
// feature/devtools/src/main/java/com/mixlauncher/feature/devtools/service/DevToolsService.kt

package com.mixlauncher.feature.devtools.service

import android.content.Intent
import android.os.Binder
import android.os.IBinder
import androidx.lifecycle.lifecycleScope
import com.mixlauncher.core.service.BaseForegroundService
import com.mixlauncher.feature.codeserver.server.CodeServerManager
import com.mixlauncher.feature.codeserver.server.ServerConfig
import com.mixlauncher.feature.terminal.service.TerminalService
import dagger.hilt.android.AndroidEntryPoint
import kotlinx.coroutines.launch
import timber.log.Timber
import javax.inject.Inject

@AndroidEntryPoint
class DevToolsService : BaseForegroundService() {
    
    @Inject
    lateinit var codeServerManager: CodeServerManager
    
    private val binder = DevToolsBinder()
    
    override fun onCreate() {
        super.onCreate()
        Timber.d("DevToolsService created")
        
        // Start code server automatically
        lifecycleScope.launch {
            val result = codeServerManager.startServer(
                ServerConfig(
                    port = 8080,
                    dataDir = filesDir.absolutePath
                )
            )
            
            result.onSuccess { url ->
                Timber.d("Code-server started at: $url")
            }.onError { error ->
                Timber.e(error, "Failed to start code-server")
            }
        }
    }
    
    override fun onBind(intent: Intent): IBinder {
        return binder
    }
    
    override fun onDestroy() {
        Timber.d("DevToolsService destroyed")
        
        // Stop code server
        codeServerManager.stopServer()
        
        super.onDestroy()
    }
    
    inner class DevToolsBinder : Binder() {
        fun getService(): DevToolsService = this@DevToolsService
    }
}
```

### **7. BaseForegroundService.kt**

```kotlin
// core/service/src/main/java/com/mixlauncher/core/service/BaseForegroundService.kt

package com.mixlauncher.core.service

import android.app.Notification
import android.app.NotificationChannel
import android.app.NotificationManager
import android.app.Service
import android.content.Intent
import android.os.Build
import androidx.core.app.NotificationCompat
import com.mixlauncher.core.service.R
import timber.log.Timber

abstract class BaseForegroundService : Service() {
    
    companion object {
        private const val CHANNEL_ID = "dev_tools_channel"
        private const val NOTIFICATION_ID = 1001
    }
    
    override fun onCreate() {
        super.onCreate()
        createNotificationChannel()
        startForeground(NOTIFICATION_ID, createNotification())
        Timber.d("${this::class.simpleName} started as foreground service")
    }
    
    private fun createNotificationChannel() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel(
                CHANNEL_ID,
                "Dev Tools Service",
                NotificationManager.IMPORTANCE_LOW
            ).apply {
                description = "Keeps development tools running in background"
            }
            
            val manager = getSystemService(NotificationManager::class.java)
            manager.createNotificationChannel(channel)
        }
    }
    
    private fun createNotification(): Notification {
        return NotificationCompat.Builder(this, CHANNEL_ID)
            .setContentTitle("Mix Launcher Dev Tools")
            .setContentText("Terminal and Code Server are running")
            .setSmallIcon(R.drawable.ic_terminal)
            .setPriority(NotificationCompat.PRIORITY_LOW)
            .setOngoing(true)
            .build()
    }
    
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        return START_STICKY
    }
}
```

---

## 📅 **Development Timeline**

### **Week 1-2: Terminal Implementation**
- [ ] Setup terminal emulator library
- [ ] Create TerminalView component with Compose
- [ ] Implement shell session management
- [ ] Add terminal widget (resizable)
- [ ] Fullscreen terminal mode
- [ ] Custom terminal keyboard
- [ ] Session persistence

### **Week 3-4: Code Server Integration**
- [ ] Download and prepare code-server binaries
- [ ] Implement binary extraction logic
- [ ] Create process management system
- [ ] WebView integration for VS Code UI
- [ ] Auto-start service on boot
- [ ] Watchdog for process monitoring
- [ ] Widget for quick access

### **Week 5-6: File Manager**
- [ ] File browser UI with Compose
- [ ] File operations (copy, move, delete, rename)
- [ ] File search functionality
- [ ] Text file preview
- [ ] Image preview
- [ ] Code syntax highlighting preview
- [ ] Integration with terminal (open in terminal)

### **Week 7-8: Service Optimization & Testing**
- [ ] Foreground service implementation
- [ ] Battery optimization exemption handling
- [ ] Process monitoring and auto-restart
- [ ] Memory management
- [ ] Performance profiling
- [ ] Widget optimization
- [ ] End-to-end testing

---

## 🧪 **Testing Scenarios**

#### **Terminal Testing**
```
# Test Cases
1. Open terminal widget
2. Run: ls -la
3. Run: cd /sdcard && ls
4. Run: echo "Hello World"
5. Create multiple sessions
6. Switch between sessions
7. Test long-running commands (e.g., ping)
8. Lock screen for 10 minutes
9. Return and verify terminal still running
10. Test fullscreen mode
```

#### **Code-Server Testing**
```
# Test Cases
1. Start code-server service
2. Open in widget
3. Create new file
4. Write some code
5. Save file
6. Open file manager
7. Verify file exists
8. Test Git operations
9. Install extension
10. Restart device and verify auto-start
```

#### **Service Persistence Testing**
```
# Scenarios
1. Lock screen for 30 minutes
2. Open multiple heavy apps
3. Enable battery saver mode
4. Reboot device
5. Force stop app from settings
6. Use aggressive task killer apps

# Expected Result
Services should survive or auto-restart in all scenarios
```

---

## 📦** Binary Preparation**

### **Download Code-Server**
```bash
#!/bin/bash
# scripts/download_binaries.sh

VERSION="4.20.0"

# ARM64
wget https://github.com/coder/code-server/releases/download/v${VERSION}/code-server-${VERSION}-linux-arm64.tar.gz
tar -xzf code-server-${VERSION}-linux-arm64.tar.gz
cp code-server-${VERSION}-linux-arm64/bin/code-server app/src/main/assets/binaries/arm64-v8a/

# ARMv7
wget https://github.com/coder/code-server/releases/download/v${VERSION}/code-server-${VERSION}-linux-armv7l.tar.gz
tar -xzf code-server-${VERSION}-linux-armv7l.tar.gz
cp code-server-${VERSION}-linux-armv7l/bin/code-server app/src/main/assets/binaries/armeabi-v7a/
```

> chmod +x app/src/main/assets/binaries/*/code-server

### **Asset Directory Structure**
```
app/src/main/assets/
└── binaries/
    ├── arm64-v8a/
    │   └── code-server
    └── armeabi-v7a/
        └── code-server
```

---

## **Common Issues & Solutions**

#### **Issue: Code-Server won't start**
**Solution:**
```
// Check binary permissions
Runtime.getRuntime().exec("chmod 755 $binaryPath").waitFor()

// Verify port not in use
val isPortAvailable = checkPort(8080)

// Check logs
adb logcat | grep CodeServer
```

#### **Issue: Terminal crashes on certain commands**
**Solution:**

- Check shell binary exists: `/system/bin/sh`
- Verify environment variables are set correctly
- Check SELinux permissions
- Test with different shell (bash, sh)

#### **Issue: Services killed by system**
**Solution:**
```
// Request battery optimization exemption
val intent = Intent().apply {
    action = Settings.ACTION_REQUEST_IGNORE_BATTERY_OPTIMIZATIONS
    data = Uri.parse("package:$packageName")
}
startActivity(intent)

// Use START_STICKY
override fun onStartCommand(...): Int {
    return START_STICKY
}

// Implement Watchdog
WatchdogService.start(context)
Issue: WebView not loading code-server
Solution:
webView.settings.apply {
    javaScriptEnabled = true
    domStorageEnabled = true
    allowFileAccess = true
    allowContentAccess = true
    mixedContentMode = WebSettings.MIXED_CONTENT_ALWAYS_ALLOW
}
```

---

## 🎯 **Phase 2 Deliverables**

**Upon completion:**
- ✅ Working terminal emulator (widget + fullscreen)
- ✅ VS Code running via code-server
- ✅ Functional file manager
- ✅ Services that survive system pressure
- ✅ Widget configuration screens
- ✅ Auto-start on boot
- ✅ Memory usage < 300MB
- ✅ Battery drain < 5% per hour (idle)

---

## 📊 **Performance Targets**

- **Terminal startup**
* **Target:** `< 1s`
* **Measurements:** Time to first prompt

- **Code-server startup**
* **Target:** `< 5s`
* **Measurements:** Time to WebView load

- **Memory usage (idle)**
* **Target:** `< 150MB`
* **Measurements:** Android Profiler

- **Memory usage (active)**
* **Target** `< 300MB`
* **Measurements** Android Profiler

- **Battery drain (idle)**
* **Target** `< 5%/hour`
* **Measurements** Battery Historian

- **Frame rate**
* **Target** `60fps`
* **Measurements** GPU Profiler

---

## ➡️ **Next Steps**

After Phase 2:

1. Merge `phase-2-dev-tools` to develop branch
2. Tag as v0.2.0-beta
3. Create phase-3-advanced-features branch
4. Begin Phase 3 (Notifications, Themes, Plugins)

---

**Last Updated:** January 2026
**Phase Status:** 🔵 Not Started
**Target Completion:** 8 weeks

---