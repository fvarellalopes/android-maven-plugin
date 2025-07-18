# Configuration Reference

This document provides a comprehensive reference for all configuration options available in the Android Maven Plugin.

## Plugin Configuration Structure

```xml
<plugin>
    <groupId>com.simpligility.maven.plugins</groupId>
    <artifactId>android-maven-plugin</artifactId>
    <version>4.6.1</version>
    <extensions>true</extensions>
    <configuration>
        <!-- Configuration options go here -->
    </configuration>
</plugin>
```

## SDK Configuration

### Basic SDK Setup
```xml
<configuration>
    <sdk>
        <platform>28</platform>
        <path>/path/to/android/sdk</path>
        <buildTools>28.0.3</buildTools>
    </sdk>
</configuration>
```

### SDK Parameters
- **platform** (Integer): Android API level to target
- **path** (String): Path to Android SDK (uses `ANDROID_HOME` if not specified)
- **buildTools** (String): Build tools version to use

## Device Configuration

### Device Selection
```xml
<configuration>
    <device>emulator-5554</device>
    <!-- OR -->
    <devices>
        <device>emulator-5554</device>
        <device>192.168.1.100:5555</device>
    </devices>
</configuration>
```

### Device Parameters
- **device** (String): Single device identifier
- **devices** (List): Multiple device identifiers
- **deviceThreads** (Integer): Number of parallel threads for multi-device operations

## APK Configuration

### Basic APK Settings
```xml
<configuration>
    <apk>
        <debug>true</debug>
        <nativeToolchain>arm</nativeToolchain>
        <applicationMakefile>Application.mk</applicationMakefile>
        <attachNativeArtifacts>true</attachNativeArtifacts>
    </apk>
</configuration>
```

### APK Parameters
- **debug** (Boolean): Create debug APK (default: auto-detected from packaging)
- **nativeToolchain** (String): Native toolchain for NDK builds
- **applicationMakefile** (String): Path to Application.mk file
- **attachNativeArtifacts** (Boolean): Attach native libraries as Maven artifacts

## DEX Configuration

### DEX Processing Options
```xml
<configuration>
    <dex>
        <jvmArguments>-Xmx1024m</jvmArguments>
        <coreLibrary>false</coreLibrary>
        <noLocals>false</noLocals>
        <optimize>true</optimize>
        <preDex>true</preDex>
        <predexLibraries>true</predexLibraries>
    </dex>
</configuration>
```

### DEX Parameters
- **jvmArguments** (String): JVM arguments for DEX process
- **coreLibrary** (Boolean): Include core library in DEX
- **noLocals** (Boolean): Optimize out local variable information
- **optimize** (Boolean): Enable DEX optimization
- **preDex** (Boolean): Enable pre-DEX processing
- **predexLibraries** (Boolean): Pre-DEX library dependencies

## ProGuard Configuration

### ProGuard Setup
```xml
<configuration>
    <proguard>
        <skip>false</skip>
        <config>proguard.cfg</config>
        <configs>
            <config>proguard-base.cfg</config>
            <config>proguard-custom.cfg</config>
        </configs>
        <filterMavenDescriptor>true</filterMavenDescriptor>
        <filterManifest>true</filterManifest>
    </proguard>
</configuration>
```

### ProGuard Parameters
- **skip** (Boolean): Skip ProGuard processing
- **config** (String): Single ProGuard configuration file
- **configs** (List): Multiple ProGuard configuration files
- **filterMavenDescriptor** (Boolean): Filter Maven descriptor from JAR
- **filterManifest** (Boolean): Filter manifest from JAR

## NDK Configuration

### NDK Build Settings
```xml
<configuration>
    <ndk>
        <path>/opt/android-ndk</path>
        <moduleName>mymodule</moduleName>
    </ndk>
    <ndkBuildAdditionalCommandline>NDK_DEBUG=1 V=1</ndkBuildAdditionalCommandline>
    <ndkBuildExecutable>ndk-build</ndkBuildExecutable>
    <ndkBuildNdkDirectory>/opt/android-ndk</ndkBuildNdkDirectory>
</configuration>
```

### NDK Parameters
- **path** (String): Path to NDK installation
- **moduleName** (String): Name of the native module
- **ndkBuildAdditionalCommandline** (String): Additional command line arguments
- **ndkBuildExecutable** (String): ndk-build executable name
- **ndkBuildNdkDirectory** (String): NDK directory (deprecated, use path)

## Testing Configuration

