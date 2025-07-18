# NDK Build Example

This example demonstrates how to build native code using the Android NDK with the Android Maven Plugin.

## Project Structure

```
ndk-build/
├── pom.xml
├── AndroidManifest.xml
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/ndkapp/
│   │   │       └── MainActivity.java
│   │   ├── android/
│   │   │   └── res/
│   │   └── jni/
│   │       ├── Android.mk
│   │       ├── Application.mk
│   │       ├── native-lib.cpp
│   │       └── com_example_ndkapp_NativeLib.h
│   └── test/
└── target/
```

## pom.xml Configuration

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>ndk-build-example</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>apk</packaging>
    
    <name>NDK Build Example</name>
    <description>Android app with native code using NDK</description>
    
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        
        <!-- Android Properties -->
        <android.version>4.1.1.4</android.version>
        <android.platform>28</android.platform>
        <android-maven-plugin.version>4.6.1</android-maven-plugin.version>
        
        <!-- NDK Properties -->
        <android.ndk.path>${env.ANDROID_NDK_HOME}</android.ndk.path>
    </properties>
    
    <dependencies>
        <!-- Android API -->
        <dependency>
            <groupId>com.google.android</groupId>
            <artifactId>android</artifactId>
            <version>${android.version}</version>
            <scope>provided</scope>
        </dependency>
        
        <!-- Support Library -->
        <dependency>
            <groupId>androidx.appcompat</groupId>
            <artifactId>appcompat</artifactId>
            <version>1.3.1</version>
            <type>aar</type>
        </dependency>
    </dependencies>
    
    <build>
        <finalName>${project.artifactId}</finalName>
        <sourceDirectory>src</sourceDirectory>
        
        <plugins>
            <!-- Android Maven Plugin -->
            <plugin>
                <groupId>com.simpligility.maven.plugins</groupId>
                <artifactId>android-maven-plugin</artifactId>
                <version>${android-maven-plugin.version}</version>
                <extensions>true</extensions>
                <configuration>
                    <sdk>
                        <platform>${android.platform}</platform>
                    </sdk>
                    
                    <!-- NDK Configuration -->
                    <ndk>
                        <path>${android.ndk.path}</path>
                    </ndk>
                    
                    <!-- NDK Build Configuration -->
                    <ndkBuildExecutable>ndk-build</ndkBuildExecutable>
                    <ndkBuildAdditionalCommandline>V=1 NDK_DEBUG=1</ndkBuildAdditionalCommandline>
                    
                    <!-- Native Libraries -->
                    <attachNativeArtifacts>true</attachNativeArtifacts>
                    <nativeToolchain>arm-linux-androideabi-4.9</nativeToolchain>
                </configuration>
                
                <executions>
                    <!-- NDK Build Execution -->
                    <execution>
                        <id>ndk-build</id>
                        <phase>process-classes</phase>
                        <goals>
                            <goal>ndk-build</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            
            <!-- Maven Compiler Plugin -->
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
    
    <!-- Build Profiles for Different Architectures -->
    <profiles>
        <profile>
            <id>arm</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>com.simpligility.maven.plugins</groupId>
                        <artifactId>android-maven-plugin</artifactId>
                        <configuration>
                            <ndkBuildAdditionalCommandline>
                                APP_ABI=armeabi-v7a V=1
                            </ndkBuildAdditionalCommandline>
                        </configuration>
                    </plugin>
                </plugins>
            </build>
        </profile>
        
        <profile>
            <id>arm64</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>com.simpligility.maven.plugins</groupId>
                        <artifactId>android-maven-plugin</artifactId>
                        <configuration>
                            <ndkBuildAdditionalCommandline>
                                APP_ABI=arm64-v8a V=1
                            </ndkBuildAdditionalCommandline>
                        </configuration>
                    </plugin>
                </plugins>
            </build>
        </profile>
        
        <profile>
            <id>x86</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>com.simpligility.maven.plugins</groupId>
                        <artifactId>android-maven-plugin</artifactId>
                        <configuration>
                            <ndkBuildAdditionalCommandline>
                                APP_ABI=x86 V=1
                            </ndkBuildAdditionalCommandline>
                        </configuration>
                    </plugin>
                </plugins>
            </build>
        </profile>
        
        <profile>
            <id>all-arch</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>com.simpligility.maven.plugins</groupId>
                        <artifactId>android-maven-plugin</artifactId>
                        <configuration>
                            <ndkBuildAdditionalCommandline>
                                APP_ABI=all V=1
                            </ndkBuildAdditionalCommandline>
                        </configuration>
                    </plugin>
                </plugins>
            </build>
        </profile>
    </profiles>
