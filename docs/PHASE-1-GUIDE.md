# Phase 1: Core Launcher Development Guide

**Branch:** `phase-1-core-launcher`  
**Duration:** 8 weeks  
**Status:** 🔵 Not Started
**Commit:** read on CONTRIBUTING.md
---

## 📋 **Overview**

Phase 1 focuses on building the foundational launcher functionality that allows the app to function as a complete Android launcher. This includes home screen, app drawer, widgets support, and basic settings.

---

## 🎯 **Milestone Objectives**

- ✅ Basic launcher functionality
- ✅ Home screen with app icons
- ✅ App drawer with search
- ✅ Basic widgets support
- ✅ Settings screen
- ✅ Set as default launcher

---

## 🏗️ **Module Structure**

### **App Module**

```
app/
└── src/main/java/com/mixlauncher/
    ├── LauncherApplication.kt
    ├── MainActivity.kt
    │
    ├── 📂 di/
    │   ├── AppModule.kt
    │   ├── DatabaseModule.kt
    │   └── ViewModelModule.kt
    │
    └── 📂 utils/
        ├── Constants.kt
        ├── Extensions.kt
        └── PermissionHelper.kt
```

### **Core Modules**

#### **core/common**
```
core/common/
└── src/main/java/com/mixlauncher/core/common/
    ├── Result.kt
    ├── BaseViewModel.kt
    ├── Logger.kt
    └── Constants.kt
```

#### **core/data**
```
core/data/
└── src/main/java/com/mixlauncher/core/data/
    │
    ├── 📂 local/
    │   ├── AppDatabase.kt
    │   │
    │   ├── 📂 dao/
    │   │   ├── AppInfoDao.kt
    │   │   └── SettingsDao.kt
    │   │
    │   └── 📂 entities/
    │       ├── AppInfoEntity.kt
    │       └── SettingsEntity.kt
    │
    ├── 📂 repository/
    │   ├── AppRepository.kt
    │   └── SettingsRepository.kt
    │
    └── 📂 preferences/
        └── LauncherPreferences.kt
```

#### **core/domain**
```
core/domain/
└── src/main/java/com/mixlauncher/core/domain/
    │
    ├── 📂 model/
    │   ├── AppInfo.kt
    │   ├── LauncherSettings.kt
    │   └── GridSize.kt
    │
    └── 📂 usecase/
        ├── GetInstalledAppsUseCase.kt
        ├── LaunchAppUseCase.kt
        └── SearchAppsUseCase.kt
```

#### **core/ui**
```
core/ui/
└── src/main/java/com/mixlauncher/core/ui/
    │
    ├── 📂 components/
    │   ├── AppIcon.kt
    │   ├── SearchBar.kt
    │   └── LoadingIndicator.kt
    │
    ├── 📂 theme/
    │   ├── Color.kt
    │   ├── Theme.kt
    │   └── Type.kt
    │
    └── 📂 utils/
        └── ComposeUtils.kt
```

### **Feature Modules**

#### **feature/home**
```
feature/home/
└── src/main/java/com/mixlauncher/feature/home/
    ├── HomeScreen.kt
    ├── HomeViewModel.kt
    ├── HomeState.kt
    │
    ├── 📂 components/
    │   ├── HomeScreenPager.kt
    │   ├── DockBar.kt
    │   ├── AppGrid.kt
    │   └── WallpaperLayer.kt
    │
    └── 📂 gestures/
        └── HomeGestureDetector.kt
```

#### **feature/appdrawer**
```
feature/appdrawer/
└── src/main/java/com/mixlauncher/feature/appdrawer/
    ├── AppDrawerScreen.kt
    ├── AppDrawerViewModel.kt
    ├── AppDrawerState.kt
    │
    └── 📂 components/
        ├── AppList.kt
        ├── AppSearchBar.kt
        ├── CategoryTabs.kt
        └── AlphabetScrollbar.kt
```

#### **feature/widgets**
```
feature/widgets/
└── src/main/java/com/mixlauncher/feature/widgets/
    ├── WidgetHostView.kt
    ├── WidgetManager.kt
    ├── WidgetPicker.kt
    │
    └── 📂 base/
        └── BaseWidget.kt
```

#### **feature/settings**
```
feature/settings/
└── src/main/java/com/mixlauncher/feature/settings/
    ├── SettingsScreen.kt
    ├── SettingsViewModel.kt
    │
    └── 📂 screens/
        ├── GeneralSettings.kt
        ├── AppearanceSettings.kt
        └── GestureSettings.kt
```

---

## 💻 **Key Implementation Files**

### **1. Dependencies.kt**

