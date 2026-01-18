# Contributing to MIX Launcher

First off, thank you for considering contributing to MIX Launcher! 🎉

It's people like you that make MIX Launcher such a great tool for developers worldwide.

## Table of Contents

- [Getting Started](#getting-started)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Workflow](#development-workflow)
- [Style Guides](#style-guides)
- [Commit Messages](#commit-messages)
- [Pull Request Process](#pull-request-process)
- [Community](#community)

---

## Getting Started

### Prerequisites

Before you begin, ensure you have:

- **JDK 17** or higher
- **Android Studio Hedgehog (2023.1.1)** or later
- **Android SDK** with API 26+ and API 34
- **Git** for version control
- **NDK** (if working on native code)
- A physical Android device or emulator for testing

### Fork and Clone

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
```bash
git clone https://github.com/YOUR_USERNAME/mixlauncher.git
cd mixlauncher-develop
```

3. **Add upstream remote**:
```bash
git remote add upstream https://github.com/mixos-go/mixlauncher.git
```

4. **Run setup script**:
```bash
chmod +x scripts/setup.sh
./scripts/setup.sh
```

5. **Open in Android Studio** and sync Gradle

---

## How Can I Contribute?

### Reporting Bugs 🐛

Before creating bug reports, please check the [issue tracker](https://github.com/yourusername/launcher-phone-os/issues) to avoid duplicates.

When creating a bug report, include:

**Required Information:**
- **Clear, descriptive title**
- **Steps to reproduce** the issue
- **Expected behavior** vs **actual behavior**
- **Screenshots or logs** if applicable
- **Device information**: Model, Android version, ROM
- **App version**: Found in Settings → About

**Example Bug Report:**
```markdown
**Bug Description:**
Terminal widget crashes when running `ls -la` in /sdcard

**Steps to Reproduce:**
1. Add terminal widget to home screen
2. Navigate to /sdcard using `cd /sdcard`
3. Run `ls -la`
4. App crashes

**Expected Behavior:**
Should list all files with details

**Actual Behavior:**
App crashes with NullPointerException

**Device Info:**
- Device: Samsung Galaxy S21
- Android: 13
- ROM: OneUI 5.1
- App Version: 1.0.0-beta

**Logs:**
```
java.lang.NullPointerException at ...
```
```

### Suggesting Features 💡

Feature suggestions are welcome! Please:

1. **Check existing feature requests** first
2. **Provide clear use case** - why is this needed?
3. **Describe expected behavior** in detail
4. **Consider implementation complexity**
5. **Add mockups/wireframes** if UI-related

**Feature Request Template:**
```markdown
**Feature Name:** Quick SSH Connection

**Problem Statement:**
As a developer, I need to quickly SSH into my servers
without opening a separate terminal session.

**Proposed Solution:**
Add "SSH Connections" widget that allows:
- Saved connection profiles
- One-tap connection
- Auto-authentication with keys

**Alternatives Considered:**
- Use regular terminal (too many steps)
- Use separate SSH app (defeats launcher purpose)

**Additional Context:**
Mockup: [attach image]
Similar to: Termux SSH widget
```

### Writing Code 💻

#### Good First Issues

Look for issues labeled:
- `good first issue` - Perfect for newcomers
- `help wanted` - Community input needed
- `beginner friendly` - Easy to medium difficulty

#### Claiming Issues

1. Comment on the issue: "I'd like to work on this"
2. Wait for maintainer assignment
3. Start working within 7 days or it may be reassigned

---

## Development Workflow

### Branch Strategy

```
main (production)
  ↓
develop (integration)
  ↓
phase-X-feature-name (your work here)
```

### Creating a Branch

```bash
# Update develop branch
git checkout develop
git pull upstream develop

# Create feature branch
git checkout -b phase-2-terminal-search

# Or bug fix branch
git checkout -b fix/terminal-crash-on-ls
```

**Branch Naming Conventions:**
- Features: `phase-X-feature-name` (e.g., `phase-2-terminal-widget`)
- Bug fixes: `fix/short-description` (e.g., `fix/app-drawer-scroll`)
- Improvements: `improve/short-description` (e.g., `improve/search-performance`)
- Documentation: `docs/what-you-changed` (e.g., `docs/contributing-guide`)

### Making Changes

1. **Make your changes** in your feature branch
2. **Write tests** for new functionality
3. **Run tests** to ensure nothing breaks:
```bash
./gradlew test
./gradlew ktlintCheck
```

4. **Commit your changes** (see commit guidelines below)
5. **Push to your fork**:
```bash
git push origin phase-2-terminal-widget
```

### Keeping Your Branch Updated

```bash
# Fetch latest changes
git fetch upstream

# Rebase your branch
git checkout your-branch-name
git rebase upstream/develop

# If conflicts, resolve them and continue
git rebase --continue

# Force push (if already pushed)
git push origin your-branch-name --force-with-lease
```

---

## Style Guides

### Kotlin Code Style

We follow the official [Kotlin Coding Conventions](https://kotlinlang.org/docs/coding-conventions.html).

**Key Points:**

**1. Naming Conventions**
```kotlin
// Classes: PascalCase
class TerminalViewModel

// Functions: camelCase
fun loadInstalledApps()

// Constants: SCREAMING_SNAKE_CASE
const val MAX_TERMINAL_SESSIONS = 10

// Private properties: camelCase with underscore prefix (optional)
private val _terminalState = MutableStateFlow<TerminalState>()
```

**2. Code Organization**
```kotlin
class TerminalViewModel @Inject constructor(
    private val repository: TerminalRepository,
    private val sessionManager: SessionManager
) : ViewModel() {

    // Public properties first
    val sessions: StateFlow<List<Session>> = ...
    
    // Private properties
    private val _isLoading = MutableStateFlow(false)
    
    // Init block
    init {
        loadSessions()
    }
    
    // Public functions
    fun createSession() { }
    
    // Private functions
    private fun loadSessions() { }
    
    // Companion object last
    companion object {
        const val TAG = "TerminalViewModel"
    }
}
```

**3. Function Length**
- Keep functions under 40 lines
- Extract complex logic into separate functions
- Use meaningful function names

**4. Comments**
```kotlin
// Good: Explains WHY, not WHAT
// Workaround for Android 12+ phantom process killer
restartServiceWithDelay()

// Bad: States the obvious
// Increment counter
counter++
```

**5. Null Safety**
```kotlin
// Prefer safe calls
user?.name?.uppercase()

// Use elvis operator for defaults
val name = user?.name ?: "Unknown"

// Avoid !! unless absolutely certain
// val name = user!!.name // ❌ Avoid

// Better: Check first
val name = user?.name ?: return
```

### Jetpack Compose Guidelines

**1. Composable Naming**
```kotlin
// Composables: PascalCase (noun)
@Composable
fun TerminalScreen() { }

@Composable
fun AppIcon(app: AppInfo) { }
```

**2. State Management**
```kotlin
// Stateless composables (preferred)
@Composable
fun AppIcon(
    app: AppInfo,
    onClick: () -> Unit,
    modifier: Modifier = Modifier
) { }

// Stateful wrapper when needed
@Composable
fun StatefulAppIcon(app: AppInfo) {
    var isPressed by remember { mutableStateOf(false) }
    AppIcon(
        app = app,
        onClick = { isPressed = !isPressed }
    )
}
```

**3. Preview Annotations**
```kotlin
@Preview(name = "Light Mode")
@Preview(name = "Dark Mode", uiMode = Configuration.UI_MODE_NIGHT_YES)
@Composable
private fun AppIconPreview() {
    LauncherTheme {
        AppIcon(
            app = AppInfo(name = "Chrome", packageName = "com.android.chrome"),
            onClick = {}
        )
    }
}
```

**4. Modifier Order**
```kotlin
// Standard order
Modifier
    .fillMaxWidth()           // Size
    .padding(16.dp)           // Spacing
    .background(Color.Blue)   // Drawing
    .clickable { }            // Input
```

### XML Resources

**1. Naming Conventions**
```
// Layout files
activity_main.xml
fragment_settings.xml
item_app_icon.xml
dialog_confirmation.xml

// Drawable files
ic_launcher_foreground.xml
bg_rounded_card.xml
selector_button_state.xml

// String resources
<string name="app_name">MIX Launcher</string>
<string name="error_permission_denied">Permission denied</string>
<string name="action_settings">Settings</string>
```

**2. Dimension Resources**
```xml
<!-- Use dp for sizes -->
<dimen name="spacing_small">8dp</dimen>
<dimen name="spacing_medium">16dp</dimen>
<dimen name="spacing_large">24dp</dimen>

<!-- Use sp for text -->
<dimen name="text_size_small">12sp</dimen>
<dimen name="text_size_body">14sp</dimen>
<dimen name="text_size_heading">20sp</dimen>
```

### Architecture Guidelines

**1. Module Structure**
```
feature/
└── terminal/
    ├── data/          # Data sources, repositories
    ├── domain/        # Use cases, models
    ├── ui/            # Composables, ViewModels
    └── di/            # Dependency injection
```

**2. Dependency Rules**
- `ui` depends on `domain`
- `domain` depends on nothing (pure Kotlin)
- `data` implements `domain` interfaces
- Never reverse dependencies

**3. ViewModel Pattern**
```kotlin
@HiltViewModel
class TerminalViewModel @Inject constructor(
    private val createSessionUseCase: CreateSessionUseCase,
    private val destroySessionUseCase: DestroySessionUseCase
) : ViewModel() {

    // UI State
    private val _uiState = MutableStateFlow(TerminalUiState())
    val uiState: StateFlow<TerminalUiState> = _uiState.asStateFlow()
    
    // Events
    fun onEvent(event: TerminalEvent) {
        when (event) {
            is TerminalEvent.CreateSession -> createSession()
            is TerminalEvent.DestroySession -> destroySession(event.id)
        }
    }
    
    private fun createSession() {
        viewModelScope.launch {
            createSessionUseCase()
                .onSuccess { session ->
                    _uiState.update { it.copy(sessions = it.sessions + session) }
                }
                .onFailure { error ->
                    _uiState.update { it.copy(error = error.message) }
                }
        }
    }
}
```

---

## Commit Messages

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification.

### Format
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc)
- `refactor`: Code refactoring
- `perf`: Performance improvements
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

### Examples

**Good Commit Messages:**
```
feat(terminal): add multi-session support

Implement ability to create and manage multiple terminal sessions.
Users can now switch between sessions using tabs.

Closes #123
```

```
fix(codeserver): prevent service restart loop

Added delay and max retry count to prevent infinite restart
loop when code-server binary is corrupted.

Fixes #456
```

```
docs(readme): update installation instructions

- Add prerequisites section
- Clarify APK installation steps
- Add troubleshooting for common issues
```

**Bad Commit Messages:**
```
❌ updated stuff
❌ fix bug
❌ WIP
❌ final version
❌ fixed terminal and added feature and updated docs
```

### Commit Message Tips

1. **Use imperative mood**: "add feature" not "added feature"
2. **First line under 72 characters**
3. **Capitalize first letter** of subject
4. **No period** at end of subject
5. **Blank line** between subject and body
6. **Wrap body** at 72 characters
7. **Reference issues** in footer

---

## Pull Request Process

### Before Submitting

**Checklist:**
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex code
- [ ] Tests added/updated
- [ ] All tests passing locally
- [ ] Documentation updated
- [ ] No console warnings or errors
- [ ] Commit messages follow convention

### Creating Pull Request

1. **Push your branch** to your fork
2. **Open Pull Request** against `develop` branch
3. **Fill PR template** completely
4. **Link related issues** using keywords (Closes #123)
5. **Add labels** (enhancement, bug, documentation, etc)
6. **Request reviews** from maintainers

### Pull Request Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix (non-breaking change fixing an issue)
- [ ] New feature (non-breaking change adding functionality)
- [ ] Breaking change (fix/feature causing existing functionality to break)
- [ ] Documentation update

## Related Issues
Closes #123
Related to #456

## Changes Made
- Added terminal multi-session support
- Implemented session tabs UI
- Updated terminal service to manage multiple sessions

## Testing
- [ ] Unit tests added/updated
- [ ] Manual testing completed
- [ ] Tested on physical device (specify model)
- [ ] Tested on emulator

**Test Devices:**
- Samsung Galaxy S21 (Android 13)
- Pixel 6 Emulator (Android 14)

## Screenshots
<!-- Add before/after screenshots if UI changes -->

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-reviewed code
- [ ] Commented complex code
- [ ] Updated documentation
- [ ] No new warnings
- [ ] Tests pass locally
- [ ] Commit messages follow convention

## Additional Notes
Any other information reviewers should know
```

### Review Process

1. **Automated checks** must pass (CI/CD)
2. **At least one maintainer approval** required
3. **Address review comments** promptly
4. **Update your PR** based on feedback
5. **Resolve conflicts** if any
6. **Squash commits** if requested

### After Merge

1. **Delete your branch** (optional)
```bash
git branch -d phase-2-terminal-widget
git push origin --delete phase-2-terminal-widget
```

2. **Update your fork**
```bash
git checkout develop
git pull upstream develop
git push origin develop
```

---

## Testing Guidelines

### Unit Tests

```kotlin
class TerminalViewModelTest {
    
    @get:Rule
    val instantExecutorRule = InstantTaskExecutorRule()
    
    private lateinit var viewModel: TerminalViewModel
    private lateinit var fakeRepository: FakeTerminalRepository
    
    @Before
    fun setup() {
        fakeRepository = FakeTerminalRepository()
        viewModel = TerminalViewModel(fakeRepository)
    }
    
    @Test
    fun `createSession should add session to state`() = runTest {
        // Given
        val initialCount = viewModel.uiState.value.sessions.size
        
        // When
        viewModel.onEvent(TerminalEvent.CreateSession)
        advanceUntilIdle()
        
        // Then
        val newCount = viewModel.uiState.value.sessions.size
        assertThat(newCount).isEqualTo(initialCount + 1)
    }
}
```

### UI Tests

```kotlin
@RunWith(AndroidJUnit4::class)
class TerminalScreenTest {
    
    @get:Rule
    val composeTestRule = createComposeRule()
    
    @Test
    fun terminalScreen_displaysSessionTabs() {
        // Given
        val sessions = listOf(
            Session(id = "1", name = "Session 1"),
            Session(id = "2", name = "Session 2")
        )
        
        // When
        composeTestRule.setContent {
            TerminalScreen(sessions = sessions)
        }
        
        // Then
        composeTestRule
            .onNodeWithText("Session 1")
            .assertIsDisplayed()
        composeTestRule
            .onNodeWithText("Session 2")
            .assertIsDisplayed()
    }
}
```

### Running Tests

```bash
# All tests
./gradlew test

# Specific module
./gradlew :feature:terminal:test

# UI tests
./gradlew connectedAndroidTest

# With coverage
./gradlew jacocoTestReport
```

---

## Documentation

### Code Documentation

```kotlin
/**
 * Manages terminal sessions and their lifecycle.
 *
 * This class handles creation, destruction, and monitoring of terminal
 * sessions. It ensures proper cleanup when sessions are no longer needed.
 *
 * @property context Application context for resource access
 * @property maxSessions Maximum number of concurrent sessions allowed
 */
class TerminalSessionManager(
    private val context: Context,
    private val maxSessions: Int = 10
) {
    
    /**
     * Creates a new terminal session.
     *
     * @param sessionId Unique identifier for the session
     * @param workingDir Initial working directory (default: home)
     * @return Created session or null if max sessions reached
     * @throws SecurityException if permissions are insufficient
     */
    fun createSession(
        sessionId: String,
        workingDir: File = homeDirectory
    ): TerminalSession? {
        // Implementation
    }
}
```

### README Updates

When adding features, update relevant sections:
- Features list
- Screenshots (if UI changes)
- Installation instructions (if requirements change)
- API documentation (if public API changes)

---

## Community

### Communication Channels

- **GitHub Issues**: Bug reports, feature requests
- **GitHub Discussions**: General questions, ideas
- **Discord**: Real-time chat (coming soon)

### Getting Help

1. **Check documentation** first
2. **Search existing issues**
3. **Ask in Discussions** for general questions
4. **Create issue** for bugs/features
5. **Join Discord** for quick questions

### Recognition

Contributors will be:
- Listed in CONTRIBUTORS.md
- Mentioned in release notes
- Credited in app About section (for significant contributions)

---

## Development Resources

### Useful Links

- [Android Developers](https://developer.android.com/)
- [Kotlin Documentation](https://kotlinlang.org/docs/home.html)
- [Jetpack Compose](https://developer.android.com/jetpack/compose)
- [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- [Terminal Emulator Resources](https://github.com/termux/termux-app/wiki)

### Project Documentation

- [Architecture Guide](docs/ARCHITECTURE.md)
- [Phase Development Guides](docs/)
- [API Documentation](docs/API.md)
- [Plugin Development](docs/PLUGIN_DEVELOPMENT.md)

---

## License

By contributing, you agree that your contributions will be licensed under the same MIT License that covers this project.

---

## Questions?

Don't hesitate to reach out if you have questions:
- Open a [Discussion](https://github.com/mixos-go/mixlauncher/discussions)
- Create an issue with `question` label

**Thank you for contributing! 🚀**

---

*Last updated: January 2026*
