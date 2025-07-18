# Multi-Module Project Example

This example demonstrates a multi-module Android project structure using the Android Maven Plugin.

## Project Structure

```
multi-module/
├── pom.xml (parent)
├── shared-library/
│   ├── pom.xml
│   ├── AndroidManifest.xml
│   └── src/
│       └── main/
│           ├── java/
│           │   └── com/example/shared/
│           │       ├── NetworkClient.java
│           │       ├── DatabaseHelper.java
│           │       └── utils/
│           │           └── StringUtils.java
│           └── android/
│               └── res/
├── feature-library/
│   ├── pom.xml
│   ├── AndroidManifest.xml
│   └── src/
│       └── main/
│           ├── java/
│           │   └── com/example/feature/
│           │       ├── FeatureManager.java
│           │       └── widgets/
│           │           └── CustomButton.java
│           └── android/
│               └── res/
├── main-app/
│   ├── pom.xml
│   ├── AndroidManifest.xml
│   └── src/
│       └── main/
│           ├── java/
│           │   └── com/example/app/
│           │       └── MainActivity.java
│           └── android/
│               └── res/
└── integration-tests/
    ├── pom.xml
    └── src/
        └── main/
            └── java/
                └── com/example/tests/
                    └── IntegrationTest.java
```

## Parent POM (pom.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>multi-module-parent</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    
    <name>Multi-Module Android Project</name>
    <description>Parent POM for multi-module Android project</description>
    
    <!-- Module Declaration -->
    <modules>
        <module>shared-library</module>
        <module>feature-library</module>
        <module>main-app</module>
        <module>integration-tests</module>
    </modules>
    
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        
        <!-- Version Properties -->
        <android.version>4.1.1.4</android.version>
        <android.platform>28</android.platform>
        <android.sdk.buildtools>28.0.3</android.sdk.buildtools>
        <android-maven-plugin.version>4.6.1</android-maven-plugin.version>
        
        <!-- Dependency Versions -->
        <androidx.appcompat.version>1.3.1</androidx.appcompat.version>
        <junit.version>4.13.2</junit.version>
        <mockito.version>3.11.2</mockito.version>
        <espresso.version>3.4.0</espresso.version>
    </properties>
    
    <!-- Dependency Management -->
    <dependencyManagement>
        <dependencies>
            <!-- Android -->
            <dependency>
                <groupId>com.google.android</groupId>
                <artifactId>android</artifactId>
                <version>${android.version}</version>
                <scope>provided</scope>
            </dependency>
            
            <!-- AndroidX -->
            <dependency>
                <groupId>androidx.appcompat</groupId>
                <artifactId>appcompat</artifactId>
                <version>${androidx.appcompat.version}</version>
                <type>aar</type>
            </dependency>
            
            <!-- Internal Dependencies -->
            <dependency>
                <groupId>${project.groupId}</groupId>
                <artifactId>shared-library</artifactId>
                <version>${project.version}</version>
                <type>aar</type>
            </dependency>
            
            <dependency>
                <groupId>${project.groupId}</groupId>
                <artifactId>feature-library</artifactId>
                <version>${project.version}</version>
                <type>aar</type>
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
            
            <dependency>
                <groupId>androidx.test.espresso</groupId>
                <artifactId>espresso-core</artifactId>
                <version>${espresso.version}</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    
    <!-- Plugin Management -->
    <build>
        <pluginManagement>
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
                        <dex>
                            <jvmArguments>-Xmx1024m</jvmArguments>
                        </dex>
                        <lint>
                            <failOnError>true</failOnError>
                            <warningsAsErrors>false</warningsAsErrors>
                        </lint>
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
                
                <!-- Maven Surefire Plugin -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>2.22.2</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
    
    <!-- Build Profiles -->
    <profiles>
        <profile>
            <id>development</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <android.debug>true</android.debug>
                <android.lint.skip>true</android.lint.skip>
            </properties>
        </profile>
        
        <profile>
            <id>testing</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>com.simpligility.maven.plugins</groupId>
                        <artifactId>android-maven-plugin</artifactId>
                        <configuration>
                            <test>
                                <coverage>true</coverage>
                                <createReport>true</createReport>
                            </test>
                            <emma>
                                <enable>true</enable>
                            </emma>
                        </configuration>
                    </plugin>
                </plugins>
            </build>
        </profile>
        
        <profile>
            <id>release</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>com.simpligility.maven.plugins</groupId>
                        <artifactId>android-maven-plugin</artifactId>
                        <configuration>
                            <proguard>
                                <skip>false</skip>
                                <config>proguard.cfg</config>
                            </proguard>
                            <zipalign>
                                <skip>false</skip>
                            </zipalign>
                            <lint>
                                <failOnError>true</failOnError>
                                <warningsAsErrors>true</warningsAsErrors>
                            </lint>
                        </configuration>
                    </plugin>
                </plugins>
            </build>
        </profile>
    </profiles>
