# Goals Reference

The Android Maven Plugin provides numerous goals for different aspects of Android development. This document provides detailed information about each goal.

## Core Build Goals

### android:generate-sources
**Description**: Generates R.java and other source files from Android resources.

**Default Phase**: generate-sources

**Parameters**:
- `generateR` - Generate R.java file (default: true)
- `generateResourcedirectory` - Directory for generated sources (default: target/generated-sources/r)

**Example**:
```xml
<plugin>
    <groupId>com.simpligility.maven.plugins</groupId>
    <artifactId>android-maven-plugin</artifactId>
    <executions>
        <execution>
            <id>generate-sources</id>
            <goals>
                <goal>generate-sources</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

### android:dex
**Description**: Converts Java bytecode to Dalvik bytecode (DEX format).

**Default Phase**: process-classes

**Parameters**:
- `dexJvmArguments` - JVM arguments for DEX process
- `dexCoreLibrary` - Include core library (default: false)
- `dexNoLocals` - Optimize out local variable information (default: false)

**Example**:
```xml
<configuration>
    <dex>
        <jvmArguments>-Xmx1024m</jvmArguments>
        <coreLibrary>false</coreLibrary>
        <noLocals>false</noLocals>
    </dex>
</configuration>
```

### android:apk
**Description**: Creates the Android application package (APK).

**Default Phase**: package

**Parameters**:
- `apkDebug` - Create debug APK (default: auto-detected)
- `apkSign` - Sign the APK (default: true for release)

## Device Management Goals

### android:deploy
**Description**: Installs the APK to connected Android devices or emulators.

**Parameters**:
- `deploySkip` - Skip deployment (default: false)
- `device` - Target specific device

**Example**:
```bash
# Deploy to all connected devices
mvn android:deploy

# Deploy to specific device
mvn android:deploy -Dandroid.device=emulator-5554
```

### android:undeploy
**Description**: Uninstalls the application from connected devices.

**Example**:
```bash
mvn android:undeploy
```

### android:run
**Description**: Starts the application on the device/emulator.

**Example**:
```bash
mvn android:run
```

## File Management Goals

### android:pull
**Description**: Pulls files from the device to local filesystem.

**Parameters**:
- `pullSource` - Source path on device
- `pullDestination` - Destination path on local filesystem

**Example**:
```xml
<configuration>
    <pull>
        <source>/sdcard/myfile.txt</source>
        <destination>target/pulled-files/</destination>
    </pull>
</configuration>
```

### android:push
**Description**: Pushes files from local filesystem to the device.

**Parameters**:
- `pushSource` - Source path on local filesystem
- `pushDestination` - Destination path on device

**Example**:
```xml
<configuration>
    <push>
        <source>src/test/resources/testdata.json</source>
        <destination>/sdcard/testdata.json</destination>
    </push>
</configuration>
```

## Testing Goals

### android:instrument
**Description**: Runs instrumentation tests on the device.

**Parameters**:
- `testSkip` - Skip tests (default: false)
- `testFailSafe` - Don't fail build on test failures
- `testCreateReport` - Create test report (default: true)

**Example**:
```xml
<configuration>
    <test>
        <skip>false</skip>
        <createReport>true</createReport>
        <packages>
            <package>com.example.test</package>
        </packages>
    </test>
</configuration>
```

### android:monkey
**Description**: Runs Android Monkey testing tool for UI stress testing.

**Parameters**:
- `monkeyEventCount` - Number of events to generate
- `monkeyPackages` - Packages to test

**Example**:
```xml
<configuration>
    <monkey>
        <eventCount>1000</eventCount>
        <packages>
            <package>com.example.myapp</package>
        </packages>
    </monkey>
</configuration>
```

### android:uiautomator
**Description**: Runs UI Automator tests.

**Parameters**:
- `uiautomatorJar` - JAR file containing tests
- `uiautomatorClasses` - Test classes to run

**Example**:
```xml
<configuration>
    <uiautomator>
        <jar>target/ui-tests.jar</jar>
        <classes>
            <class>com.example.UiTest</class>
        </classes>
    </uiautomator>