</project>
```

## Native Code Files

### Android.mk
```makefile
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)

LOCAL_MODULE    := native-lib
LOCAL_SRC_FILES := native-lib.cpp
LOCAL_LDLIBS    := -llog

include $(BUILD_SHARED_LIBRARY)
```

### Application.mk
```makefile
APP_ABI := armeabi-v7a arm64-v8a x86 x86_64
APP_PLATFORM := android-16
APP_STL := c++_static
APP_CPPFLAGS := -frtti -fexceptions
APP_OPTIM := release
```

### native-lib.cpp
```cpp
#include <jni.h>
#include <string>
#include <android/log.h>

#define LOG_TAG "NativeLib"
#define LOGI(...) __android_log_print(ANDROID_LOG_INFO, LOG_TAG, __VA_ARGS__)
#define LOGE(...) __android_log_print(ANDROID_LOG_ERROR, LOG_TAG, __VA_ARGS__)

extern "C" {

JNIEXPORT jstring JNICALL
Java_com_example_ndkapp_NativeLib_getStringFromJNI(JNIEnv *env, jobject thiz) {
    std::string hello = "Hello from C++";
    LOGI("getStringFromJNI called");
    return env->NewStringUTF(hello.c_str());
}

JNIEXPORT jint JNICALL
Java_com_example_ndkapp_NativeLib_addNumbers(JNIEnv *env, jobject thiz, jint a, jint b) {
    jint result = a + b;
    LOGI("addNumbers: %d + %d = %d", a, b, result);
    return result;
}

JNIEXPORT jintArray JNICALL
Java_com_example_ndkapp_NativeLib_processArray(JNIEnv *env, jobject thiz, jintArray input) {
    jsize length = env->GetArrayLength(input);
    jint *inputElements = env->GetIntArrayElements(input, nullptr);
    
    jintArray result = env->NewIntArray(length);
    jint *resultElements = env->GetIntArrayElements(result, nullptr);
    
    // Process array - multiply each element by 2
    for (int i = 0; i < length; i++) {
        resultElements[i] = inputElements[i] * 2;
    }
    
    env->ReleaseIntArrayElements(input, inputElements, 0);
    env->ReleaseIntArrayElements(result, resultElements, 0);
    
    LOGI("processArray: processed %d elements", length);
    return result;
}

JNIEXPORT void JNICALL
Java_com_example_ndkapp_NativeLib_performHeavyComputation(JNIEnv *env, jobject thiz) {
    LOGI("Starting heavy computation...");
    
    // Simulate heavy computation
    long long sum = 0;
    for (int i = 0; i < 1000000; i++) {
        sum += i * i;
    }
    
    LOGI("Heavy computation completed. Result: %lld", sum);
}

} // extern "C"
```

### com_example_ndkapp_NativeLib.h
```c
/* DO NOT EDIT THIS FILE - it is machine generated */
#include <jni.h>
/* Header for class com_example_ndkapp_NativeLib */

#ifndef _Included_com_example_ndkapp_NativeLib
#define _Included_com_example_ndkapp_NativeLib
#ifdef __cplusplus
extern "C" {
#endif

/*
 * Class:     com_example_ndkapp_NativeLib
 * Method:    getStringFromJNI
 * Signature: ()Ljava/lang/String;
 */
JNIEXPORT jstring JNICALL Java_com_example_ndkapp_NativeLib_getStringFromJNI
  (JNIEnv *, jobject);

/*
 * Class:     com_example_ndkapp_NativeLib
 * Method:    addNumbers
 * Signature: (II)I
 */
JNIEXPORT jint JNICALL Java_com_example_ndkapp_NativeLib_addNumbers
  (JNIEnv *, jobject, jint, jint);

/*
 * Class:     com_example_ndkapp_NativeLib
 * Method:    processArray
 * Signature: ([I)[I
 */
JNIEXPORT jintArray JNICALL Java_com_example_ndkapp_NativeLib_processArray
  (JNIEnv *, jobject, jintArray);

/*
 * Class:     com_example_ndkapp_NativeLib
 * Method:    performHeavyComputation
 * Signature: ()V
 */
JNIEXPORT void JNICALL Java_com_example_ndkapp_NativeLib_performHeavyComputation
  (JNIEnv *, jobject);

#ifdef __cplusplus
}
#endif
#endif
```

## Java Code

### NativeLib.java
```java
package com.example.ndkapp;

