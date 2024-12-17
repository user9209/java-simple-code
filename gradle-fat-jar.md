# Gradle Build Fat jar

```
task createFatJar(type: Jar) {
    manifest {
        attributes 'Implementation-Title': 'HelloWorld',
                'Implementation-Version': version,
                'Main-Class': "com.example.HelloWorld"
    }
    archiveBaseName ='HelloWorld-all'

    duplicatesStrategy = DuplicatesStrategy.EXCLUDE

    exclude "META-INF/*.SF"
    exclude "META-INF/*.DSA"
    exclude "META-INF/*.RSA"

    from { configurations.runtimeClasspath.collect { it.isDirectory() ? it : zipTree(it) } }
    with jar
}
```

## Run

```
task executeApp(type:JavaExec) {
    mainClass = "com.example.HelloWorld"
    classpath = sourceSets.main.runtimeClasspath
}
```
