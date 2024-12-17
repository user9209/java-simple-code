# Javalin

## Gradle

- [Maven Repo](https://mvnrepository.com/artifact/io.javalin/javalin)

```
dependencies {
    // https://mvnrepository.com/artifact/org.slf4j/slf4j-simple
    implementation group: 'org.slf4j', name: 'slf4j-simple', version: '2.0.16'

    // https://mvnrepository.com/artifact/io.javalin/javalin
    implementation (group: 'io.javalin', name: 'javalin', version: '6.3.0') {
        exclude group: 'org.eclipse.jetty', module: 'jetty-server'
    }

    // https://mvnrepository.com/artifact/org.eclipse.jetty/jetty-server
    implementation group: 'org.eclipse.jetty', name: 'jetty-server', version: '11.0.24'
}
```

## Example

### WebServer8080

```
package com.example;

import com.example.handler.BasicHandler;
import io.javalin.Javalin;
import io.javalin.http.staticfiles.Location;

import java.io.File;
import java.net.URL;
import java.nio.file.Files;
import java.nio.file.Path;

import static io.javalin.apibuilder.ApiBuilder.before;
import static io.javalin.apibuilder.ApiBuilder.get;

public class WebServer8080 {

    public static final String JAR_LOCATION = setJarLocation();

    public static void main(String[] args) {

        int PORT_HTTP = 8080;

        org.eclipse.jetty.util.log.Log.setLog(new NoLogging());

        Javalin app = Javalin.create(config -> {

            if(Files.exists(Path.of("data/module/api/htmlStatic"))) {
                config.staticFiles.add("data/module/api/htmlStatic", Location.EXTERNAL);
            }

            if(new File(JAR_LOCATION + "static").exists()) {
                config.staticFiles.add(JAR_LOCATION + "static", Location.EXTERNAL);
            } else {
                System.out.println("No static content!");
            }
            //config.enforceSsl = true;
            config.http.maxRequestSize = 4096000L;

            config.router.apiBuilder(() -> {
                before(BasicHandler.beforeHandler);
                get("/", BasicHandler.handle);
            });

        }).start(PORT_HTTP);

        app.exception(SkipOtherSteps.class, (e, ctx) -> {});

        app.error(404, BasicHandler.notFound);

        Runtime.getRuntime().addShutdownHook(new Thread(app::stop));

        app.events(event -> {
            event.serverStopping(() -> {
                System.out.println("Server is stopping ...");
            });
            event.serverStopped(() -> { System.out.println("Server is stopped successfully!"); });
        });
        // System.err.println("[main] INFO io.javalin.Javalin - Listening also on https://127.0.0.1:" + PORT_HTTPS + "/");
        // System.err.println("[main] INFO io.javalin.Javalin - Have Fun!");
    }

    private static class NoLogging implements org.eclipse.jetty.util.log.Logger {
        @Override public String getName() { return "no"; }
        @Override public void warn(String msg, Object... args) { }
        @Override public void warn(Throwable thrown) { }
        @Override public void warn(String msg, Throwable thrown) { }
        @Override public void info(String msg, Object... args) { }
        @Override public void info(Throwable thrown) { }
        @Override public void info(String msg, Throwable thrown) { }
        @Override public boolean isDebugEnabled() { return false; }
        @Override public void setDebugEnabled(boolean enabled) { }
        @Override public void debug(String msg, Object... args) { }

        @Override
        public void debug(String msg, long value) {}

        @Override public void debug(Throwable thrown) { }
        @Override public void debug(String msg, Throwable thrown) { }
        @Override public org.eclipse.jetty.util.log.Logger getLogger(String name) { return this; }
        @Override public void ignore(Throwable ignored) { }
    }

    private static String setJarLocation() {
        URL jarLocationUrl = WebServer8080.class.getProtectionDomain().getCodeSource().getLocation();
        return new File(jarLocationUrl.toString()).getParentFile().toString().substring(5) + File.separatorChar;
    }

    private static class SkipOtherSteps extends Exception {
    }
}
```

### BasicHandler

```
package com.example.handler;

import io.javalin.http.Handler;
import io.javalin.http.util.NaiveRateLimit;

import java.util.concurrent.TimeUnit;

public class BasicHandler {
    public static Handler beforeHandler = (ctx) -> {
        ctx.res().setHeader("Server", "Hello World");

        NaiveRateLimit.requestPerTimeUnit(ctx, 600, TimeUnit.MINUTES);
    };

    public static Handler notFound = (ctx) -> {
        ctx.res().setHeader("Server","No such service");
        ctx.html("Nix da!");
        ctx.status(404);
    };

    public static Handler handle = (ctx -> {
        ctx.html("<h1>Hello World! Server is running!</h1>");
    });
}

```
