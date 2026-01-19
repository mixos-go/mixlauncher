# 📁 Mix Launcher - Complete Project Structure

This document provides the complete and detailed structure of the Mix Launcher project, organized by modules and features.

---

## 🏗️ **Root Directory Structure**

```
mix-launcher/
│
├── 📄 README.md
├── 📄 CONTRIBUTING.md
├── 📄 CODE_OF_CONDUCT.md
├── 📄 LICENSE
├── 📄 CHANGELOG.md
├── 📄 SECURITY.md
│
├── 📂 .github/
│   ├── workflows/
│   │   ├── ci.yml
│   │   ├── pr-checks.yml
│   │   └── release.yml
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   ├── feature_request.md
│   │   └── custom.md
│   └── pull_request_template.md
│
├── 📂 docs/
│   ├── ARCHITECTURE.md
│   ├── SETUP.md
│   ├── API.md
│   ├── PLUGIN_DEVELOPMENT.md
│   ├── THEMING_GUIDE.md
│   ├── TROUBLESHOOTING.md
│   ├── PHASE-1-GUIDE.md
│   ├── PHASE-2-GUIDE.md
│   ├── PHASE-3-GUIDE.md
│   ├── PHASE-4-GUIDE.md
│   └── 📂 screenshots/
│
├── 📂 scripts/
│   ├── setup.sh
│   ├── build_binaries.sh
│   ├── download_binaries.sh
│   └── deploy.sh
│
├── 📂 binaries/
│   ├── 📂 code-server/
│   │   ├── arm64-v8a/
│   │   │   └── code-server
│   │   ├── armeabi-v7a/
│   │   │   └── code-server
│   │   └── README.md
│   │
│   └── 📂 node/
│       ├── arm64-v8a/
│       │   └── node
│       └── armeabi-v7a/
│           └── node
│
├── 📂 buildSrc/
│   └── src/main/kotlin/
│       ├── Dependencies.kt
│       ├── Versions.kt
│       └── Config.kt
│
├── 📂 app/
├── 📂 core/
├── 📂 feature/
├── 📂 benchmark/
├── 📂 performance/
└── 📂 qa/
```

---

## 📱 **App Module**

```
app/
├── build.gradle.kts
├── proguard-rules.pro
│
└── src/
    ├── main/
    │   ├── AndroidManifest.xml
    │   │
    │   ├── java/com/mixlauncher/
    │   │   ├── LauncherApplication.kt
    │   │   ├── MainActivity.kt
    │   │   │
    │   │   ├── 📂 di/
    │   │   │   ├── AppModule.kt
    │   │   │   ├── DatabaseModule.kt
    │   │   │   └── ViewModelModule.kt
    │   │   │
    │   │   └── 📂 utils/
    │   │       ├── Constants.kt
    │   │       ├── Extensions.kt
    │   │       └── PermissionHelper.kt
    │   │
    │   ├── res/
    │   │   ├── drawable/
    │   │   ├── layout/
    │   │   ├── mipmap/
    │   │   ├── values/
    │   │   └── xml/
    │   │
    │   └── assets/
    │
    ├── test/
    │   └── java/com/mixlauncher/
    │
    └── androidTest/
        └── java/com/mixlauncher/
```

---

## 🧩 **Core Modules**

### **core/common**

```
core/common/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/core/common/
    ├── Result.kt
    ├── BaseViewModel.kt
    ├── Logger.kt
    ├── Constants.kt
    └── 📂 extensions/
        ├── ContextExt.kt
        ├── FlowExt.kt
        └── StringExt.kt
```

### **core/data**

```
core/data/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/core/data/
    │
    ├── 📂 local/
    │   ├── AppDatabase.kt
    │   │
    │   ├── 📂 dao/
    │   │   ├── AppInfoDao.kt
    │   │   ├── SettingsDao.kt
    │   │   └── WidgetDao.kt
    │   │
    │   └── 📂 entities/
    │       ├── AppInfoEntity.kt
    │       ├── SettingsEntity.kt
    │       └── WidgetEntity.kt
    │
    ├── 📂 repository/
    │   ├── AppRepository.kt
    │   ├── AppRepositoryImpl.kt
    │   ├── SettingsRepository.kt
    │   └── SettingsRepositoryImpl.kt
    │
    └── 📂 preferences/
        └── LauncherPreferences.kt
```

