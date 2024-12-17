# lombok

- Lib: `org.projectlombok.lombok`, group: `org.projectlombok`, name: `lombok`
- See [Maven Repo](https://mvnrepository.com/artifact/org.projectlombok/lombok)

## Gradle

```
dependencies {
    // https://mvnrepository.com/artifact/org.projectlombok/lombok
    compileOnly group: 'org.projectlombok', name: 'lombok', version: '1.18.36'
    annotationProcessor group: 'org.projectlombok', name: 'lombok', version: '1.18.36'
}

tasks.withType(JavaCompile) {
    options.annotationProcessorPath = configurations.annotationProcessor
}
```

## Example

```
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class Person {
    private String name;
    private int age;
}
```


## Maven

```
<dependencies>
    <!-- Lombok dependency -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.36</version> <!-- Use the latest Lombok version -->
        <scope>provided</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <!-- Enable annotation processing for Lombok -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version> <!-- Make sure to use a compatible version -->
            <configuration>
                <source>1.8</source> <!-- Use appropriate Java version -->
                <target>1.8</target> <!-- Use appropriate Java version -->
                <annotationProcessorPaths>
                    <annotationProcessorPath>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok</artifactId>
                        <version>1.18.36</version> <!-- Lombok version -->
                    </annotationProcessorPath>
                </annotationProcessorPaths>
            </configuration>
        </plugin>
    </plugins>
</build>
```

