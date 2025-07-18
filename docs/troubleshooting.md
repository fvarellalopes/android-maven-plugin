# Troubleshooting

This guide helps you resolve common issues when using the Android Maven Plugin.

## Environment Setup Issues

### ANDROID_HOME Not Set

**Problem**: Build fails with "Android SDK not found" error.

**Solution**:
```bash
# Linux/macOS
export ANDROID_HOME=/path/to/android/sdk
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools

# Windows
set ANDROID_HOME=C:\path\to\android\sdk
set PATH=%PATH%;%ANDROID_HOME%\tools;%ANDROID_HOME%\platform-tools
```

**Verification**:
```bash
echo $ANDROID_HOME
adb version
android list targets
```

### Build Tools Not Found

**Problem**: Error message about missing build tools.

**Solution**:
1. Install build tools via SDK Manager:
   ```bash
   sdkmanager "build-tools;28.0.3"
   ```

2. Update pom.xml with correct version:
   ```xml
   <properties>
       <android.sdk.buildtools>28.0.3</android.sdk.buildtools>
   </properties>
   ```

### Platform Not Found

**Problem**: "Target 'android-XX' is not known" error.

**Solution**:
1. List available platforms:
   ```bash
   android list targets
   ```

2. Install missing platform:
   ```bash
   sdkmanager "platforms;android-28"
   ```

3. Update pom.xml:
   ```xml
   <sdk>
       <platform>28</platform>
   </sdk>
   ```

## Build Issues

### OutOfMemoryError During DEX

**Problem**: `java.lang.OutOfMemoryError` during DEX compilation.

**Solutions**:

1. **Increase DEX heap size**:
   ```xml
   <dex>
       <jvmArguments>-Xmx2048m</jvmArguments>
   </dex>
   ```

2. **Enable pre-DEX for libraries**:
   ```xml
   <dex>
       <preDex>true</preDex>
       <predexLibraries>true</predexLibraries>
   </dex>
   ```

3. **Use ProGuard to reduce method count**:
   ```xml
   <proguard>
       <skip>false</skip>
       <config>proguard.cfg</config>
   </proguard>
   ```

### 65K Method Limit

**Problem**: "method ID not in [0, 0xffff]: 65536" error.

**Solutions**:

1. **Enable multidex**:
   ```xml
   <dependencies>
       <dependency>
           <groupId>androidx.multidex</groupId>
           <artifactId>multidex</artifactId>
           <version>2.0.1</version>
           <type>aar</type>
       </dependency>
   </dependencies>
   ```

2. **Update AndroidManifest.xml**:
   ```xml
   <application
       android:name="androidx.multidex.MultiDexApplication"
       ...>
   ```

3. **Use ProGuard to reduce methods**:
   ```xml
   <proguard>
       <skip>false</skip>
       <config>proguard.cfg</config>
   </proguard>
   ```

### Resource Merge Conflicts

**Problem**: "Resource merge conflicts" during build.

**Solutions**:

1. **Check for duplicate resources**:
   ```bash
   find src/main/android/res -name "*.xml" -exec grep -l "duplicate_name" {} \;
   ```

2. **Use resource overlays**:
   ```xml
   <configuration>
       <resourceOverlayDirectories>
           <resourceOverlayDirectory>src/overlay/res</resourceOverlayDirectory>
       </resourceOverlayDirectories>
   </configuration>
   ```

3. **Exclude conflicting resources from dependencies**:
   ```xml
   <dependency>
       <groupId>com.example</groupId>
       <artifactId>library</artifactId>
       <version>1.0</version>
       <type>aar</type>
       <exclusions>
           <exclusion>
               <groupId>*</groupId>
               <artifactId>*</artifactId>
           </exclusion>
       </exclusions>
   </dependency>
   ```

## Device and Deployment Issues

### No Devices Found

**Problem**: "No devices found" when trying to deploy.

**Solutions**:

1. **Check device connectivity**:
   ```bash
   adb devices
   ```