```kotlin
// buildSrc/src/main/kotlin/Dependencies.kt

object Versions {
    const val kotlin = "1.9.20"
    const val compose = "1.5.4"
    const val composeBom = "2023.10.01"
    const val hilt = "2.48"
    const val room = "2.6.0"
    const val coroutines = "1.7.3"
}

object Deps {
    // AndroidX Core
    const val coreKtx = "androidx.core:core-ktx:1.12.0"
    const val lifecycleRuntime = "androidx.lifecycle:lifecycle-runtime-ktx:2.7.0"
    const val activityCompose = "androidx.activity:activity-compose:1.8.2"
    
    // Compose
    const val composeBom = "androidx.compose:compose-bom:${Versions.composeBom}"
    const val composeUi = "androidx.compose.ui:ui"
    const val composeMaterial3 = "androidx.compose.material3:material3"
    const val composeUiTooling = "androidx.compose.ui:ui-tooling-preview"
    
    // Hilt
    const val hiltAndroid = "com.google.dagger:hilt-android:${Versions.hilt}"
    const val hiltCompiler = "com.google.dagger:hilt-android-compiler:${Versions.hilt}"
    const val hiltNavigation = "androidx.hilt:hilt-navigation-compose:1.1.0"
    
    // Room
    const val roomRuntime = "androidx.room:room-runtime:${Versions.room}"
    const val roomKtx = "androidx.room:room-ktx:${Versions.room}"
    const val roomCompiler = "androidx.room:room-compiler:${Versions.room}"
    
    // Coroutines
    const val coroutinesAndroid = "org.jetbrains.kotlinx:kotlinx-coroutines-android:${Versions.coroutines}"
    
    // Navigation
    const val navigationCompose = "androidx.navigation:navigation-compose:2.7.6"
    
    // Testing
    const val junit = "junit:junit:4.13.2"
    const val junitExt = "androidx.test.ext:junit:1.1.5"
    const val espresso = "androidx.test.espresso:espresso-core:3.5.1"
}
```

### **2. LauncherApplication.kt**

```kotlin
// app/src/main/java/com/mixlauncher/LauncherApplication.kt

package com.mixlauncher

import android.app.Application
import dagger.hilt.android.HiltAndroidApp
import timber.log.Timber

@HiltAndroidApp
class LauncherApplication : Application() {
    
    override fun onCreate() {
        super.onCreate()
        
        // Initialize Timber for logging
        if (BuildConfig.DEBUG) {
            Timber.plant(Timber.DebugTree())
        }
        
        // Initialize logger
        Logger.init(this)
        
        // Load initial settings
        loadSettings()
        
        Timber.d("LauncherApplication initialized")
    }
    
    private fun loadSettings() {
        // Load user preferences from DataStore or SharedPreferences
        Timber.d("Loading launcher settings...")
    }
}
```

### **3. MainActivity.kt**

```kotlin
// app/src/main/java/com/mixlauncher/MainActivity.kt

package com.mixlauncher

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface
import androidx.compose.ui.Modifier
import androidx.activity.viewModels
import dagger.hilt.android.AndroidEntryPoint
import com.mixlauncher.core.ui.theme.LauncherTheme
import timber.log.Timber

@AndroidEntryPoint
class MainActivity : ComponentActivity() {
    
    private val viewModel: MainViewModel by viewModels()
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        Timber.d("MainActivity onCreate")
        
        setContent {
            LauncherTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    LauncherScreen()
                }
            }
        }
    }
    
    @Deprecated("Deprecated in Java")
    override fun onBackPressed() {
        // Handle back button - always go to home screen
        Timber.d("Back pressed - navigating to home")
        // Don't call super to prevent launcher from closing
    }
    
    override fun onResume() {
        super.onResume()
        Timber.d("MainActivity onResume")
    }
}
```

### **4. MainViewModel.kt**

```kotlin
// app/src/main/java/com/mixlauncher/MainViewModel.kt

package com.mixlauncher

import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import dagger.hilt.android.lifecycle.HiltViewModel
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.flow.asStateFlow
import kotlinx.coroutines.launch
import javax.inject.Inject

@HiltViewModel
class MainViewModel @Inject constructor(
    // Inject use cases here
) : ViewModel() {
    
    private val _uiState = MutableStateFlow(MainUiState())
    val uiState: StateFlow<MainUiState> = _uiState.asStateFlow()
    
    init {
        loadInitialData()
    }
    
    private fun loadInitialData() {
        viewModelScope.launch {
            // Load installed apps
            // Load settings
            // Initialize launcher
        }
    }
}

data class MainUiState(
    val isLoading: Boolean = true,
    val error: String? = null
)
```

### **5. AppModule.kt (Hilt)**

