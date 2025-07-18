# Testing Examples

This directory contains comprehensive examples for testing Android applications with the Android Maven Plugin.

## Available Examples

### Unit Testing
- JUnit unit tests
- Mockito for mocking
- Robolectric for Android unit tests

### Instrumentation Testing
- Espresso UI tests
- AndroidJUnitRunner
- Test APK building

### Code Coverage
- EMMA integration
- Coverage reports
- Coverage enforcement

### UI Testing
- Monkey testing
- UIAutomator tests
- Performance testing

## Project Structure

```
testing/
├── pom.xml
├── AndroidManifest.xml
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/testapp/
│   │   │       ├── MainActivity.java
│   │   │       ├── Calculator.java
│   │   │       └── UserRepository.java
│   │   └── android/
│   │       └── res/
│   ├── test/
│   │   └── java/
│   │       └── com/example/testapp/
│   │           ├── CalculatorTest.java
│   │           ├── UserRepositoryTest.java
│   │           └── MainActivityTest.java (Robolectric)
│   └── androidTest/
│       └── java/
│           └── com/example/testapp/
│               ├── MainActivityEspressoTest.java
│               └── CalculatorInstrumentationTest.java
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
    <artifactId>android-testing-example</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>apk</packaging>
    
    <name>Android Testing Example</name>
    
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        
        <!-- Android Properties -->
        <android.version>4.1.1.4</android.version>
        <android.platform>28</android.platform>
        <android-maven-plugin.version>4.6.1</android-maven-plugin.version>
        
        <!-- Test Properties -->
        <junit.version>4.13.2</junit.version>
        <mockito.version>3.11.2</mockito.version>
        <robolectric.version>4.6.1</robolectric.version>
        <espresso.version>3.4.0</espresso.version>
        <androidx.test.version>1.4.0</androidx.test.version>
    </properties>
    
    <dependencies>
        <!-- Android API -->
        <dependency>
            <groupId>com.google.android</groupId>
            <artifactId>android</artifactId>
            <version>${android.version}</version>
            <scope>provided</scope>
        </dependency>
        
        <!-- Support Libraries -->
        <dependency>
            <groupId>androidx.appcompat</groupId>
            <artifactId>appcompat</artifactId>
            <version>1.3.1</version>
            <type>aar</type>
        </dependency>
        
        <!-- Unit Test Dependencies -->
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
            <groupId>org.robolectric</groupId>
            <artifactId>robolectric</artifactId>
            <version>${robolectric.version}</version>
            <scope>test</scope>
        </dependency>
        
        <!-- Instrumentation Test Dependencies -->
        <dependency>
            <groupId>androidx.test.ext</groupId>
            <artifactId>junit</artifactId>
            <version>1.1.3</version>
            <scope>provided</scope>
        </dependency>
        
        <dependency>
            <groupId>androidx.test.espresso</groupId>
            <artifactId>espresso-core</artifactId>
            <version>${espresso.version}</version>
            <scope>provided</scope>
        </dependency>
        
        <dependency>
            <groupId>androidx.test</groupId>
            <artifactId>runner</artifactId>
            <version>${androidx.test.version}</version>
            <scope>provided</scope>
        </dependency>
        
        <dependency>
            <groupId>androidx.test</groupId>
            <artifactId>rules</artifactId>
            <version>${androidx.test.version}</version>
            <scope>provided</scope>
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
                    
                    <!-- Test Configuration -->
                    <test>
                        <skip>false</skip>
                        <createReport>true</createReport>
                        <coverage>true</coverage>
                        <coverageFile>coverage.ec</coverageFile>
                        <singleInstrumentationCall>true</singleInstrumentationCall>
                    </test>
                    
                    <!-- EMMA Coverage -->
                    <emma>
                        <enable>true</enable>
                        <classpath>target/classes</classpath>
                        <filter>com.example.testapp.*</filter>
                        <outputDirectory>target/emma</outputDirectory>
                    </emma>
                    
                    <!-- Monkey Testing -->
                    <monkey>
                        <eventCount>1000</eventCount>
                        <packages>
                            <package>com.example.testapp</package>
                        </packages>
                        <throttle>100</throttle>
                    </monkey>
                </configuration>
                
                <executions>
                    <!-- Run instrumentation tests -->
                    <execution>
                        <id>instrumentation-tests</id>
                        <phase>integration-test</phase>
                        <goals>
                            <goal>instrument</goal>
                        </goals>
                    </execution>
                    
                    <!-- Generate coverage report -->
                    <execution>
                        <id>emma-report</id>
                        <phase>post-integration-test</phase>
                        <goals>
                            <goal>emma</goal>
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
            
            <!-- Surefire Plugin for Unit Tests -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.2</version>
                <configuration>
                    <testFailureIgnore>false</testFailureIgnore>
                    <includes>
                        <include>**/*Test.java</include>
                    </includes>
                </configuration>
            </plugin>
            
            <!-- Failsafe Plugin for Integration Tests -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-failsafe-plugin</artifactId>
                <version>2.22.2</version>
                <configuration>
                    <includes>
                        <include>**/*IT.java</include>
                    </includes>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>integration-test</goal>
                            <goal>verify</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
    
    <!-- Test Profiles -->
    <profiles>
        <profile>
            <id>unit-tests</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-surefire-plugin</artifactId>
                        <configuration>
                            <skipTests>false</skipTests>
                        </configuration>
                    </plugin>
                </plugins>
            </build>
        </profile>
        
        <profile>
            <id>integration-tests</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>com.simpligility.maven.plugins</groupId>
                        <artifactId>android-maven-plugin</artifactId>
                        <configuration>
                            <test>
                                <skip>false</skip>
                            </test>
                        </configuration>
                    </plugin>
                </plugins>
            </build>
        </profile>
        
        <profile>
            <id>monkey-tests</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>com.simpligility.maven.plugins</groupId>
                        <artifactId>android-maven-plugin</artifactId>
                        <executions>
                            <execution>
                                <id>monkey-test</id>
                                <phase>integration-test</phase>
                                <goals>
                                    <goal>monkey</goal>
                                </goals>
                            </execution>
                        </executions>
                    </plugin>
                </plugins>
            </build>
        </profile>
    </profiles>
</project>
```

