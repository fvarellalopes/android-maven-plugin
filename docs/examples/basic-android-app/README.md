# Basic Android App Example

This example demonstrates a simple Android application built with the Android Maven Plugin.

## Project Structure

```
basic-android-app/
├── pom.xml
├── AndroidManifest.xml
├── src/
│   └── main/
│       ├── java/
│       │   └── com/example/basicapp/
│       │       └── MainActivity.java
│       └── android/
│           ├── res/
│           │   ├── layout/
│           │   │   └── activity_main.xml
│           │   ├── values/
│           │   │   └── strings.xml
│           │   └── drawable/
│           │       └── icon.png
│           └── assets/
│               └── README.txt
└── target/ (generated during build)
```

## pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>basic-android-app</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>apk</packaging>
    
    <name>Basic Android App</name>
    <description>A simple Android application example</description>
    
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        
        <!-- Android Properties -->
        <android.version>4.1.1.4</android.version>
        <android.platform>28</android.platform>
        <android.sdk.buildtools>28.0.3</android.sdk.buildtools>
        <android-maven-plugin.version>4.6.1</android-maven-plugin.version>
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
            <version>1.1.0</version>
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
                        <buildTools>${android.sdk.buildtools}</buildTools>
                    </sdk>
                    <lint>
                        <skip>false</skip>
                        <failOnError>true</failOnError>
                    </lint>
                    <dex>
                        <jvmArguments>-Xmx1024m</jvmArguments>
                    </dex>
                </configuration>
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
</project>
```

## AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.example.basicapp"
          android:versionCode="1"
          android:versionName="1.0">
    
    <uses-sdk 
        android:minSdkVersion="16" 
        android:targetSdkVersion="28"/>
    
    <application 
        android:label="@string/app_name"
        android:icon="@drawable/icon"
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

## MainActivity.java

```java
package com.example.basicapp;

import android.app.Activity;
import android.os.Bundle;
import android.widget.TextView;

public class MainActivity extends Activity {
    
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        TextView textView = findViewById(R.id.hello_text);
        textView.setText("Hello from Android Maven Plugin!");
    }
}
```

## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="16dp">
    
    <TextView
        android:id="@+id/hello_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/hello_world"
        android:textSize="18sp"
        android:textStyle="bold"
        android:layout_marginBottom="16dp"/>
    
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/welcome_message"
        android:textSize="14sp"
        android:gravity="center"/>
        
</LinearLayout>
```

## strings.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">Basic Android App</string>
    <string name="hello_world">Hello World!</string>
    <string name="welcome_message">Welcome to Android development with Maven!</string>
</resources>
```

## Building the App

### 1. Clean and Compile
```bash
mvn clean compile
```

### 2. Generate APK
```bash
mvn package
```

### 3. Deploy to Device
Make sure you have a device connected or emulator running:
```bash
adb devices
mvn android:deploy
```

### 4. Run the Application
```bash
mvn android:run
```

## Build Lifecycle

The complete build process:
```bash
mvn clean \
    android:generate-sources \
    compile \
    android:dex \
    android:apk \
    android:deploy \
    android:run
```

## Common Commands

| Command | Description |
|---------|-------------|
| `mvn clean` | Clean build directory |
| `mvn compile` | Compile Java sources |
| `mvn package` | Create APK |
| `mvn android:lint` | Run lint analysis |
| `mvn android:deploy` | Install to device |
| `mvn android:undeploy` | Uninstall from device |
| `mvn android:run` | Start the app |

## Customization

### Adding Dependencies
Add more dependencies to the `<dependencies>` section:

```xml
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.6</version>
</dependency>
```

### Configuring Build Tools
Update the build tools version:
```xml
<properties>
    <android.sdk.buildtools>29.0.2</android.sdk.buildtools>
</properties>
```

### Adding Resources
- Add images to `src/main/android/res/drawable/`
- Add layouts to `src/main/android/res/layout/`
- Add strings to `src/main/android/res/values/strings.xml`

## Troubleshooting

### Common Issues

1. **ANDROID_HOME not set**
   ```bash
   export ANDROID_HOME=/path/to/android/sdk
   ```

2. **Build tools not found**
   - Update `android.sdk.buildtools` property
   - Install build tools via SDK Manager

3. **Device not found**
   ```bash
   adb devices
   adb kill-server
   adb start-server
   ```

4. **OutOfMemoryError during DEX**
   - Increase DEX heap size in configuration:
   ```xml
   <dex>
       <jvmArguments>-Xmx2048m</jvmArguments>
   </dex>
   ```

## Next Steps

- Explore the [Android Library Example](../android-library/) to learn about creating reusable components
- Check out [Testing Examples](../testing/) to add unit and integration tests
- Review [Advanced Topics](../../advanced-topics.md) for more complex configurations