```kotlin
// app/src/main/java/com/mixlauncher/di/AppModule.kt

package com.mixlauncher.di

import android.content.Context
import dagger.Module
import dagger.Provides
import dagger.hilt.InstallIn
import dagger.hilt.android.qualifiers.ApplicationContext
import dagger.hilt.components.SingletonComponent
import javax.inject.Singleton

@Module
@InstallIn(SingletonComponent::class)
object AppModule {
    
    @Provides
    @Singleton
    fun provideApplicationContext(
        @ApplicationContext context: Context
    ): Context = context
}
```

### **6. AndroidManifest.xml**

```xml
<!-- app/src/main/AndroidManifest.xml -->

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <!-- Permissions -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.QUERY_ALL_PACKAGES" />
    
    <application
        android:name=".LauncherApplication"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MixLauncher">
        
        <!-- Main Launcher Activity -->
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:launchMode="singleTask"
            android:stateNotNeeded="true"
            android:windowSoftInputMode="adjustResize">
            
            <!-- Intent filter to make this a launcher -->
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.HOME" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        
    </application>

</manifest>
```

### **7. Result.kt (Common)**

```kotlin
// core/common/src/main/java/com/mixlauncher/core/common/Result.kt

package com.mixlauncher.core.common

sealed class Result<out T> {
    data class Success<T>(val data: T) : Result<T>()
    data class Error(val exception: Throwable) : Result<Nothing>()
    object Loading : Result<Nothing>()
}

inline fun <T> Result<T>.onSuccess(action: (T) -> Unit): Result<T> {
    if (this is Result.Success) action(data)
    return this
}

inline fun <T> Result<T>.onError(action: (Throwable) -> Unit): Result<T> {
    if (this is Result.Error) action(exception)
    return this
}

inline fun <T> Result<T>.onLoading(action: () -> Unit): Result<T> {
    if (this is Result.Loading) action()
    return this
}
```

### **8. AppInfo.kt (Domain Model)**

```kotlin
// core/domain/src/main/java/com/mixlauncher/core/domain/model/AppInfo.kt

package com.mixlauncher.core.domain.model

import android.graphics.drawable.Drawable

data class AppInfo(
    val packageName: String,
    val appName: String,
    val icon: Drawable? = null,
    val isSystemApp: Boolean = false,
    val lastUsed: Long = 0L,
    val installTime: Long = 0L
)
```

### **9. GetInstalledAppsUseCase.kt**

```kotlin
// core/domain/src/main/java/com/mixlauncher/core/domain/usecase/GetInstalledAppsUseCase.kt

package com.mixlauncher.core.domain.usecase

import com.mixlauncher.core.common.Result
import com.mixlauncher.core.domain.model.AppInfo
import com.mixlauncher.core.domain.repository.AppRepository
import kotlinx.coroutines.flow.Flow
import javax.inject.Inject

class GetInstalledAppsUseCase @Inject constructor(
    private val appRepository: AppRepository
) {
    operator fun invoke(): Flow<Result<List<AppInfo>>> {
        return appRepository.getInstalledApps()
    }
}
```

### **10. HomeScreen.kt**

```kotlin
// feature/home/src/main/java/com/mixlauncher/feature/home/HomeScreen.kt

package com.mixlauncher.feature.home

import androidx.compose.foundation.layout.*
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.collectAsState
import androidx.compose.runtime.getValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import androidx.hilt.navigation.compose.hiltViewModel
import com.mixlauncher.feature.home.components.AppGrid
import com.mixlauncher.feature.home.components.DockBar
import com.mixlauncher.feature.home.components.WallpaperLayer

@Composable
fun HomeScreen(
    viewModel: HomeViewModel = hiltViewModel()
) {
    val uiState by viewModel.uiState.collectAsState()
    
    Box(modifier = Modifier.fillMaxSize()) {
        // Wallpaper layer
        WallpaperLayer()
        
        // Main content
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(16.dp)
        ) {
            // Search bar at top
            Spacer(modifier = Modifier.height(24.dp))
            
            // App grid
            AppGrid(
                apps = uiState.apps,
                onAppClick = { app ->
                    viewModel.onEvent(HomeEvent.LaunchApp(app))
                },
                modifier = Modifier.weight(1f)
            )
            
            // Dock at bottom
            DockBar(
                dockedApps = uiState.dockedApps,
                onAppClick = { app ->
                    viewModel.onEvent(HomeEvent.LaunchApp(app))
                }
            )
        }
    }
}
```

---

## 📅 **Development Timeline**

### **Week 1-2: Project Setup & Architecture**
- [ ] Initialize project structure
- [ ] Setup Hilt dependency injection
- [ ] Configure Room database
- [ ] Create base classes (BaseViewModel, Result, etc)
- [ ] Setup Compose navigation
- [ ] Configure build variants