### **core/domain**

```
core/domain/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/core/domain/
    │
    ├── 📂 model/
    │   ├── AppInfo.kt
    │   ├── LauncherSettings.kt
    │   ├── GridSize.kt
    │   ├── Theme.kt
    │   └── Widget.kt
    │
    ├── 📂 repository/
    │   ├── AppRepository.kt (interface)
    │   └── SettingsRepository.kt (interface)
    │
    └── 📂 usecase/
        ├── GetInstalledAppsUseCase.kt
        ├── LaunchAppUseCase.kt
        ├── SearchAppsUseCase.kt
        ├── SaveSettingsUseCase.kt
        └── GetSettingsUseCase.kt
```

### **core/ui**

```
core/ui/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/core/ui/
    │
    ├── 📂 components/
    │   ├── AppIcon.kt
    │   ├── SearchBar.kt
    │   ├── LoadingIndicator.kt
    │   ├── EmptyState.kt
    │   └── ErrorState.kt
    │
    ├── 📂 theme/
    │   ├── Color.kt
    │   ├── Theme.kt
    │   ├── Type.kt
    │   └── Shape.kt
    │
    └── 📂 utils/
        ├── ComposeUtils.kt
        └── PreviewUtils.kt
```

### **core/service**

```
core/service/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/core/service/
    ├── BaseForegroundService.kt
    ├── ServiceNotification.kt
    ├── WatchdogService.kt
    └── ServiceManager.kt
```

### **core/native**

```
core/native/
├── build.gradle.kts
│
└── src/main/
    ├── cpp/
    │   ├── native-lib.cpp
    │   └── CMakeLists.txt
    │
    └── java/com/mixlauncher/core/native/
        └── NativeLibrary.kt
```

### **core/plugin-api**

```
core/plugin-api/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/core/pluginapi/
    ├── LauncherPlugin.kt
    ├── PluginCallback.kt
    ├── PluginMetadata.kt
    └── PluginContext.kt
```

### **core/analytics**

```
core/analytics/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/core/analytics/
    ├── AnalyticsManager.kt
    ├── EventTracker.kt
    └── 📂 events/
        ├── AppLaunchEvent.kt
        ├── ScreenViewEvent.kt
        └── UserActionEvent.kt
```

---

## 🎯 **Feature Modules**

### **Phase 1 Features**

#### **feature/home**

```
feature/home/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/feature/home/
    ├── HomeScreen.kt
    ├── HomeViewModel.kt
    ├── HomeState.kt
    ├── HomeEvent.kt
    │
    ├── 📂 components/
    │   ├── HomeScreenPager.kt
    │   ├── DockBar.kt
    │   ├── AppGrid.kt
    │   ├── WallpaperLayer.kt
    │   └── PageIndicator.kt
    │
    └── 📂 gestures/
        ├── HomeGestureDetector.kt
        └── GestureHandler.kt
```

#### **feature/appdrawer**

```
feature/appdrawer/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/feature/appdrawer/
    ├── AppDrawerScreen.kt
    ├── AppDrawerViewModel.kt
    ├── AppDrawerState.kt
    ├── AppDrawerEvent.kt
    │
    └── 📂 components/
        ├── AppList.kt
        ├── AppSearchBar.kt
        ├── CategoryTabs.kt
        ├── AlphabetScrollbar.kt
        └── AppItem.kt
```

#### **feature/widgets**

```
feature/widgets/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/feature/widgets/
    ├── WidgetHostView.kt
    ├── WidgetManager.kt
    ├── WidgetPicker.kt
    ├── WidgetViewModel.kt
    │
    └── 📂 base/
        ├── BaseWidget.kt
        └── WidgetConfig.kt
```

#### **feature/settings**

```
feature/settings/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/feature/settings/
    ├── SettingsScreen.kt
    ├── SettingsViewModel.kt
    ├── SettingsState.kt
    │
    └── 📂 screens/
        ├── GeneralSettings.kt
        ├── AppearanceSettings.kt
        ├── GestureSettings.kt
        └── AboutSettings.kt
```

