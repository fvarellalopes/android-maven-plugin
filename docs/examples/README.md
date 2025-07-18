# Examples

This directory contains practical examples demonstrating various use cases of the Android Maven Plugin.

## Available Examples

### [Basic Android App](basic-android-app/)
A simple Android application showing the basic setup and configuration.

**Features:**
- Basic APK building
- Device deployment
- Resource management
- Manifest configuration

### [Android Library](android-library/)
An Android library project that can be consumed by other Android projects.

**Features:**
- Library packaging (AAR)
- Dependency management
- Publishing to repositories

### [Testing](testing/)
Comprehensive testing examples including unit tests and instrumentation tests.

**Features:**
- Unit testing with JUnit
- Instrumentation testing
- Code coverage with EMMA
- UI testing with Monkey and UIAutomator

### [NDK Build](ndk-build/)
Native code compilation using Android NDK.

**Features:**
- Native library compilation
- JNI integration
- Multiple architectures
- Custom build scripts

### [Multi-Module Project](multi-module/)
A multi-module project structure with app and library modules.

**Features:**
- Parent POM configuration
- Module dependencies
- Shared resources
- Coordinated builds

## Getting Started

Each example includes:
- Complete `pom.xml` configuration
- Source code structure
- Build instructions
- Usage explanations

## Building Examples

All examples can be built using standard Maven commands:

```bash
cd example-directory
mvn clean package
```

For device deployment:
```bash
mvn android:deploy
```

## Requirements

- Android SDK installed and configured
- `ANDROID_HOME` environment variable set
- Connected Android device or running emulator
- Maven 3.0.5 or higher

## Tips

- Start with the [Basic Android App](basic-android-app/) example
- Each example builds upon concepts from previous examples
- Check the README.md in each example directory for specific instructions
- Examples are designed to work with Android API level 28 by default