2. **Restart ADB server**:
   ```bash
   adb kill-server
   adb start-server
   ```

3. **Enable USB debugging** on device (Settings > Developer Options > USB Debugging)

4. **Check device permissions** (allow computer access when prompted)

5. **Specify device explicitly**:
   ```bash
   mvn android:deploy -Dandroid.device=emulator-5554
   ```

### Deployment Fails

**Problem**: APK installation fails on device.

**Solutions**:

1. **Check available space** on device

2. **Uninstall existing app**:
   ```bash
   mvn android:undeploy
   # or manually
   adb uninstall com.example.myapp
   ```

3. **Check app permissions** in AndroidManifest.xml

4. **Verify APK signing**:
   ```bash
   jarsigner -verify -verbose target/my-app.apk
   ```

5. **Enable undeploy before deploy**:
   ```xml
   <configuration>
       <undeployBeforeDeploy>true</undeployBeforeDeploy>
   </configuration>
   ```

### Emulator Issues

**Problem**: Emulator won't start or is slow.

**Solutions**:

1. **Check available AVDs**:
   ```bash
   android list avd
   ```

2. **Create new AVD**:
   ```bash
   android create avd -n test-avd -t android-28
   ```

3. **Start emulator with specific options**:
   ```xml
   <emulator>
       <avd>test-avd</avd>
       <options>-no-audio -no-window -gpu off</options>
       <wait>true</wait>
   </emulator>
   ```

4. **Increase emulator memory**:
   ```bash
   emulator -avd test-avd -memory 2048
   ```

## Testing Issues

### Instrumentation Tests Fail

**Problem**: Instrumentation tests don't run or fail unexpectedly.

**Solutions**:

1. **Check test APK is built**:
   ```bash
   ls -la target/*test*.apk
   ```

2. **Verify test runner configuration**:
   ```xml
   <test>
       <createReport>true</createReport>
       <singleInstrumentationCall>true</singleInstrumentationCall>
   </test>
   ```

3. **Check AndroidManifest.xml in test project**:
   ```xml
   <instrumentation
       android:name="androidx.test.runner.AndroidJUnitRunner"
       android:targetPackage="com.example.myapp"/>
   ```

4. **Ensure test dependencies are correct**:
   ```xml
   <dependency>
       <groupId>androidx.test.ext</groupId>
       <artifactId>junit</artifactId>
       <version>1.1.2</version>
       <scope>provided</scope>
   </dependency>
   ```

### Coverage Reports Empty

**Problem**: EMMA coverage reports show no coverage data.

**Solutions**:

1. **Enable coverage in configuration**:
   ```xml
   <test>
       <coverage>true</coverage>
       <coverageFile>coverage.ec</coverageFile>
   </test>
   <emma>
       <enable>true</enable>
   </emma>
   ```

2. **Check instrumented APK is used**:
   ```bash
   # Look for emma.jar in APK
   unzip -l target/my-app.apk | grep emma
   ```

3. **Pull coverage file from device**:
   ```bash
   adb pull /sdcard/coverage.ec target/
   ```

## Dependency Issues

### AAR Dependencies Not Found

**Problem**: AAR dependencies cannot be resolved.

**Solutions**:

1. **Check repository configuration**:
   ```xml
   <repositories>
       <repository>
           <id>google</id>
           <url>https://maven.google.com</url>
       </repository>
   </repositories>
   ```

2. **Use correct dependency type**:
   ```xml
   <dependency>
       <groupId>androidx.appcompat</groupId>
       <artifactId>appcompat</artifactId>
       <version>1.1.0</version>
       <type>aar</type>
   </dependency>
   ```

3. **Enable AAR support**:
   ```xml
   <configuration>
       <includeLibsJarsFromAar>true</includeLibsJarsFromAar>
   </configuration>
   ```

### Dependency Conflicts

**Problem**: "Class already added" or version conflicts.

**Solutions**:

1. **Analyze dependency tree**:
   ```bash
   mvn dependency:tree -Dverbose
   ```