---

### **Phase 2 Features**

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
    │   ├── DeleteFiles.kt
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
├── build.gradle.kts
└── src/main/java/com/mixlauncher/feature/devtools/
    ├── DevToolsPanel.kt
    ├── DevToolsViewModel.kt
    ├── DevToolsState.kt
    │
    ├── 📂 quickaccess/
    │   ├── QuickAccessPanel.kt
    │   ├── QuickLaunchButtons.kt
    │   └── ToolShortcuts.kt
    │
    └── 📂 service/
        ├── DevToolsService.kt
        └── ServiceManager.kt
```

---

### **Phase 3 Features**

#### **feature/notifications**

```
feature/notifications/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/feature/notifications/
    ├── NotificationShade.kt
    ├── NotificationViewModel.kt
    ├── NotificationState.kt
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
├── build.gradle.kts
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
├── build.gradle.kts
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
├── build.gradle.kts
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
├── build.gradle.kts
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

---

### **Phase 4: Testing & Performance**

#### **benchmark/**

```
benchmark/
├── build.gradle.kts
└── src/androidTest/java/com/mixlauncher/benchmark/
    ├── LaunchBenchmark.kt
    ├── ScrollBenchmark.kt
    ├── MemoryBenchmark.kt
    ├── StartupBenchmark.kt
    └── WidgetBenchmark.kt
```

#### **performance/**

```
performance/
├── build.gradle.kts
└── src/main/java/com/mixlauncher/performance/
    ├── PerformanceMonitor.kt
    ├── MemoryOptimizer.kt
    ├── BatteryOptimizer.kt
    ├── FPSMonitor.kt
    └── 📂 profiler/
        ├── CpuProfiler.kt
        └── NetworkProfiler.kt
```

#### **qa/**

```
qa/
├── test-plan.md
├── manual-tests.md
├── regression-tests.md
│
└── automation/
    ├── ui-tests/
    ├── integration-tests/
    └── e2e-tests/
```

---

## 🗂️ **Build Configuration Files**

### **Root build.gradle.kts**

```kotlin
buildscript {
    repositories {
        google()
        mavenCentral()
    }
    dependencies {
        classpath("com.android.tools.build:gradle:8.2.0")
        classpath("org.jetbrains.kotlin:kotlin-gradle-plugin:1.9.20")
        classpath("com.google.dagger:hilt-android-gradle-plugin:2.48")
    }
}

plugins {
    id("com.android.application") version "8.2.0" apply false
    id("com.android.library") version "8.2.0" apply false
    id("org.jetbrains.kotlin.android") version "1.9.20" apply false
}
```

### **settings.gradle.kts**

```kotlin
pluginManagement {
    repositories {
        google()
        mavenCentral()
        gradlePluginPortal()
    }
}

dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
    }
}

rootProject.name = "MixLauncher"

// App module
include(":app")

// Core modules
include(":core:common")
include(":core:data")
include(":core:domain")
include(":core:ui")
include(":core:service")
include(":core:native")
include(":core:plugin-api")
include(":core:analytics")

// Feature modules - Phase 1
include(":feature:home")
include(":feature:appdrawer")
include(":feature:widgets")
include(":feature:settings")

// Feature modules - Phase 2
include(":feature:terminal")
include(":feature:codeserver")
include(":feature:filemanager")
include(":feature:devtools")

// Feature modules - Phase 3
include(":feature:notifications")
include(":feature:gestures")
include(":feature:themes")
include(":feature:recents")
include(":feature:plugins")

// Performance & Testing - Phase 4
include(":benchmark")
include(":performance")
```

### **buildSrc/src/main/kotlin/Dependencies.kt**

