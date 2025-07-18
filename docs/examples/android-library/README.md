# Android Library Example

This example demonstrates how to create an Android library (AAR) using the Android Maven Plugin.

## Project Structure

```
android-library/
├── pom.xml
├── AndroidManifest.xml
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/mylibrary/
│   │   │       ├── MyLibrary.java
│   │   │       └── utils/
│   │   │           └── NetworkUtils.java
│   │   └── android/
│   │       ├── res/
│   │       │   ├── layout/
│   │       │   │   └── library_view.xml
│   │       │   ├── values/
│   │       │   │   ├── strings.xml
│   │       │   │   └── colors.xml
│   │       │   └── drawable/
│   │       │       └── library_icon.png
│   │       └── assets/
│   └── test/
│       └── java/
│           └── com/example/mylibrary/
│               └── MyLibraryTest.java
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
    <artifactId>my-android-library</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>aar</packaging>
    
    <name>My Android Library</name>
    <description>A reusable Android library</description>
    
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        
        <!-- Android Properties -->
        <android.version>4.1.1.4</android.version>
        <android.platform>28</android.platform>
        <android.sdk.buildtools>28.0.3</android.sdk.buildtools>
        <android-maven-plugin.version>4.6.1</android-maven-plugin.version>
        
        <!-- Test Properties -->
        <junit.version>4.13.2</junit.version>
        <mockito.version>3.11.2</mockito.version>
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
        
        <!-- HTTP Client -->
        <dependency>
            <groupId>com.squareup.okhttp3</groupId>
            <artifactId>okhttp</artifactId>
            <version>4.9.1</version>
        </dependency>
        
        <!-- JSON Processing -->
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.7</version>
        </dependency>
        
        <!-- Test Dependencies -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
        
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>${mockito.version}</version>
            <scope>test</scope>
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
                        <warningsAsErrors>false</warningsAsErrors>
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
            
            <!-- Maven Surefire Plugin for unit tests -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.2</version>
                <configuration>
                    <testFailureIgnore>false</testFailureIgnore>
                </configuration>
            </plugin>
            
            <!-- Maven Source Plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-source-plugin</artifactId>
                <version>3.2.1</version>
                <executions>
                    <execution>
                        <id>attach-sources</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            
            <!-- Maven Javadoc Plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-javadoc-plugin</artifactId>
                <version>3.3.0</version>
                <executions>
                    <execution>
                        <id>attach-javadocs</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <doclint>none</doclint>
                    <quiet>true</quiet>
                </configuration>
            </plugin>
        </plugins>
    </build>
    
    <!-- Distribution Management for publishing -->
    <distributionManagement>
        <repository>
            <id>releases</id>
            <name>Release Repository</name>
            <url>https://your-repo.com/releases</url>
        </repository>
        <snapshotRepository>
            <id>snapshots</id>
            <name>Snapshot Repository</name>
            <url>https://your-repo.com/snapshots</url>
        </snapshotRepository>
    </distributionManagement>
</project>
```

## AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.example.mylibrary">
    
    <uses-sdk 
        android:minSdkVersion="16" 
        android:targetSdkVersion="28"/>
    
    <!-- Permissions required by the library -->
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    
    <!-- Application tag is not required for libraries -->
    
</manifest>
```

## Library Code

### MyLibrary.java
```java
package com.example.mylibrary;

import android.content.Context;
import android.util.Log;
import com.example.mylibrary.utils.NetworkUtils;

/**
 * Main library class providing core functionality.
 */
public class MyLibrary {
    private static final String TAG = "MyLibrary";
    private Context context;
    private boolean isInitialized = false;
    
    /**
     * Initialize the library with application context.
     * 
     * @param context Application context
     */
    public void initialize(Context context) {
        this.context = context.getApplicationContext();
        this.isInitialized = true;
        Log.d(TAG, "Library initialized successfully");
    }
    
    /**
     * Check if the library is initialized.
     * 
     * @return true if initialized, false otherwise
     */
    public boolean isInitialized() {
        return isInitialized;
    }
    
    /**
     * Get library version.
     * 
     * @return Version string
     */
    public String getVersion() {
        return "1.0.0";
    }
    
    /**
     * Perform network check using utility class.
     * 
     * @return true if network is available
     */
    public boolean isNetworkAvailable() {
        if (!isInitialized) {
            throw new IllegalStateException("Library not initialized");
        }
        return NetworkUtils.isNetworkAvailable(context);
    }
    
    /**
     * Make HTTP request to specified URL.
     * 
     * @param url Target URL
     * @param callback Response callback
     */
    public void makeRequest(String url, NetworkCallback callback) {
        if (!isInitialized) {
            callback.onError(new IllegalStateException("Library not initialized"));
            return;
        }
        
        NetworkUtils.makeHttpRequest(url, callback);
    }
    
    /**
     * Callback interface for network operations.
     */
    public interface NetworkCallback {
        void onSuccess(String response);
        void onError(Throwable error);
    }
}
```

### NetworkUtils.java
```java
package com.example.mylibrary.utils;

import android.content.Context;
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import com.example.mylibrary.MyLibrary;
import okhttp3.*;
import java.io.IOException;

/**
 * Utility class for network operations.
 */
public class NetworkUtils {
    private static final OkHttpClient client = new OkHttpClient();
    
    /**
     * Check if network is available.
     * 
     * @param context Android context
     * @return true if network is available
     */
    public static boolean isNetworkAvailable(Context context) {
        ConnectivityManager connectivityManager = 
            (ConnectivityManager) context.getSystemService(Context.CONNECTIVITY_SERVICE);
        
        if (connectivityManager != null) {
            NetworkInfo activeNetworkInfo = connectivityManager.getActiveNetworkInfo();
            return activeNetworkInfo != null && activeNetworkInfo.isConnected();
        }
        
        return false;
    }
    