2. **Exclude conflicting dependencies**:
   ```xml
   <dependency>
       <groupId>com.example</groupId>
       <artifactId>library</artifactId>
       <version>1.0</version>
       <exclusions>
           <exclusion>
               <groupId>conflicting.group</groupId>
               <artifactId>conflicting-artifact</artifactId>
           </exclusion>
       </exclusions>
   </dependency>
   ```

3. **Force specific versions**:
   ```xml
   <dependencyManagement>
       <dependencies>
           <dependency>
               <groupId>com.example</groupId>
               <artifactId>common-lib</artifactId>
               <version>2.0.0</version>
           </dependency>
       </dependencies>
   </dependencyManagement>
   ```

## Lint Issues

### Lint Failures

**Problem**: Build fails due to lint errors.

**Solutions**:

1. **Check lint report**:
   ```bash
   cat target/lint-results.xml
   ```

2. **Configure lint to ignore specific issues**:
   ```xml
   <lint>
       <config>lint.xml</config>
       <warningsAsErrors>false</warningsAsErrors>
   </lint>
   ```

3. **Create lint.xml configuration**:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <lint>
       <issue id="IconMissingDensityFolder" severity="ignore"/>
       <issue id="GoogleAppIndexingWarning" severity="ignore"/>
   </lint>
   ```

4. **Disable lint for development builds**:
   ```xml
   <profiles>
       <profile>
           <id>dev</id>
           <build>
               <plugins>
                   <plugin>
                       <groupId>com.simpligility.maven.plugins</groupId>
                       <artifactId>android-maven-plugin</artifactId>
                       <configuration>
                           <lint>
                               <skip>true</skip>
                           </lint>
                       </configuration>
                   </plugin>
               </plugins>
           </build>
       </profile>
   </profiles>
   ```

## ProGuard Issues

### ProGuard Removes Required Classes

**Problem**: App crashes after ProGuard with "ClassNotFoundException".

**Solutions**:

1. **Add keep rules**:
   ```proguard
   -keep class com.example.myapp.** { *; }
   -keep interface com.example.myapp.** { *; }
   ```

2. **Keep classes used by reflection**:
   ```proguard
   -keepclassmembers class * {
       @com.google.gson.annotations.SerializedName <fields>;
   }
   ```

3. **Enable verbose ProGuard output**:
   ```xml
   <proguard>
       <options>
           <option>-verbose</option>
           <option>-printmapping target/proguard-mapping.txt</option>
       </options>
   </proguard>
   ```

### ProGuard OutOfMemoryError

**Problem**: ProGuard fails with memory errors.

**Solutions**:

1. **Increase ProGuard heap size**:
   ```xml
   <proguard>
       <jvmArguments>-Xmx2048m</jvmArguments>
   </proguard>
   ```

2. **Split ProGuard configuration**:
   ```xml
   <proguard>
       <configs>
           <config>proguard-base.cfg</config>
           <config>proguard-app.cfg</config>
       </configs>
   </proguard>
   ```

## Performance Issues

### Slow Build Times

**Problem**: Builds take too long to complete.

**Solutions**:

1. **Enable parallel builds**:
   ```bash
   mvn -T 4 clean package
   ```

2. **Use build cache**:
   ```xml
   <configuration>
       <dex>
           <preDex>true</preDex>
           <predexLibraries>true</predexLibraries>
       </dex>
   </configuration>
   ```

3. **Skip unnecessary goals in development**:
   ```bash
   mvn package -Dandroid.lint.skip=true -DskipTests=true
   ```

4. **Configure Maven memory settings** (.mvn/jvm.config):
   ```
   -Xmx4g
   -XX:ReservedCodeCacheSize=512m
   ```

### Large APK Size

**Problem**: Generated APK is too large.

**Solutions**:

1. **Enable ProGuard**:
   ```xml
   <proguard>
       <skip>false</skip>
       <config>proguard.cfg</config>
   </proguard>
   ```

2. **Use APK splits**:
   ```xml
   <configuration>
       <apkSplits>
           <density>true</density>
           <abi>true</abi>
       </apkSplits>
   </configuration>
   ```

3. **Remove unused resources**:
   ```proguard
   -dontshrink
   -dontoptimize
   ```

4. **Check APK contents**:
   ```bash
   unzip -l target/my-app.apk | sort -k4 -nr
   ```

## Maven-Specific Issues

### Plugin Version Conflicts

**Problem**: Multiple versions of the plugin in the build.

**Solutions**:

1. **Use pluginManagement**:
   ```xml
   <build>
       <pluginManagement>
           <plugins>
               <plugin>
                   <groupId>com.simpligility.maven.plugins</groupId>
                   <artifactId>android-maven-plugin</artifactId>
                   <version>4.6.1</version>
               </plugin>
           </plugins>
       </pluginManagement>
   </build>
   ```

2. **Check effective POM**:
   ```bash
   mvn help:effective-pom
   ```

### Settings.xml Configuration

**Problem**: Corporate proxy or repository issues.

**Solutions**:

1. **Configure proxy in ~/.m2/settings.xml**:
   ```xml
   <settings>
       <proxies>
           <proxy>
               <host>proxy.company.com</host>
               <port>8080</port>
           </proxy>
       </proxies>
   </settings>
   ```

2. **Add corporate repositories**:
   ```xml
   <repositories>
       <repository>
           <id>corporate</id>
           <url>http://nexus.company.com/repository/public</url>
       </repository>
   </repositories>
   ```

## Debugging Build Issues

### Enable Debug Output

```bash
# Debug Maven build
mvn clean package -X