### **Week 3-4: Home Screen**
- [ ] Implement app grid layout
- [ ] Add app icon loading with Glide/Coil
- [ ] Implement dock bar
- [ ] Add wallpaper support
- [ ] Gesture detection for swipe up
- [ ] Multi-page home screen support

### **Week 5-6: App Drawer**
- [ ] Load all installed apps
- [ ] Implement search functionality
- [ ] Add alphabetical scrollbar
- [ ] Category tabs (All, Recent, System)
- [ ] App launch functionality
- [ ] App long-press menu (info, uninstall)

### **Week 7-8: Widgets & Settings**
- [ ] Widget host implementation
- [ ] Widget picker dialog
- [ ] Widget resize and reposition
- [ ] Basic settings screen
- [ ] Theme switcher (Light/Dark)
- [ ] Grid size customization
- [ ] Icon size adjustment

---

## ✅ **Testing Checklist**

### **Manual Testing**

#### **Launcher Functionality**
- [ ] App shows up in launcher picker
- [ ] Can be set as default launcher
- [ ] Back button returns to home
- [ ] Home button works correctly
- [ ] Launcher persists after reboot

#### **Home Screen**
- [ ] Apps display correctly
- [ ] App icons load properly
- [ ] Tap to launch works
- [ ] Long press menu works
- [ ] Swipe between pages works
- [ ] Dock is functional

#### **App Drawer**
- [ ] All apps load correctly
- [ ] Search finds apps
- [ ] Alphabetical scroll works
- [ ] Category tabs work
- [ ] Launch from drawer works

#### **Widgets**
- [ ] Can add widgets
- [ ] Widgets display correctly
- [ ] Widget interactions work
- [ ] Can remove widgets
- [ ] Widget resizing works

#### **Settings**
- [ ] Can open settings
- [ ] Theme change applies
- [ ] Grid size change works
- [ ] Settings persist

### **Automated Testing**

```bash
# Run unit tests
./gradlew test

# Run instrumented tests
./gradlew connectedAndroidTest

# Generate coverage report
./gradlew jacocoTestReport
```

---

## 🐛 **Common Issues & Solutions**

### **Issue: Launcher not showing in defaults**
**Solution:** 
- Check `AndroidManifest.xml` has correct intent filters
- Ensure `android:exported="true"` is set
- Verify all three categories are present (HOME, DEFAULT, LAUNCHER)

### **Issue: Apps not loading**
**Solution:**
- Add `QUERY_ALL_PACKAGES` permission in manifest
- For Android 11+, add package queries
- Check PackageManager implementation

### **Issue: Widgets not working**
**Solution:**
- Verify `AppWidgetHost` is properly initialized
- Check widget permissions in manifest
- Ensure widget IDs are properly managed

### **Issue: App crashes on back button**
**Solution:**
- Override `onBackPressed()` in MainActivity
- Don't call `super.onBackPressed()` for launcher
- Navigate to home screen instead

---

## 📚 **Resources**

### **Documentation**
- [Android Launcher Development](https://developer.android.com/guide/topics/ui/home)
- [Jetpack Compose](https://developer.android.com/jetpack/compose)
- [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

### **Example Projects**
- [Lawnchair Launcher](https://github.com/LawnchairLauncher/lawnchair)
- [KISS Launcher](https://github.com/Neamar/KISS)

### **Tools**
- [Android Studio](https://developer.android.com/studio)
- [Scrcpy](https://github.com/Genymobile/scrcpy) - Screen mirroring for testing

---

## 🎯 **Phase 1 Deliverables**

Upon completion of Phase 1, you should have:

- ✅ Functional launcher that can be set as default
- ✅ Home screen displaying installed apps in a grid
- ✅ Working app drawer with search capability
- ✅ Basic widget support (add, remove, resize)
- ✅ Settings screen with theme and layout options
- ✅ Smooth 60fps animations
- ✅ Unit tests with >70% coverage
- ✅ Documentation updated

---

## ➡️ **Next Steps**

After completing Phase 1:
1. Merge `phase-1-core-launcher` to `develop` branch
2. Tag release as `v0.1.0-alpha`
3. Create `phase-2-dev-tools` branch
4. Begin Phase 2 development (Terminal, Code Server, File Manager)

---

## 📞 **Need Help?**

- Check [CONTRIBUTING.md](../CONTRIBUTING.md) for development guidelines
- Open an issue with `phase-1` label
- Join our Discord for real-time help
- Review [ARCHITECTURE.md](../docs/ARCHITECTURE.md) for system design

---

**Last Updated:** January 2026  
**Phase Status:** 🔵 Not Started  
**Target Completion:** 8 weeks from start
```