## Example Test Classes

### Unit Test Example
```java
// CalculatorTest.java
public class CalculatorTest {
    private Calculator calculator;
    
    @Before
    public void setUp() {
        calculator = new Calculator();
    }
    
    @Test
    public void testAddition() {
        assertEquals(5, calculator.add(2, 3));
    }
    
    @Test
    public void testDivisionByZero() {
        assertThrows(ArithmeticException.class, () -> {
            calculator.divide(10, 0);
        });
    }
}
```

### Robolectric Test Example
```java
// MainActivityTest.java
@RunWith(RobolectricTestRunner.class)
@Config(sdk = 28)
public class MainActivityTest {
    
    @Test
    public void testActivityCreation() {
        ActivityController<MainActivity> controller = 
            Robolectric.buildActivity(MainActivity.class);
        MainActivity activity = controller.create().start().resume().get();
        
        assertNotNull(activity);
        assertTrue(activity.isFinishing() == false);
    }
}
```

### Espresso Test Example
```java
// MainActivityEspressoTest.java
@RunWith(AndroidJUnit4.class)
public class MainActivityEspressoTest {
    
    @Rule
    public ActivityTestRule<MainActivity> activityRule = 
        new ActivityTestRule<>(MainActivity.class);
    
    @Test
    public void testButtonClick() {
        onView(withId(R.id.button))
            .perform(click());
        
        onView(withId(R.id.result))
            .check(matches(withText("Clicked")));
    }
}
```

## Running Tests

### Unit Tests Only
```bash
mvn test
# or with profile
mvn test -P unit-tests
```

### Integration Tests
```bash
# Ensure device is connected
adb devices

# Run instrumentation tests
mvn integration-test -P integration-tests
```

### All Tests with Coverage
```bash
mvn clean verify
```

### Monkey Testing
```bash
mvn integration-test -P monkey-tests
```

## Test Commands Reference

| Command | Description |
|---------|-------------|
| `mvn test` | Run unit tests only |
| `mvn integration-test` | Run integration tests |
| `mvn verify` | Run all tests and verify |
| `mvn android:instrument` | Run instrumentation tests |
| `mvn android:monkey` | Run monkey tests |
| `mvn android:emma` | Generate coverage report |

## Coverage Reports

After running tests with coverage, reports are generated in:
- `target/emma/coverage.html` - HTML coverage report
- `target/emma/coverage.xml` - XML coverage report
- `target/surefire-reports/` - Unit test reports
- `target/failsafe-reports/` - Integration test reports

## Best Practices

1. **Separate unit and integration tests** using different source directories
2. **Use profiles** for different test scenarios
3. **Mock external dependencies** in unit tests
4. **Test on multiple devices** for comprehensive coverage
5. **Set coverage thresholds** to maintain quality
6. **Use page object pattern** for UI tests
7. **Keep tests fast** and independent

## Troubleshooting

### Tests Not Running
- Check that test classes end with "Test"
- Verify test dependencies are in correct scope
- Ensure device is connected for instrumentation tests

### Coverage Not Working
- Verify EMMA is enabled in configuration
- Check that instrumented APK is being used
- Ensure coverage file is being generated and pulled

### Flaky Tests
- Add waits for UI elements
- Use proper test runners
- Isolate test data and state

## See Also

- [Basic Android App Example](../basic-android-app/) - Simple app setup
- [Advanced Topics](../../advanced-topics.md) - CI/CD and automation
- [Troubleshooting](../../troubleshooting.md) - Common testing issues