public class NativeLib {
    
    // Load the native library
    static {
        System.loadLibrary("native-lib");
    }
    
    // Native method declarations
    public native String getStringFromJNI();
    public native int addNumbers(int a, int b);
    public native int[] processArray(int[] input);
    public native void performHeavyComputation();
}
```

### MainActivity.java
```java
package com.example.ndkapp;

import android.app.Activity;
import android.os.AsyncTask;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import java.util.Arrays;

public class MainActivity extends Activity {
    private static final String TAG = "MainActivity";
    private NativeLib nativeLib;
    private TextView resultText;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        nativeLib = new NativeLib();
        resultText = findViewById(R.id.result_text);
        
        setupButtons();
        
        // Test basic JNI call
        String jniString = nativeLib.getStringFromJNI();
        Log.i(TAG, "JNI String: " + jniString);
        resultText.setText(jniString);
    }
    
    private void setupButtons() {
        Button addButton = findViewById(R.id.add_button);
        addButton.setOnClickListener(v -> {
            int result = nativeLib.addNumbers(42, 58);
            resultText.setText("Addition result: " + result);
            Log.i(TAG, "Addition result: " + result);
        });
        
        Button arrayButton = findViewById(R.id.array_button);
        arrayButton.setOnClickListener(v -> {
            int[] input = {1, 2, 3, 4, 5};
            int[] result = nativeLib.processArray(input);
            resultText.setText("Array result: " + Arrays.toString(result));
            Log.i(TAG, "Array processing: " + Arrays.toString(input) + 
                  " -> " + Arrays.toString(result));
        });
        
        Button heavyButton = findViewById(R.id.heavy_button);
        heavyButton.setOnClickListener(v -> {
            resultText.setText("Performing heavy computation...");
            new HeavyComputationTask().execute();
        });
    }
    
    private class HeavyComputationTask extends AsyncTask<Void, Void, Void> {
        @Override
        protected Void doInBackground(Void... params) {
            nativeLib.performHeavyComputation();
            return null;
        }
        
        @Override
        protected void onPostExecute(Void result) {
            resultText.setText("Heavy computation completed!");
        }
    }
}
```

## AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.example.ndkapp"
          android:versionCode="1"
          android:versionName="1.0">
    
    <uses-sdk 
        android:minSdkVersion="16" 
        android:targetSdkVersion="28"/>
    
    <application 
        android:label="@string/app_name"
        android:theme="@style/AppTheme">
        
        <activity 
            android:name=".MainActivity"
            android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

## Prerequisites

### NDK Installation
1. **Download Android NDK**:
   ```bash
   # Via SDK Manager
   sdkmanager "ndk;21.4.7075529"
   
   # Or download directly from:
   # https://developer.android.com/ndk/downloads
   ```

2. **Set Environment Variables**:
   ```bash
   export ANDROID_NDK_HOME=/path/to/android-ndk
   export PATH=$PATH:$ANDROID_NDK_HOME
   ```

3. **Verify Installation**:
   ```bash
   ndk-build --version
   ```

## Building the Project

### Basic Build
```bash
mvn clean package
```

### Build for Specific Architecture
```bash
# ARM 32-bit
mvn clean package -P arm

# ARM 64-bit  
mvn clean package -P arm64

# x86
mvn clean package -P x86

# All architectures
mvn clean package -P all-arch
```

### Debug Build with Verbose Output
```bash
mvn clean package -X -Dandroid.ndk.debug=true
```

## Generated Artifacts

After successful build, you'll find:
- **APK**: `target/ndk-build-example.apk`
- **Native Libraries**: `target/libs/{arch}/libnative-lib.so`
  - `target/libs/armeabi-v7a/libnative-lib.so`
  - `target/libs/arm64-v8a/libnative-lib.so`
  - `target/libs/x86/libnative-lib.so`
  - `target/libs/x86_64/libnative-lib.so`

## Advanced Configuration

### Custom NDK Build Arguments
```xml
<configuration>
    <ndkBuildAdditionalCommandline>
        APP_ABI=armeabi-v7a 
        APP_OPTIM=debug 
        NDK_DEBUG=1 
        V=1
    </ndkBuildAdditionalCommandline>