</project>
```

## Shared Library (shared-library/pom.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <parent>
        <groupId>com.example</groupId>
        <artifactId>multi-module-parent</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    
    <artifactId>shared-library</artifactId>
    <packaging>aar</packaging>
    
    <name>Shared Library</name>
    <description>Common utilities and services</description>
    
    <dependencies>
        <!-- Android API -->
        <dependency>
            <groupId>com.google.android</groupId>
            <artifactId>android</artifactId>
        </dependency>
        
        <!-- External Libraries -->
        <dependency>
            <groupId>com.squareup.okhttp3</groupId>
            <artifactId>okhttp</artifactId>
            <version>4.9.1</version>
        </dependency>
        
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.7</version>
        </dependency>
        
        <!-- Test Dependencies -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
        
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>com.simpligility.maven.plugins</groupId>
                <artifactId>android-maven-plugin</artifactId>
            </plugin>
            
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

## Feature Library (feature-library/pom.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <parent>
        <groupId>com.example</groupId>
        <artifactId>multi-module-parent</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    
    <artifactId>feature-library</artifactId>
    <packaging>aar</packaging>
    
    <name>Feature Library</name>
    <description>Feature-specific components and widgets</description>
    
    <dependencies>
        <!-- Android API -->
        <dependency>
            <groupId>com.google.android</groupId>
            <artifactId>android</artifactId>
        </dependency>
        
        <!-- AndroidX -->
        <dependency>
            <groupId>androidx.appcompat</groupId>
            <artifactId>appcompat</artifactId>
        </dependency>
        
        <!-- Internal Dependencies -->
        <dependency>
            <groupId>${project.groupId}</groupId>
            <artifactId>shared-library</artifactId>
        </dependency>
        
        <!-- Test Dependencies -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
        
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>com.simpligility.maven.plugins</groupId>
                <artifactId>android-maven-plugin</artifactId>
            </plugin>
            
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

## Main App (main-app/pom.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <parent>
        <groupId>com.example</groupId>
        <artifactId>multi-module-parent</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    
    <artifactId>main-app</artifactId>
    <packaging>apk</packaging>
    
    <name>Main Application</name>
    <description>Main Android application</description>
    
    <dependencies>
        <!-- Android API -->
        <dependency>
            <groupId>com.google.android</groupId>
            <artifactId>android</artifactId>
        </dependency>
        
        <!-- AndroidX -->
        <dependency>
            <groupId>androidx.appcompat</groupId>
            <artifactId>appcompat</artifactId>
        </dependency>
        
        <!-- Internal Dependencies -->
        <dependency>
            <groupId>${project.groupId}</groupId>
            <artifactId>shared-library</artifactId>
        </dependency>
        
        <dependency>
            <groupId>${project.groupId}</groupId>
            <artifactId>feature-library</artifactId>
        </dependency>
        
        <!-- Test Dependencies -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
        
        <dependency>
            <groupId>androidx.test.espresso</groupId>
            <artifactId>espresso-core</artifactId>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>com.simpligility.maven.plugins</groupId>
                <artifactId>android-maven-plugin</artifactId>
            </plugin>
            
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

## Integration Tests (integration-tests/pom.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <parent>
        <groupId>com.example</groupId>
        <artifactId>multi-module-parent</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>
    
    <artifactId>integration-tests</artifactId>
    <packaging>apk</packaging>
    
    <name>Integration Tests</name>
    <description>Integration tests for the entire application</description>
    
    <dependencies>
        <!-- Main app dependency -->
        <dependency>
            <groupId>${project.groupId}</groupId>
            <artifactId>main-app</artifactId>
            <type>apk</type>
            <scope>provided</scope>
        </dependency>
        
        <!-- Test dependencies -->
        <dependency>
            <groupId>com.google.android</groupId>
            <artifactId>android</artifactId>
        </dependency>
        
        <dependency>
            <groupId>androidx.test.espresso</groupId>
            <artifactId>espresso-core</artifactId>
        </dependency>
        
        <dependency>
            <groupId>androidx.test.ext</groupId>
            <artifactId>junit</artifactId>
            <version>1.1.3</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>com.simpligility.maven.plugins</groupId>
                <artifactId>android-maven-plugin</artifactId>
                <configuration>
                    <test>
                        <createReport>true</createReport>
                        <coverage>true</coverage>
                    </test>
                </configuration>
                <executions>
                    <execution>
                        <id>deploy-app</id>
                        <phase>pre-integration-test</phase>
                        <goals>
                            <goal>deploy</goal>
                        </goals>
                        <configuration>
                            <apk>${project.parent.basedir}/main-app/target/main-app.apk</apk>
                        </configuration>
                    </execution>
                    
                    <execution>
                        <id>integration-tests</id>
                        <phase>integration-test</phase>
                        <goals>
                            <goal>instrument</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

## Example Source Files

### Shared Library - NetworkClient.java
```java
package com.example.shared;

