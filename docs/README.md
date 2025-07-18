# Android Maven Plugin Documentation

Welcome to the comprehensive documentation for the Android Maven Plugin!

## Table of Contents

1. [Getting Started](getting-started.md) - Quick start guide for new users
2. [Configuration Reference](configuration-reference.md) - Complete configuration options
3. [Goals Reference](goals-reference.md) - Detailed documentation of all plugin goals
4. [Examples](examples/) - Practical examples and use cases
5. [Advanced Topics](advanced-topics.md) - Advanced features and configurations
6. [Troubleshooting](troubleshooting.md) - Common issues and solutions

## Quick Links

- [Basic Android App Example](examples/basic-android-app/)
- [Android Library Example](examples/android-library/)
- [Testing Examples](examples/testing/)
- [NDK Example](examples/ndk-build/)
- [Multi-Module Project](examples/multi-module/)

## Plugin Overview

The Android Maven Plugin is a comprehensive Maven plugin for Android application development. It provides:

- Complete Android project lifecycle management
- APK building and packaging
- Device deployment and testing
- NDK compilation support
- Code coverage and static analysis
- Emulator management
- And much more!

## Requirements

- Maven 3.0.5 or higher
- Android SDK
- Java 8 or higher

## Quick Start

Add the plugin to your `pom.xml`:

```xml
<plugin>
    <groupId>com.simpligility.maven.plugins</groupId>
    <artifactId>android-maven-plugin</artifactId>
    <version>4.6.1</version>
    <extensions>true</extensions>
</plugin>
```

See the [Getting Started Guide](getting-started.md) for complete setup instructions.

## Community

- [GitHub Issues](https://github.com/simpligility/android-maven-plugin/issues)
- [Mailing List](https://groups.google.com/forum/?fromgroups#!forum/maven-android-developers)
- [Contributors](https://github.com/simpligility/android-maven-plugin/graphs/contributors)