```kotlin
object Versions {
    const val kotlin = "1.9.20"
    const val compose = "1.5.4"
    const val composeBom = "2023.10.01"
    const val hilt = "2.48"
    const val room = "2.6.0"
    const val coroutines = "1.7.3"
    const val lifecycle = "2.7.0"
    const val navigation = "2.7.6"
}

object Deps {
    // AndroidX Core
    const val coreKtx = "androidx.core:core-ktx:1.12.0"
    const val lifecycleRuntime = "androidx.lifecycle:lifecycle-runtime-ktx:${Versions.lifecycle}"
    const val activityCompose = "androidx.activity:activity-compose:1.8.2"
    
    // Compose
    const val composeBom = "androidx.compose:compose-bom:${Versions.composeBom}"
    const val composeUi = "androidx.compose.ui:ui"
    const val composeUiGraphics = "androidx.compose.ui:ui-graphics"
    const val composeUiTooling = "androidx.compose.ui:ui-tooling-preview"
    const val composeMaterial3 = "androidx.compose.material3:material3"
    const val composeMaterialIcons = "androidx.compose.material:material-icons-extended"
    
    // Hilt
    const val hiltAndroid = "com.google.dagger:hilt-android:${Versions.hilt}"
    const val hiltCompiler = "com.google.dagger:hilt-android-compiler:${Versions.hilt}"
    const val hiltNavigation = "androidx.hilt:hilt-navigation-compose:1.1.0"
    
    // Room
    const val roomRuntime = "androidx.room:room-runtime:${Versions.room}"
    const val roomKtx = "androidx.room:room-ktx:${Versions.room}"
    const val roomCompiler = "androidx.room:room-compiler:${Versions.room}"
    
    // Coroutines
    const val coroutinesCore = "org.jetbrains.kotlinx:kotlinx-coroutines-core:${Versions.coroutines}"
    const val coroutinesAndroid = "org.jetbrains.kotlinx:kotlinx-coroutines-android:${Versions.coroutines}"
    
    // Navigation
    const val navigationCompose = "androidx.navigation:navigation-compose:${Versions.navigation}"
    
    // DataStore
    const val dataStore = "androidx.datastore:datastore-preferences:1.0.0"
    
    // Testing
    const val junit = "junit:junit:4.13.2"
    const val junitExt = "androidx.test.ext:junit:1.1.5"
    const val espresso = "androidx.test.espresso:espresso-core:3.5.1"
    const val composeUiTest = "androidx.compose.ui:ui-test-junit4"
    const val composeUiTestManifest = "androidx.compose.ui:ui-test-manifest"
    
    // Logging
    const val timber = "com.jakewharton.timber:timber:5.0.1"
}
```

### **buildSrc/src/main/kotlin/Config.kt**

```kotlin
object Config {
    const val compileSdk = 34
    const val minSdk = 26
    const val targetSdk = 34
    const val versionCode = 1
    const val versionName = "1.0.0"
    
    const val applicationId = "com.mixlauncher"
    const val testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
}
```

---

## 📊 **Module Dependencies Graph**

```
app
├── core:common
├── core:data
├── core:domain
├── core:ui
├── core:service
├── core:native
└── all feature modules

feature:home
├── core:common
├── core:domain
└── core:ui

feature:terminal
├── core:common
├── core:domain
├── core:ui
├── core:service
└── core:native

feature:codeserver
├── core:common
├── core:domain
├── core:ui
├── core:service
└── core:native
```

---

## 📝 **Notes**

### **Module Naming Convention**
- `core:*` - Shared core functionality
- `feature:*` - Feature-specific modules
- Each feature module is independent and can be developed separately

### **Package Structure**
All packages follow: `com.mixlauncher.<module>.<layer>`

Examples:
- `com.mixlauncher.core.common`
- `com.mixlauncher.feature.home`
- `com.mixlauncher.feature.terminal.emulator`

### **Development Phases**
- **Phase 1**: Core launcher functionality
- **Phase 2**: Development tools integration
- **Phase 3**: Advanced features (notifications, themes, plugins)
- **Phase 4**: Performance optimization and polish

---

## 🔗 **Related Documentation**

- [Architecture Guide](docs/ARCHITECTURE.md)
- [Setup Instructions](docs/SETUP.md)
- [Phase Development Guides](docs/)
- [Contributing Guidelines](CONTRIBUTING.md)

---

**Last Updated:** January 2026  
**Version:** 1.0.0
```