import okhttp3.*;
import java.io.IOException;
import java.util.concurrent.TimeUnit;

public class NetworkClient {
    private static final OkHttpClient client = new OkHttpClient.Builder()
            .connectTimeout(10, TimeUnit.SECONDS)
            .readTimeout(30, TimeUnit.SECONDS)
            .build();
    
    public interface Callback {
        void onSuccess(String response);
        void onError(Exception error);
    }
    
    public static void get(String url, Callback callback) {
        Request request = new Request.Builder()
                .url(url)
                .build();
        
        client.newCall(request).enqueue(new okhttp3.Callback() {
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

### Feature Library - FeatureManager.java
```java
package com.example.feature;

import com.example.shared.NetworkClient;
import com.example.shared.utils.StringUtils;

public class FeatureManager {
    private boolean initialized = false;
    
    public void initialize() {
        this.initialized = true;
    }
    
    public boolean isInitialized() {
        return initialized;
    }
    
    public void loadData(String endpoint, NetworkClient.Callback callback) {
        if (!initialized) {
            callback.onError(new IllegalStateException("FeatureManager not initialized"));
            return;
        }
        
        String cleanUrl = StringUtils.trimAndValidate(endpoint);
        if (cleanUrl == null) {
            callback.onError(new IllegalArgumentException("Invalid endpoint"));
            return;
        }
        
        NetworkClient.get(cleanUrl, callback);
    }
}
```

### Main App - MainActivity.java
```java
package com.example.app;

import android.app.Activity;
import android.os.Bundle;
import android.widget.Toast;
import com.example.feature.FeatureManager;
import com.example.shared.NetworkClient;

public class MainActivity extends Activity {
    private FeatureManager featureManager;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        featureManager = new FeatureManager();
        featureManager.initialize();
        
        loadInitialData();
    }
    
    private void loadInitialData() {
        featureManager.loadData("https://api.example.com/data", new NetworkClient.Callback() {
            @Override
            public void onSuccess(String response) {
                runOnUiThread(() -> {
                    Toast.makeText(MainActivity.this, "Data loaded", Toast.LENGTH_SHORT).show();
                });
            }
            
            @Override
            public void onError(Exception error) {
                runOnUiThread(() -> {
                    Toast.makeText(MainActivity.this, "Error: " + error.getMessage(), 
                                   Toast.LENGTH_LONG).show();
                });
            }
        });
    }
}
```

## Building the Multi-Module Project

### Build All Modules
```bash
mvn clean package
```

### Build Specific Module
```bash
# Build only shared library
mvn clean package -pl shared-library

# Build app and its dependencies
mvn clean package -pl main-app -am
```

### Run Tests
```bash
# Unit tests for all modules
mvn test

# Integration tests
mvn integration-test -pl integration-tests
```

### Build with Different Profiles
```bash
# Development build
mvn clean package -P development

# Testing build with coverage
mvn clean verify -P testing

# Release build
mvn clean package -P release
```

## Dependency Management Benefits

1. **Version Consistency**: All modules use the same versions defined in parent POM
2. **Simplified Updates**: Update versions in one place
3. **Dependency Inheritance**: Child modules inherit common dependencies
4. **Build Coordination**: Maven builds modules in correct order

## Module Build Order

Maven automatically determines build order based on dependencies:
1. `shared-library` (no internal dependencies)
2. `feature-library` (depends on shared-library)
3. `main-app` (depends on both libraries)
4. `integration-tests` (depends on main-app)

## Best Practices

### Project Structure
- **Keep modules focused**: Each module should have a single responsibility
- **Minimize dependencies**: Avoid circular dependencies between modules
- **Use consistent naming**: Follow naming conventions for modules

### Dependency Management
- **Use dependencyManagement**: Define versions in parent POM
- **Scope appropriately**: Use correct scopes (provided, test, etc.)
- **Minimize transitive dependencies**: Exclude unnecessary dependencies

### Build Configuration
- **Use pluginManagement**: Centralize plugin configuration
- **Profile separation**: Use profiles for different build scenarios
- **Version properties**: Use properties for version management

### Testing Strategy
- **Unit tests per module**: Test each module independently
- **Integration tests**: Separate module for integration testing
- **Test isolation**: Ensure tests don't interfere with each other

## Common Commands

| Command | Description |
|---------|-------------|
| `mvn clean package` | Build all modules |
| `mvn clean package -pl module-name` | Build specific module |
| `mvn clean package -pl module-name -am` | Build module and dependencies |
| `mvn clean package -pl module-name -amd` | Build module and dependents |
| `mvn dependency:tree` | Show dependency tree |
| `mvn clean install` | Install all modules to local repository |

## Troubleshooting

### Build Order Issues
- Check for circular dependencies using `mvn dependency:tree`
- Use `-am` flag to build dependencies first

### Version Conflicts
- Use `mvn dependency:analyze` to find issues
- Check dependencyManagement in parent POM

### Module Not Found
- Ensure modules are declared in parent POM
- Check relative paths in module declarations

## Advanced Features

### Parallel Builds
```bash
mvn clean package -T 4  # Use 4 threads
```

### Selective Module Building
```bash
# Build only changed modules (with git)
mvn clean package -pl $(git diff --name-only HEAD~1 | grep pom.xml | xargs dirname | paste -sd,)
```

### Profile Activation
```xml
<profile>
    <id>module-specific</id>
    <activation>
        <file>
            <exists>src/main/special-feature</exists>
        </file>
    </activation>
</profile>
```

## See Also

- [Android Library Example](../android-library/) - Single library creation
- [Advanced Topics](../../advanced-topics.md) - CI/CD for multi-module projects
- [Configuration Reference](../../configuration-reference.md) - Complete configuration options
- [Maven Multi-Module Documentation](https://maven.apache.org/guides/mini/guide-multiple-modules.html)