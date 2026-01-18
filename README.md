# MIX Launcher

A revolutionary Android launcher designed for developers, featuring integrated development tools including terminal emulator, code-server (VS Code), and file manager - all accessible directly from your home screen.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Android](https://img.shields.io/badge/Platform-Android-green.svg)](https://android.com)
[![Kotlin](https://img.shields.io/badge/Language-Kotlin-purple.svg)](https://kotlinlang.org)
[![Min SDK](https://img.shields.io/badge/Min%20SDK-26-blue.svg)](https://developer.android.com/about/versions/oreo)

## 🌟 Features

### Core Launcher Features
- ✨ Modern, customizable home screen
- 🎨 Multiple theme support with icon packs
- 🔍 Smart app search and organization
- 📲 Custom notification system
- 👆 Advanced gesture controls
- 🔄 Recent apps / Task switcher

### Dev Tools (Unique)
- 💻 **Integrated Terminal Emulator** - Full-featured terminal accessible as widget or fullscreen
- 🖥️ **Built-in Code Server** - VS Code running directly on your phone
- 📁 **File Manager** - Advanced file browsing with code preview
- 🔌 **Plugin System** - Extend functionality with custom plugins
- 🛠️ **Developer Dashboard** - Quick access to all dev tools

## 🎯 Why MIX Launcher?

Another launchers focus on aesthetics and productivity. **MIX Launcher** goes further by providing a complete development environment on your Android device, making it perfect for:

- 👨‍💻 Developers who want to code on-the-go
- 🎓 Students learning programming
- 🔧 System administrators needing terminal access
- 🚀 Power users who want maximum control

## 📸 Screenshots

_Coming soon..._

## 🚀 Getting Started

### Prerequisites
- Android 8.0 (API 26) or higher
- 2GB RAM minimum (4GB recommended)
- 500MB free storage

### Installation

#### From Releases (Recommended)
1. Download the latest APK from [Releases](https://github.com/mixos-go/mixlauncher/releases)
2. Install the APK
3. Set as default launcher in Android settings
4. Grant necessary permissions

#### From Source
```bash
# Clone repository
git clone https://github.com/mixos-go/mixlauncher.git
cd mixlauncher-main

# Checkout desired phase
git checkout phase-1-core-launcher

# Build
./gradlew assembleDebug

# Install
adb install app/build/outputs/apk/debug/app-debug.apk
```

## 🏗️ Architecture

**Tech Stack:**
- **Language:** Kotlin
- **UI:** Jetpack Compose + Material 3
- **Architecture:** MVVM + Clean Architecture
- **DI:** Hilt
- **Database:** Room
- **Async:** Coroutines + Flow
- **Navigation:** Compose Navigation

## 📋 Development Phases

### Phase 1: Core Launcher 🔄 (In-Progress)
- [ ] Home screen implementation
- [ ] App drawer
- [ ] Widget support
- [ ] Basic settings

### Phase 2: Dev Tools (Planned)
- [ ] Terminal emulator
- [ ] Code-server integration
- [ ] File manager
- [ ] Background services

### Phase 3: Advanced Features 📅 (Planned)
- [ ] Custom notifications
- [ ] Gesture system
- [ ] Theme engine
- [ ] Plugin architecture

### Phase 4: Polish & Release 📅 (Planned)
- [ ] Performance optimization
- [ ] Battery optimization
- [ ] Documentation
- [ ] Play Store release

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

### Development Setup

1. **Clone the repository**
```bash
git clone https://github.com/mixos-go/mixlauncher.git
cd mixlauncher-develop
```

2. **Run setup script**
```bash
chmod +x scripts/setup.sh
./scripts/setup.sh
```

3. **Open in Android Studio**
- Open Android Studio
- Select "Open an Existing Project"
- Navigate to cloned directory
- Wait for Gradle sync

4. **Run on device**
```bash
./gradlew installDebug
```

### Code Style
- Follow [Kotlin Coding Conventions](https://kotlinlang.org/docs/coding-conventions.html)
- Use ktlint for formatting: `./gradlew ktlintFormat`
- Run tests before PR: `./gradlew test`

## Testing

```bash
# Unit tests
./gradlew test

# Instrumented tests
./gradlew connectedAndroidTest

# Code coverage
./gradlew jacocoTestReport
```

## 📖 Documentation

- [Architecture Guide](docs/ARCHITECTURE.md)
- [API Documentation](docs/API.md)
- [Plugin Development](docs/PLUGIN_DEVELOPMENT.md)
- [Theming Guide](docs/THEMING_GUIDE.md)
- [Troubleshooting](docs/TROUBLESHOOTING.md)

## Known Issues

- Terminal may be killed on aggressive OEM devices (Xiaomi, Oppo) - enable battery optimization exemption
- Code-server startup may take 5-10 seconds on first launch
- Widget resizing limited by Android system constraints

See [Issues](https://github.com/mixos-go/mixlauncher/issues) for full list.

## 🗺️ Roadmap

**v1.0 (Q2 2026)**
- Complete Phase 1-4
- Play Store release
- Documentation complete

**v1.1 (Q3 2026)**
- Git integration
- Package manager (npm, pip, etc)
- SSH client
- Cloud sync

**v2.0 (Q4 2026)**
- AI assistant integration
- Collaborative features
- Custom ROM support
- Desktop mode

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Termux](https://github.com/termux/termux-app) - Terminal emulator inspiration
- [Code-Server](https://github.com/coder/code-server) - VS Code in browser
- [Lawnchair](https://github.com/LawnchairLauncher/lawnchair) - Launcher reference
- [Nova Launcher](https://novalauncher.com/) - UI/UX inspiration

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=mixos-go/mixlauncher&type=Date)](https://star-history.com/#mixos-go/mixlauncher&Date)

---

**Made with ❤️ by developers, for developers**

*Transform your Android device into a portable development environment*

---