# Debug Android plugin specifically
mvn android:help -Ddetail=true -Dgoal=deploy

# Debug with specific properties
mvn package -Dandroid.enableLogLevel=DEBUG
```

### Common Debug Commands

```bash
# Check plugin configuration
mvn help:describe -Dplugin=android-maven-plugin -Ddetail

# Analyze dependencies
mvn dependency:tree
mvn dependency:analyze

# Check effective POM
mvn help:effective-pom

# Validate POM
mvn validate

# Check system properties
mvn help:system
```

### Log Analysis

Look for these patterns in build logs:

1. **Memory issues**: "OutOfMemoryError", "Java heap space"
2. **Missing files**: "FileNotFoundException", "No such file"
3. **Permission issues**: "Permission denied", "Access denied"
4. **Version conflicts**: "version conflict", "duplicate class"

## Getting Help

### Community Resources

1. **GitHub Issues**: https://github.com/simpligility/android-maven-plugin/issues
2. **Mailing List**: maven-android-developers@googlegroups.com
3. **Stack Overflow**: Tag questions with `android-maven-plugin`

### Reporting Issues

When reporting issues, include:

1. **Plugin version**: Check your pom.xml
2. **Android SDK version**: `android list targets`
3. **Java version**: `java -version`
4. **Maven version**: `mvn -version`
5. **Operating system**: Windows, macOS, Linux
6. **Full error message**: Complete stack trace
7. **Minimal reproduction case**: Simple project that demonstrates the issue

### Debug Information Script

```bash
#!/bin/bash
echo "=== Environment Information ==="
echo "Java Version:"
java -version
echo ""
echo "Maven Version:"
mvn -version
echo ""
echo "Android SDK:"
echo "ANDROID_HOME: $ANDROID_HOME"
android list targets
echo ""
echo "Connected Devices:"
adb devices
echo ""
echo "Plugin Version:"
mvn help:describe -Dplugin=android-maven-plugin | grep Version
```

Run this script and include the output when reporting issues.

## See Also

- [Configuration Reference](configuration-reference.md) - Complete configuration options
- [Goals Reference](goals-reference.md) - Detailed goal documentation
- [Advanced Topics](advanced-topics.md) - Advanced features and techniques
- [Examples](examples/) - Working examples and use cases