# Advanced Topics

This document covers advanced features and configurations of the Android Maven Plugin.

## Multi-Module Projects

### Project Structure
```
parent-project/
├── pom.xml (parent)
├── android-app/
│   ├── pom.xml
│   └── src/
├── android-library/
│   ├── pom.xml
│   └── src/
└── shared-resources/
    ├── pom.xml
    └── src/
```

### Parent POM Configuration
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>parent-project</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    
    <modules>
        <module>shared-resources</module>
        <module>android-library</module>
        <module>android-app</module>
    </modules>
    
    <properties>
        <android-maven-plugin.version>4.6.1</android-maven-plugin.version>
        <android.platform>28</android.platform>
    </properties>
    
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>com.google.android</groupId>
                <artifactId>android</artifactId>
                <version>4.1.1.4</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    
    <build>
        <pluginManagement>
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
            </plugins>
        </pluginManagement>
    </build>
</project>
```

## Build Profiles

### Environment-Specific Builds
```xml
<profiles>
    <!-- Development Profile -->
    <profile>
        <id>dev</id>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
        <properties>
            <android.debug>true</android.debug>
            <log.level>DEBUG</log.level>
        </properties>
        <build>
            <plugins>
                <plugin>
                    <groupId>com.simpligility.maven.plugins</groupId>
                    <artifactId>android-maven-plugin</artifactId>
                    <configuration>
                        <lint>
                            <warningsAsErrors>false</warningsAsErrors>
                        </lint>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    </profile>
    
    <!-- Production Profile -->
    <profile>
        <id>prod</id>
        <properties>
            <android.debug>false</android.debug>
            <log.level>ERROR</log.level>
        </properties>
        <build>
            <plugins>
                <plugin>
                    <groupId>com.simpligility.maven.plugins</groupId>
                    <artifactId>android-maven-plugin</artifactId>
                    <configuration>
                        <proguard>
                            <skip>false</skip>
                            <config>proguard-production.cfg</config>
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
    
    <!-- Testing Profile -->
    <profile>
        <id>test</id>
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
</profiles>
```

## Custom Build Configurations

### Resource Filtering
```xml
<build>
    <resources>
        <resource>
            <directory>src/main/android/res</directory>
            <filtering>true</filtering>
            <includes>
                <include>**/*.xml</include>
            </includes>
        </resource>
    </resources>
    
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-resources-plugin</artifactId>
            <configuration>
                <delimiters>
                    <delimiter>@</delimiter>
                </delimiters>
                <useDefaultDelimiters>false</useDefaultDelimiters>
            </configuration>
        </plugin>
    </plugins>
</build>
```

Example filtered resource (strings.xml):
```xml
<resources>
    <string name="app_version">@project.version@</string>
    <string name="build_time">@maven.build.timestamp@</string>
    <string name="api_endpoint">@api.endpoint@</string>
</resources>
```

### Custom Source Directories
```xml
<build>
    <sourceDirectory>src/main/java</sourceDirectory>
    <testSourceDirectory>src/test/java</testSourceDirectory>
    
    <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>build-helper-maven-plugin</artifactId>
            <version>3.2.0</version>
            <executions>
                <execution>
                    <id>add-source</id>
                    <phase>generate-sources</phase>
                    <goals>
                        <goal>add-source</goal>
                    </goals>
                    <configuration>
                        <sources>
                            <source>src/generated/java</source>
                            <source>src/flavor/java</source>
                        </sources>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

## ProGuard Integration

### Advanced ProGuard Configuration
```xml
<configuration>
    <proguard>
        <skip>false</skip>
        <configs>
            <config>proguard-base.cfg</config>
            <config>proguard-android.cfg</config>
            <config>proguard-custom.cfg</config>
        </configs>
        <filterMavenDescriptor>true</filterMavenDescriptor>
        <filterManifest>true</filterManifest>
        <jvmArguments>-Xmx1024m</jvmArguments>
        <options>
            <option>-verbose</option>
            <option>-dontpreverify</option>
            <option>-repackageclasses ''</option>
            <option>-allowaccessmodification</option>
            <option>-optimizations !code/simplification/arithmetic</option>
        </options>
    </proguard>
</configuration>
```

### ProGuard Configuration File (proguard-custom.cfg)
```proguard
# Keep application class
-keep public class * extends android.app.Application

# Keep all public classes in your package
-keep public class com.example.myapp.** { *; }

# Keep native methods
-keepclasseswithmembernames class * {
    native <methods>;
}

# Keep view constructors
-keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet);
}

# Keep enums
-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}

# Keep serializable classes
-keepclassmembers class * implements java.io.Serializable {
    static final long serialVersionUID;
    private static final java.io.ObjectStreamField[] serialPersistentFields;
    private void writeObject(java.io.ObjectOutputStream);
    private void readObject(java.io.ObjectInputStream);
    java.lang.Object writeReplace();
    java.lang.Object readResolve();
}

# Keep library-specific rules
-keep class com.google.gson.** { *; }
-keep class retrofit2.** { *; }
```

## Continuous Integration

### GitHub Actions Configuration
```yaml
name: Android CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Set up JDK 8
      uses: actions/setup-java@v2
      with:
        java-version: '8'
        distribution: 'adopt'
    
    - name: Setup Android SDK
      uses: android-actions/setup-android@v2
      with:
        api-level: 28
        build-tools: 28.0.3
    
    - name: Cache Maven dependencies
      uses: actions/cache@v2
      with:
        path: ~/.m2
        key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
    
    - name: Run tests
      run: mvn clean test -P test
    
    - name: Build APK
      run: mvn clean package -P prod
    
    - name: Run lint
      run: mvn android:lint
    
    - name: Upload APK
      uses: actions/upload-artifact@v2
      with:
        name: app-apk
        path: target/*.apk
```

### Jenkins Pipeline
```groovy
pipeline {
    agent any
    
    tools {
        maven 'Maven-3.6'
        jdk 'JDK-8'
    }
    
    environment {
        ANDROID_HOME = '/opt/android-sdk'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn clean test -P test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                    publishHTML([
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'target/site/jacoco',
                        reportFiles: 'index.html',
                        reportName: 'Coverage Report'
                    ])
                }
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package -P prod'
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh 'mvn deploy'
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: 'target/*.apk', fingerprint: true
        }
    }
}
```

## Signing Configuration

### Release Signing Setup
```xml
<profiles>
    <profile>
        <id>release</id>
        <build>
            <plugins>
                <plugin>
                    <groupId>com.simpligility.maven.plugins</groupId>
                    <artifactId>android-maven-plugin</artifactId>
                    <configuration>
                        <sign>
                            <debug>false</debug>
                            <keystore>${sign.keystore}</keystore>
                            <storepass>${sign.storepass}</storepass>
                            <keypass>${sign.keypass}</keypass>
                            <alias>${sign.alias}</alias>
                            <verify>true</verify>
                        </sign>
                        <zipalign>
                            <skip>false</skip>
                        </zipalign>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    </profile>
</profiles>
```

### Using Environment Variables
```bash
export KEYSTORE_PATH=/path/to/keystore
export KEYSTORE_PASSWORD=secret
export KEY_PASSWORD=secret
export KEY_ALIAS=release

mvn clean package -P release \
    -Dsign.keystore=$KEYSTORE_PATH \
    -Dsign.storepass=$KEYSTORE_PASSWORD \
    -Dsign.keypass=$KEY_PASSWORD \
    -Dsign.alias=$KEY_ALIAS
```

## Performance Optimization

### Build Performance
```xml
<configuration>
    <!-- Parallel builds -->
    <dex>
        <jvmArguments>-Xmx2048m -XX:+UseParallelGC</jvmArguments>
        <preDex>true</preDex>
        <predexLibraries>true</predexLibraries>
    </dex>
    
    <!-- Skip unnecessary goals -->
    <lint>
        <skip>true</skip> <!-- for development builds -->
    </lint>
</configuration>
```

### Maven Configuration (.mvn/maven.config)
```
-T 4
-Dmaven.artifact.threads=4
-Dmaven.compile.fork=true
```

### Memory Settings (.mvn/jvm.config)
```
-Xmx4g
-XX:ReservedCodeCacheSize=512m
-XX:+UseG1GC
```

## Testing Strategies

### Automated Device Testing
```xml
<configuration>
    <devices>
        <device>emulator-5554</device>
        <device>emulator-5556</device>
    </devices>
    
    <test>
        <createReport>true</createReport>
        <singleInstrumentationCall>false</singleInstrumentationCall>
        <testFailSafe>false</testFailSafe>
    </test>
    
    <monkey>
        <eventCount>10000</eventCount>
        <packages>
            <package>com.example.myapp</package>
        </packages>
    </monkey>
</configuration>
```

### Integration with Firebase Test Lab
```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>3.0.0</version>
    <executions>
        <execution>
            <id>firebase-test</id>
            <phase>integration-test</phase>
            <goals>
                <goal>exec</goal>
            </goals>
            <configuration>
                <executable>gcloud</executable>
                <arguments>
                    <argument>firebase</argument>
                    <argument>test</argument>
                    <argument>android</argument>
                    <argument>run</argument>
                    <argument>--app</argument>
                    <argument>target/${project.finalName}.apk</argument>
                    <argument>--test</argument>
                    <argument>target/${project.finalName}-test.apk</argument>
                </arguments>
            </configuration>
        </execution>
    </executions>
</plugin>
```

## Custom Goals and Extensions

### Creating Custom Plugin Goals
```java
@Mojo(name = "custom-goal", defaultPhase = LifecyclePhase.PACKAGE)
public class CustomAndroidMojo extends AbstractAndroidMojo {
    
    @Parameter(property = "android.custom.enabled", defaultValue = "true")
    private boolean enabled;
    
    @Override
    public void execute() throws MojoExecutionException {
        if (!enabled) {
            getLog().info("Custom goal disabled");
            return;
        }
        
        // Custom implementation
        getLog().info("Executing custom Android goal");
    }
}
```

### Plugin Extensions
```xml
<plugin>
    <groupId>com.simpligility.maven.plugins</groupId>
    <artifactId>android-maven-plugin</artifactId>
    <dependencies>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>android-plugin-extension</artifactId>
            <version>1.0.0</version>
        </dependency>
    </dependencies>
</plugin>
```

## Troubleshooting Advanced Issues

### Memory Issues
```xml
<!-- Increase heap size for DEX processing -->
<dex>
    <jvmArguments>-Xmx4096m -XX:MaxPermSize=512m</jvmArguments>
</dex>

<!-- Increase heap size for ProGuard -->
<proguard>
    <jvmArguments>-Xmx2048m</jvmArguments>
</proguard>
```

### Dependency Conflicts
```xml
<dependencies>
    <dependency>
        <groupId>com.google.android</groupId>
        <artifactId>android</artifactId>
        <version>4.1.1.4</version>
        <scope>provided</scope>
        <exclusions>
            <exclusion>
                <groupId>commons-logging</groupId>
                <artifactId>commons-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

### Build Reproducibility
```xml
<properties>
    <maven.build.timestamp.format>yyyy-MM-dd'T'HH:mm:ss'Z'</maven.build.timestamp.format>
    <project.build.outputTimestamp>${maven.build.timestamp}</project.build.outputTimestamp>
</properties>

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-artifact-plugin</artifactId>
    <version>3.2.0</version>
    <configuration>
        <outputTimestamp>${project.build.outputTimestamp}</outputTimestamp>
    </configuration>
</plugin>
```

## Best Practices Summary

1. **Use build profiles** for different environments
2. **Implement proper CI/CD** with automated testing
3. **Optimize build performance** with parallel processing
4. **Use ProGuard** for release builds
5. **Configure proper signing** for distribution
6. **Monitor build metrics** and optimize bottlenecks
7. **Use dependency management** to avoid conflicts
8. **Implement comprehensive testing** strategies
9. **Document build processes** and configurations
10. **Keep plugins and dependencies updated**

## See Also

- [Configuration Reference](configuration-reference.md) - Complete configuration options
- [Goals Reference](goals-reference.md) - Detailed goal documentation
- [Examples](examples/) - Practical implementation examples
- [Troubleshooting](troubleshooting.md) - Common issues and solutions