### Instrumentation Testing
```xml
<configuration>
    <test>
        <skip>false</skip>
        <createReport>true</createReport>
        <singleInstrumentationCall>true</singleInstrumentationCall>
        <testFailSafe>false</testFailSafe>
        <logOnly>false</logOnly>
        <packages>
            <package>com.example.test</package>
        </packages>
        <classes>
            <class>com.example.test.MyTest</class>
        </classes>
        <coverage>true</coverage>
        <coverageFile>coverage.ec</coverageFile>
    </test>
</configuration>
```

### Testing Parameters
- **skip** (Boolean): Skip test execution
- **createReport** (Boolean): Create test report
- **singleInstrumentationCall** (Boolean): Use single instrumentation call
- **testFailSafe** (Boolean): Don't fail build on test failures
- **logOnly** (Boolean): Only log test results, don't parse
- **packages** (List): Test packages to run
- **classes** (List): Specific test classes to run
- **coverage** (Boolean): Enable code coverage
- **coverageFile** (String): Coverage output file

### Monkey Testing
```xml
<configuration>
    <monkey>
        <eventCount>1000</eventCount>
        <seed>42</seed>
        <throttle>100</throttle>
        <percentTouch>80</percentTouch>
        <percentMotion>15</percentMotion>
        <percentNav>5</percentNav>
        <packages>
            <package>com.example.myapp</package>
        </packages>
    </monkey>
</configuration>
```

### Monkey Parameters
- **eventCount** (Integer): Number of events to generate
- **seed** (Long): Random seed for reproducible tests
- **throttle** (Integer): Delay between events (milliseconds)
- **percentTouch** (Integer): Percentage of touch events
- **percentMotion** (Integer): Percentage of motion events
- **percentNav** (Integer): Percentage of navigation events
- **packages** (List): Packages to test

## Lint Configuration

### Lint Analysis Setup
```xml
<configuration>
    <lint>
        <skip>false</skip>
        <failOnError>true</failOnError>
        <warningsAsErrors>false</warningsAsErrors>
        <config>lint.xml</config>
        <fullPath>true</fullPath>
        <showAll>true</showAll>
        <disableSourceLines>false</disableSourceLines>
        <url>file://lint-results.html</url>
        <enableClasspath>true</enableClasspath>
        <enableLibraries>true</enableLibraries>
        <sources>
            <source>src/main/java</source>
        </sources>
        <classpath>
            <path>target/classes</path>
        </classpath>
    </lint>
</configuration>
```

### Lint Parameters
- **skip** (Boolean): Skip lint analysis
- **failOnError** (Boolean): Fail build on lint errors
- **warningsAsErrors** (Boolean): Treat warnings as errors
- **config** (String): Lint configuration file
- **fullPath** (Boolean): Use full paths in reports
- **showAll** (Boolean): Show all issues
- **disableSourceLines** (Boolean): Don't include source lines in output
- **url** (String): URL for HTML report
- **enableClasspath** (Boolean): Enable classpath analysis
- **enableLibraries** (Boolean): Enable library analysis
- **sources** (List): Source directories to analyze
- **classpath** (List): Classpath entries

## Emulator Configuration

### Emulator Management
```xml
<configuration>
    <emulator>
        <avd>test-avd</avd>
        <wait>true</wait>
        <options>-no-audio -no-window -gpu off</options>
        <executable>emulator</executable>
    </emulator>
</configuration>
```

### Emulator Parameters
- **avd** (String): Android Virtual Device name
- **wait** (Boolean): Wait for emulator to start
- **options** (String): Additional emulator options
- **executable** (String): Emulator executable name

## File Operations Configuration

### Pull Configuration
```xml
<configuration>
    <pull>
        <source>/sdcard/myfile.txt</source>
        <destination>target/pulled-files/</destination>
    </pull>
</configuration>
```

### Push Configuration
```xml
<configuration>
    <push>
        <source>src/test/resources/testdata.json</source>
        <destination>/sdcard/testdata.json</destination>
    </push>
</configuration>
```

### File Operation Parameters
- **source** (String): Source file or directory
- **destination** (String): Destination file or directory

## Zipalign Configuration

### APK Optimization
```xml
<configuration>
    <zipalign>
        <skip>false</skip>
        <verbose>true</verbose>
        <inputApk>target/${project.finalName}.apk</inputApk>
        <outputApk>target/${project.finalName}-aligned.apk</outputApk>
        <classifier>aligned</classifier>
    </zipalign>
</configuration>
```

