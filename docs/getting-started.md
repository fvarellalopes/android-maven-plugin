# Getting Started with Android Maven Plugin

This guide will help you get started with the Android Maven Plugin for your Android development projects.

## Prerequisites

Before you begin, ensure you have the following installed:

1. **Java Development Kit (JDK) 8 or higher**
   ```bash
   java -version
   ```

2. **Apache Maven 3.0.5 or higher**
   ```bash
   mvn -version
   ```

3. **Android SDK**
   - Download from [Android Developer site](https://developer.android.com/studio)
   - Set `ANDROID_HOME` environment variable to SDK location
   - Add `$ANDROID_HOME/tools` and `$ANDROID_HOME/platform-tools` to your PATH

## Environment Setup

### Setting up ANDROID_HOME

**Linux/macOS:**
```bash
export ANDROID_HOME=/path/to/android/sdk
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
```

**Windows:**
```cmd
set ANDROID_HOME=C:\path\to\android\sdk
set PATH=%PATH%;%ANDROID_HOME%\tools;%ANDROID_HOME%\platform-tools
```

### Verify Installation

```bash
# Check if adb is accessible
adb version

# List available Android targets
android list targets
```

## Creating Your First Android Project

### 1. Generate Project Structure

You can use Maven archetype to generate a basic Android project:

```bash
mvn archetype:generate \
  -DgroupId=com.example.myapp \
  -DartifactId=my-android-app \
  -DarchetypeArtifactId=android-quickstart \
  -DarchetypeGroupId=de.akquinet.android.archetypes \
  -DinteractiveMode=false
```

### 2. Basic pom.xml Configuration

Here's a minimal `pom.xml` for an Android application:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>my-android-app</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>apk</packaging>
    
    <name>My Android App</name>
    
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <android.version>4.1.1.4</android.version>
        <android.platform>28</android.platform>
        <android-maven-plugin.version>4.6.1</android-maven-plugin.version>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>com.google.android</groupId>
            <artifactId>android</artifactId>
            <version>${android.version}</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    
    <build>
        <finalName>${project.artifactId}</finalName>
        <sourceDirectory>src</sourceDirectory>
        
        <plugins>
            <plugin>
                <groupId>com.simpligility.maven.plugins</groupId>
                <artifactId>android-maven-plugin</artifactId>
                <version>${android-maven-plugin.version}</version>
                <extensions>true</extensions>
                <configuration>
                    <sdk>
                        <platform>${android.platform}</platform>
                    </sdk>
                </configuration>
            </plugin>
            
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

### 3. Project Structure

Your Android project should follow this structure:

```
my-android-app/
├── pom.xml
├── AndroidManifest.xml
├── src/
│   └── main/
│       ├── java/
│       │   └── com/example/myapp/
│       │       └── MainActivity.java
│       └── android/
│           ├── res/
│           │   ├── layout/
│           │   │   └── main.xml
│           │   └── values/
│           │       └── strings.xml
│           └── assets/
└── target/ (generated)
```

### 4. Basic AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.example.myapp">
    
    <uses-sdk android:minSdkVersion="16" 
              android:targetSdkVersion="28"/>
    
    <application android:label="@string/app_name"
                 android:theme="@android:style/Theme.Material">
        <activity android:name=".MainActivity"
                  android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

## Building Your First App

### 1. Clean and Compile

```bash
mvn clean compile
```

### 2. Generate APK

```bash
mvn package
```

This will create an APK file in the `target/` directory.

### 3. Install to Device/Emulator

First, make sure you have a device connected or an emulator running:

```bash
adb devices
```

Then deploy your app:

```bash
mvn android:deploy
```

### 4. Run the Application

```bash
mvn android:run
```

## Common Commands

| Command | Description |
|---------|-------------|
| `mvn clean` | Clean the project |
| `mvn compile` | Compile the Java source code |
| `mvn package` | Create the APK |
| `mvn android:deploy` | Install APK to device/emulator |
| `mvn android:run` | Start the application |
| `mvn android:undeploy` | Uninstall the application |
| `mvn android:pull` | Pull files from device |
| `mvn android:push` | Push files to device |

## Next Steps

- Explore [Examples](examples/) for more complex use cases
- Read the [Configuration Reference](configuration-reference.md) for all available options
- Check out [Goals Reference](goals-reference.md) for detailed goal documentation
- Learn about [Advanced Topics](advanced-topics.md) like testing and NDK builds

## Troubleshooting

If you encounter issues, check the [Troubleshooting Guide](troubleshooting.md) for common problems and solutions.