</configuration>
```

## Code Quality Goals

### android:lint
**Description**: Runs Android Lint static analysis tool.

**Parameters**:
- `lintSkip` - Skip lint analysis (default: false)
- `lintFailOnError` - Fail build on lint errors (default: true)
- `lintConfig` - Custom lint configuration file

**Example**:
```xml
<configuration>
    <lint>
        <skip>false</skip>
        <failOnError>true</failOnError>
        <warningsAsErrors>true</warningsAsErrors>
        <config>lint.xml</config>
    </lint>
</configuration>
```

### android:emma
**Description**: Generates code coverage reports using EMMA.

**Parameters**:
- `emmaEnable` - Enable EMMA coverage (default: false)
- `emmaFilter` - Coverage filter

**Example**:
```xml
<configuration>
    <emma>
        <enable>true</enable>
        <filter>com.example.*</filter>
    </emma>
</configuration>
```

## NDK Goals

### android:ndk-build
**Description**: Compiles native code using Android NDK.

**Parameters**:
- `ndkBuildNdkDirectory` - NDK installation directory
- `ndkBuildExecutable` - ndk-build executable name

**Example**:
```xml
<configuration>
    <ndk>
        <path>/opt/android-ndk</path>
    </ndk>
    <ndkBuildAdditionalCommandline>NDK_DEBUG=1</ndkBuildAdditionalCommandline>
</configuration>
```

## Build Optimization Goals

### android:proguard
**Description**: Obfuscates and optimizes code using ProGuard.

**Parameters**:
- `proguardSkip` - Skip ProGuard processing (default: false)
- `proguardConfig` - ProGuard configuration file

**Example**:
```xml
<configuration>
    <proguard>
        <skip>false</skip>
        <config>proguard.cfg</config>
        <filterMavenDescriptor>true</filterMavenDescriptor>
    </proguard>
</configuration>
```

### android:zipalign
**Description**: Optimizes APK using zipalign tool.

**Parameters**:
- `zipalignSkip` - Skip zipalign (default: true)
- `zipalignVerbose` - Verbose output (default: false)

**Example**:
```xml
<configuration>
    <zipalign>
        <skip>false</skip>
        <verbose>true</verbose>
        <inputApk>target/my-app.apk</inputApk>
        <outputApk>target/my-app-aligned.apk</outputApk>
    </zipalign>
</configuration>
```

## Emulator Goals

### android:emulator-start
**Description**: Starts an Android emulator.

**Parameters**:
- `emulatorAvd` - AVD name to start
- `emulatorWait` - Wait for emulator to start (default: true)

**Example**:
```xml
<configuration>
    <emulator>
        <avd>test-avd</avd>
        <wait>true</wait>
        <options>-no-audio -no-window</options>
    </emulator>
</configuration>
```

### android:emulator-stop
**Description**: Stops a running emulator.

**Example**:
```bash
mvn android:emulator-stop
```

### android:emulator-stop-all
**Description**: Stops all running emulators.

**Example**:
```bash
mvn android:emulator-stop-all
```

## Help and Information Goals

### android:help
**Description**: Displays help information about the plugin.

**Example**:
```bash
mvn android:help
mvn android:help -Ddetail=true -Dgoal=deploy
```

### android:devices
**Description**: Lists all connected Android devices and emulators.

**Example**:
```bash
mvn android:devices
```

## Usage Examples

### Complete Build with Testing
```bash
mvn clean \
    android:generate-sources \
    compile \
    android:dex \
    android:apk \
    android:deploy \
    android:instrument
```

### Continuous Integration Build
```bash
mvn clean package \
    android:lint \
    android:emma \
    -Demulator.avd=ci-emulator \
    android:emulator-start \
    android:deploy \
    android:instrument \
    android:emulator-stop
```

### Release Build
```bash
mvn clean package \
    android:proguard \
    android:zipalign \
    -Pandroid.release
```

## Goal Execution Order

The plugin goals are typically executed in this order during a build:

1. `generate-sources` - Generate R.java and other sources
2. `compile` (Maven core) - Compile Java sources
3. `process-classes` - Process compiled classes
4. `dex` - Convert to DEX format
5. `package` - Create APK
6. `apk` - Finalize APK
7. `deploy` - Install to device (if specified)
8. `run` - Start application (if specified)

## See Also

- [Configuration Reference](configuration-reference.md) - Complete configuration options
- [Examples](examples/) - Practical usage examples
- [Advanced Topics](advanced-topics.md) - Advanced features and techniques