    /**
     * Make HTTP GET request.
     * 
     * @param url Target URL
     * @param callback Response callback
     */
    public static void makeHttpRequest(String url, MyLibrary.NetworkCallback callback) {
        Request request = new Request.Builder()
                .url(url)
                .build();
        
        client.newCall(request).enqueue(new Callback() {
            @Override
            public void onFailure(Call call, IOException e) {
                callback.onError(e);
            }
            
            @Override
            public void onResponse(Call call, Response response) throws IOException {
                if (response.isSuccessful()) {
                    callback.onSuccess(response.body().string());
                } else {
                    callback.onError(new IOException("Request failed: " + response.code()));
                }
            }
        });
    }
}
```

## Test Code

### MyLibraryTest.java
```java
package com.example.mylibrary;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import android.content.Context;

import static org.junit.Assert.*;
import static org.mockito.Mockito.*;

/**
 * Unit tests for MyLibrary class.
 */
public class MyLibraryTest {
    
    @Mock
    private Context mockContext;
    
    private MyLibrary library;
    
    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this);
        library = new MyLibrary();
    }
    
    @Test
    public void testInitialization() {
        assertFalse("Library should not be initialized initially", library.isInitialized());
        
        library.initialize(mockContext);
        
        assertTrue("Library should be initialized after calling initialize()", 
                   library.isInitialized());
    }
    
    @Test
    public void testGetVersion() {
        String version = library.getVersion();
        assertNotNull("Version should not be null", version);
        assertEquals("Version should match expected value", "1.0.0", version);
    }
    
    @Test(expected = IllegalStateException.class)
    public void testNetworkCheckWithoutInitialization() {
        library.isNetworkAvailable(); // Should throw exception
    }
}
```

## Resources

### strings.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="library_name">My Android Library</string>
    <string name="network_error">Network error occurred</string>
    <string name="initialization_required">Library must be initialized first</string>
</resources>
```

### colors.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="library_primary">#2196F3</color>
    <color name="library_accent">#FF4081</color>
    <color name="library_text">#212121</color>
</resources>
```

### library_view.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="16dp">
    
    <TextView
        android:id="@+id/library_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/library_name"
        android:textSize="18sp"
        android:textColor="@color/library_text"
        android:textStyle="bold"/>
    
    <View
        android:layout_width="match_parent"
        android:layout_height="1dp"
        android:background="@color/library_primary"
        android:layout_marginTop="8dp"/>
        
</LinearLayout>
```

## Building the Library

### 1. Clean and Compile
```bash
mvn clean compile
```

### 2. Run Unit Tests
```bash
mvn test
```

### 3. Package AAR
```bash
mvn package
```

This creates:
- `target/my-android-library.aar` - The library archive
- `target/my-android-library-sources.jar` - Source code JAR
- `target/my-android-library-javadoc.jar` - Javadoc JAR

### 4. Install to Local Repository
```bash
mvn install
```

### 5. Deploy to Remote Repository
```bash
mvn deploy
```

## Using the Library

### In Another Android Project

Add the dependency to your app's `pom.xml`:

```xml
<dependency>
    <groupId>com.example</groupId>
    <artifactId>my-android-library</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <type>aar</type>
</dependency>
```

### Usage Example

```java
public class MainActivity extends Activity {
    private MyLibrary library;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        // Initialize library
        library = new MyLibrary();
        library.initialize(this);
        
        // Use library functionality
        if (library.isNetworkAvailable()) {
            library.makeRequest("https://api.example.com/data", 
                new MyLibrary.NetworkCallback() {
                    @Override
                    public void onSuccess(String response) {
                        // Handle success
                    }
                    
                    @Override
                    public void onError(Throwable error) {
                        // Handle error
                    }
                });
        }
    }
}
```

## Advanced Features

### ProGuard Support

Add ProGuard rules to keep library classes:

```proguard
# Keep library classes
-keep class com.example.mylibrary.** { *; }

# Keep callback interfaces
-keep interface com.example.mylibrary.MyLibrary$NetworkCallback { *; }
```

### Custom Build Variants

For different library variants:

```xml
<profiles>
    <profile>
        <id>debug</id>
        <properties>
            <android.debug>true</android.debug>
        </properties>
    </profile>
    
    <profile>
        <id>release</id>
        <properties>
            <android.debug>false</android.debug>
        </properties>
        <build>
            <plugins>
                <plugin>
                    <groupId>com.simpligility.maven.plugins</groupId>
                    <artifactId>android-maven-plugin</artifactId>
                    <configuration>
                        <proguard>
                            <skip>false</skip>
                            <config>proguard-library.cfg</config>
                        </proguard>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    </profile>
</profiles>
```

## Best Practices

1. **Keep manifest minimal** - Only declare essential permissions
2. **Use proper packaging** - Always use `aar` packaging for libraries
3. **Provide source and javadoc** - Include source and documentation JARs
4. **Test thoroughly** - Write comprehensive unit tests
5. **Version properly** - Use semantic versioning
6. **Document API** - Provide clear javadoc comments
7. **Minimize dependencies** - Only include necessary dependencies

## Next Steps

- Explore [Testing Examples](../testing/) for more testing strategies
- Check [Multi-Module Project](../multi-module/) for library integration
- Review [Advanced Topics](../../advanced-topics.md) for optimization techniques