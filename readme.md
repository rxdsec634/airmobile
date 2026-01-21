<p align="center">
  <img src="assets/logo.png" alt="AirMobile Logo" width="120"/>
</p>

<h1 align="center">AirMobile</h1>
<p align="center"><strong>Enterprise-Grade WiFi Security Suite for Android</strong></p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Android%208.0+-brightgreen?style=flat-square"/>
  <img src="https://img.shields.io/badge/Architecture-ARM64%20|%20ARMv7-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/Root-Required-red?style=flat-square"/>
  <img src="https://img.shields.io/badge/Flutter-3.x-02569B?style=flat-square&logo=flutter"/>
  <img src="https://img.shields.io/badge/Dart_FFI-Native-00B4AB?style=flat-square&logo=dart"/>
  <img src="https://img.shields.io/badge/C%2B%2B-NDK%20r25+-00599C?style=flat-square&logo=cplusplus"/>
</p>

<p align="center">
  <a href="#features">Features</a> •
  <a href="#architecture">Architecture</a> •
  <a href="#tech-stack">Tech Stack</a> •
  <a href="#getting-started">Getting Started</a> •
  <a href="#development">Development</a>
</p>

---

## Overview

**AirMobile** unifies industry-standard WiFi penetration testing tools into a single, production-grade Android application. Built with Flutter for premium UI and C/C++ via Dart FFI for maximum performance.

### Integrated Tools

| Tool | Function | Status |
|------|----------|--------|
| **airmon-ng** | Monitor mode controller | Core |
| **hcxdumptool** | PMKID/EAPOL capture | Core |
| **aireplay-ng** | Packet injection | Attack |
| **hcxtools** | Hash conversion | Utility |
| **hashcat** | Password cracking | Cracker |
| **airbase-ng** | Rogue AP | Advanced |
| **packetforge-ng** | Packet crafting | Advanced |

---

## Features

### 📡 Network Scanner
- Real-time AP discovery with signal heatmap
- Client enumeration & vendor fingerprinting
- Hidden SSID detection
- WPS vulnerability identification

### 📥 Capture Engine
- PMKID extraction (clientless)
- 4-way handshake capture
- GPS-tagged sessions
- Auto-export to hashcat format

### ⚡ Attack Framework
- Deauthentication attacks
- Evil Twin AP
- Beacon flood
- Custom packet injection

### 🔓 Cracking Engine
- Local CPU cracking (hashcat)
- Cloud GPU offload option
- Dictionary/mask/rule attacks
- Session persistence

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                 Presentation (Flutter)                       │
│  Screens → Widgets → Riverpod Providers                     │
├─────────────────────────────────────────────────────────────┤
│                 Domain (Pure Dart)                           │
│  Entities → Use Cases → Repository Interfaces               │
├─────────────────────────────────────────────────────────────┤
│                 Data (Implementation)                        │
│  Models → Repositories → Data Sources                       │
├─────────────────────────────────────────────────────────────┤
│                 Native Bridge (Dart FFI)                     │
│  Bindings → Type Conversions → Memory Management            │
├─────────────────────────────────────────────────────────────┤
│                 Native Layer (C/C++)                         │
│  airmon │ hcxdump │ aireplay │ hcxtools │ hashcat           │
├─────────────────────────────────────────────────────────────┤
│                 System (Android/Linux)                       │
│  Root Access │ USB OTG │ Kernel Drivers                     │
└─────────────────────────────────────────────────────────────┘
```

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| **UI Framework** | Flutter 3.x |
| **State Management** | Riverpod 2.x + Freezed |
| **Navigation** | GoRouter |
| **DI** | get_it + injectable |
| **Database** | Drift (SQLite) |
| **Encryption** | libsodium |
| **Native Bridge** | Dart FFI |
| **Native Code** | C17/C++20 |
| **Build** | CMake + Gradle |

---

## Getting Started

### Requirements

| Requirement | Specification |
|-------------|---------------|
| Android | 8.0+ (API 26+) |
| Architecture | ARM64-v8a |
| Root | Magisk 24+ |
| Adapter | RTL8812AU, MT7612U, AR9271 |

### Installation

```bash
# Clone
git clone https://github.com/user/airmobile.git && cd airmobile

# Dependencies
flutter pub get

# Build native
cd native && mkdir build && cd build
cmake -DCMAKE_TOOLCHAIN_FILE=$NDK/build/cmake/android.toolchain.cmake \
      -DANDROID_ABI=arm64-v8a -DANDROID_PLATFORM=android-26 ..
make -j$(nproc)
cd ../..

# Build APK
flutter build apk --release
```

---

## Development

### Project Structure

```
lib/
├── core/           # DI, exceptions, constants
├── domain/         # Entities, use cases, interfaces
├── data/           # Repositories, data sources
├── presentation/   # Screens, widgets, providers
├── native/         # FFI bindings
└── main.dart

native/
├── include/        # Headers
├── src/            # C/C++ implementation
└── CMakeLists.txt
```

### Key Patterns

**Result Type (Error Handling)**
```dart
sealed class Result<T> {}
final class Success<T> extends Result<T> { final T data; }
final class Failure<T> extends Result<T> { final AppException error; }
```

**Use Case Pattern**
```dart
class StartMonitorModeUseCase implements UseCase<String, String> {
  Future<Result<String>> call(String interface) async { ... }
}
```

**FFI Bridge**
```dart
final class AirmonBindings {
  late final int Function(Pointer<Utf8>) startMonitor;
  AirmonBindings(DynamicLibrary lib) {
    startMonitor = lib.lookupFunction<...>('airmon_start');
  }
}
```

---

## Roadmap

- [x] Architecture design
- [ ] **Phase 1**: Foundation (Weeks 1-3)
- [ ] **Phase 2**: Core tools (Weeks 4-7)
- [ ] **Phase 3**: Attack modules (Weeks 8-10)
- [ ] **Phase 4**: Cracking engine (Weeks 11-14)
- [ ] **Phase 5**: Production polish (Weeks 15-16)

---

## Security

| Measure | Implementation |
|---------|----------------|
| Command Injection | Input sanitization, allowlist |
| Storage | AES-256 encryption |
| Memory | Zero-fill after use |
| Root Commands | Minimal privilege |

---

## Disclaimer

> **For authorized security testing only.** Unauthorized network access is illegal. Users are responsible for compliance with applicable laws.

---

<p align="center"><strong>Built with ❤️ for security researchers</strong></p>