### Zipalign Parameters
- **skip** (Boolean): Skip zipalign processing
- **verbose** (Boolean): Verbose output
- **inputApk** (String): Input APK file
- **outputApk** (String): Output APK file
- **classifier** (String): Classifier for aligned APK

## EMMA Coverage Configuration

### Code Coverage Setup
```xml
<configuration>
    <emma>
        <enable>true</enable>
        <classpath>target/classes</classpath>
        <filter>com.example.*</filter>
        <outputDirectory>target/emma</outputDirectory>
    </emma>
</configuration>
```

### EMMA Parameters
- **enable** (Boolean): Enable EMMA coverage
- **classpath** (String): Classpath for coverage
- **filter** (String): Package filter for coverage
- **outputDirectory** (String): Output directory for reports

## Signing Configuration

### APK Signing
```xml
<configuration>
    <sign>
        <debug>auto</debug>
        <keystore>path/to/keystore</keystore>
        <storepass>storepass</storepass>
        <keypass>keypass</keypass>
        <alias>mykey</alias>
        <verbose>true</verbose>
        <verify>true</verify>
    </sign>
</configuration>
```

### Signing Parameters
- **debug** (String): Debug signing mode (auto/true/false)
- **keystore** (String): Path to keystore file
- **storepass** (String): Keystore password
- **keypass** (String): Key password
- **alias** (String): Key alias
- **verbose** (Boolean): Verbose signing output
- **verify** (Boolean): Verify signature after signing

## Resource Configuration

### Resource Processing
```xml
<configuration>
    <resourceDirectory>src/main/android/res</resourceDirectory>
    <resourceOverlayDirectory>src/main/android/res-overlay</resourceOverlayDirectory>
    <resourceOverlayDirectories>
        <resourceOverlayDirectory>src/overlay1/res</resourceOverlayDirectory>
        <resourceOverlayDirectory>src/overlay2/res</resourceOverlayDirectory>
    </resourceOverlayDirectories>
    <assetsDirectory>src/main/android/assets</assetsDirectory>
    <manifestFile>AndroidManifest.xml</manifestFile>
</configuration>
```

### Resource Parameters
- **resourceDirectory** (String): Main resource directory
- **resourceOverlayDirectory** (String): Resource overlay directory
- **resourceOverlayDirectories** (List): Multiple overlay directories
- **assetsDirectory** (String): Assets directory
- **manifestFile** (String): Android manifest file

## Build Configuration

### General Build Settings
```xml
<configuration>
    <includeLibsJarsFromApklib>true</includeLibsJarsFromApklib>
    <includeLibsJarsFromAar>true</includeLibsJarsFromAar>
    <extractDuplicates>false</extractDuplicates>
    <undeployBeforeDeploy>false</undeployBeforeDeploy>
</configuration>
```

### Build Parameters
- **includeLibsJarsFromApklib** (Boolean): Include JARs from APKLIB dependencies
- **includeLibsJarsFromAar** (Boolean): Include JARs from AAR dependencies
- **extractDuplicates** (Boolean): Extract duplicate resources
- **undeployBeforeDeploy** (Boolean): Undeploy before deploying

## Example: Complete Configuration

```xml
<plugin>
    <groupId>com.simpligility.maven.plugins</groupId>
    <artifactId>android-maven-plugin</artifactId>
    <version>4.6.1</version>
    <extensions>true</extensions>
    <configuration>
        <sdk>
            <platform>28</platform>
            <buildTools>28.0.3</buildTools>
        </sdk>
        
        <dex>
            <jvmArguments>-Xmx1024m</jvmArguments>
        </dex>
        
        <proguard>
            <skip>false</skip>
            <config>proguard.cfg</config>
        </proguard>
        
        <test>
            <createReport>true</createReport>
            <coverage>true</coverage>
        </test>
        
        <lint>
            <failOnError>true</failOnError>
            <warningsAsErrors>true</warningsAsErrors>
        </lint>
        
        <zipalign>
            <skip>false</skip>
        </zipalign>
    </configuration>
</plugin>
```

## Property-Based Configuration

Most configuration options can also be set via Maven properties or command line:

```bash
# Via command line
mvn android:deploy -Dandroid.device=emulator-5554

# Via properties in pom.xml
<properties>
    <android.sdk.platform>28</android.sdk.platform>
    <android.lint.failOnError>true</android.lint.failOnError>
    <android.test.skip>false</android.test.skip>
</properties>
```

## See Also

- [Getting Started](getting-started.md) - Basic setup and usage
- [Goals Reference](goals-reference.md) - Detailed goal documentation
- [Examples](examples/) - Practical configuration examples