</configuration>
```

### Multiple Native Modules
```makefile
# Android.mk for multiple modules
LOCAL_PATH := $(call my-dir)

# Module 1
include $(CLEAR_VARS)
LOCAL_MODULE    := module1
LOCAL_SRC_FILES := module1.cpp
include $(BUILD_SHARED_LIBRARY)

# Module 2
include $(CLEAR_VARS)
LOCAL_MODULE    := module2
LOCAL_SRC_FILES := module2.cpp
LOCAL_SHARED_LIBRARIES := module1
include $(BUILD_SHARED_LIBRARY)
```

### Using External Libraries
```makefile
# Android.mk with external library
LOCAL_PATH := $(call my-dir)

# Prebuilt library
include $(CLEAR_VARS)
LOCAL_MODULE := external-lib
LOCAL_SRC_FILES := libs/$(TARGET_ARCH_ABI)/libexternal.so
include $(PREBUILT_SHARED_LIBRARY)

# Main module
include $(CLEAR_VARS)
LOCAL_MODULE := native-lib
LOCAL_SRC_FILES := native-lib.cpp
LOCAL_SHARED_LIBRARIES := external-lib
include $(BUILD_SHARED_LIBRARY)
```

## Debugging Native Code

### GDB Debugging
```bash
# Build debug version
mvn clean package -Dandroid.ndk.debug=true

# Run GDB server on device
adb shell gdbserver :5039 --attach $(adb shell pidof com.example.ndkapp)

# Connect from host
adb forward tcp:5039 tcp:5039
gdb target/obj/local/armeabi-v7a/libnative-lib.so
(gdb) target remote :5039
```

### LLDB Debugging
```bash
# Start LLDB
lldb
(lldb) platform select remote-android
(lldb) platform connect connect://localhost:5039
(lldb) file target/obj/local/arm64-v8a/libnative-lib.so
```

## Testing Native Code

### Unit Testing with Google Test
```cpp
// test_native_lib.cpp
#include <gtest/gtest.h>
#include "native-lib.h"

TEST(NativeLibTest, AddNumbers) {
    // Test would need refactoring to separate business logic
    EXPECT_EQ(5, 2 + 3);
}

TEST(NativeLibTest, ArrayProcessing) {
    // Test array processing logic
    int input[] = {1, 2, 3};
    int expected[] = {2, 4, 6};
    
    // Process array and verify results
    // Implementation depends on separating JNI from business logic
}
```

## Performance Optimization

### Compiler Flags
```makefile
# Application.mk optimizations
APP_CFLAGS := -O3 -ffast-math -DNDEBUG
APP_CPPFLAGS := -O3 -ffast-math -DNDEBUG
APP_LDFLAGS := -Wl,--gc-sections
```

### Profile-Guided Optimization
```makefile
# First build with profiling
APP_CFLAGS := -fprofile-generate

# After profiling run, rebuild with:
APP_CFLAGS := -fprofile-use
```

## Common Issues and Solutions

### Library Not Found
```bash
# Check if library is included in APK
unzip -l target/ndk-build-example.apk | grep lib

# Verify library loading in logcat
adb logcat | grep "dlopen"
```

### Architecture Mismatch
- Ensure target device architecture matches built libraries
- Use `adb shell getprop ro.product.cpu.abi` to check device architecture
- Build for multiple architectures using `APP_ABI := all`

### JNI Method Not Found
- Verify method signatures match between Java and C++
- Use `javah` to generate correct header files
- Check method name mangling

## Best Practices

1. **Separate JNI from Business Logic**: Keep JNI wrappers thin
2. **Handle Exceptions**: Always check for Java exceptions in JNI code
3. **Memory Management**: Properly release JNI references
4. **Threading**: Use proper synchronization for multi-threaded access
5. **Error Handling**: Use Android logging for debugging
6. **Architecture Support**: Build for multiple architectures
7. **Security**: Validate all inputs from Java layer

## See Also

- [Basic Android App Example](../basic-android-app/) - Basic app setup
- [Advanced Topics](../../advanced-topics.md) - Build optimization
- [Troubleshooting](../../troubleshooting.md) - NDK-specific issues
- [Android NDK Documentation](https://developer.android.com/ndk/)