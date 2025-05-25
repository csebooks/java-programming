---
title: 'Tools'
weight: 6
---

# Java Tools

Java Development Kit (JDK) tools, providing an overview, purpose, usage examples, and notes for each tool. These tools are critical for developing, debugging, and managing Java applications, making them valuable for students and developers alike.

## 1. jdeprscan – Deprecated API Scanner
**Hunt Deprecated Dinosaurs**

### Purpose
`jdeprscan` analyzes Java code to detect usage of deprecated APIs in JAR or class files, helping developers identify and update outdated code.

### Usage
- **Scan a JAR file for deprecated API usage**:
  ```bash
  jdeprscan jackson-core-2.18.3.jar
  ```
  **Output**:
  ```
  Jar file jackson-core-2.18.3.jar:
  class com/faster...DoubleToDecimal uses deprecated method java/lang/String::<init>([BIII)V
  class com/faster...FloatToDecimal uses deprecated method java/lang/String::<init>([BIII)V
  ```
- **List deprecated APIs for Java versions 9 to 24**:
  ```bash
  for version in {9..24}; do
    all=$(jdeprscan --list --release "$version" 2>/dev/null | wc -l)
    removal=$(jdeprscan --list --for-removal --release "$version" 2>/dev/null | wc -l)
    echo "$version,$all,$removal"
  done
  ```

### Notes
- Helps maintain code compatibility by identifying APIs marked for removal.
- Useful for upgrading projects to newer Java versions.

## 2. jshell – Java REPL
**Test Ideas. Live and Fast**

### Purpose
`jshell` is an interactive Read-Eval-Print Loop (REPL) for executing Java code snippets, ideal for testing and learning.

### Usage
- **Start jshell**:
  ```bash
  jshell
  ```
- **Examples**:
  ```java
  jshell> var result = 0.1 + 0.2;
  result ==> 0.30000000000000004

  jshell> int test() {
      ...> try { return 1; }
      ...> finally { return 2; }
      ...> }
  | created method test()
  jshell> var result = test()
  result ==> 2

  jshell> Integer a = 127; Integer b = 127; var result = a == b;
  a ==> 127
  b ==> 127
  result ==> true

  jshell> Integer c = 128; Integer d = 128; var result = c == d;
  c ==> 128
  d ==> 128
  result ==> false

  jshell> var result = 'A' == 65
  result ==> true
  ```
- **Get help**:
  ```java
  jshell> /help
  ```

### Notes
- Demonstrates Java behaviors like floating-point precision, `finally` block overrides, `Integer` caching, and char-integer comparisons.
- Ideal for quick prototyping and learning Java syntax.

## 3. jps – JVM Process Status Tool
**JVMs Around You – Listed Live**

### Purpose
`jps` lists running Java Virtual Machines (JVMs) on a machine, providing process IDs (PIDs), arguments, and JVM flags.

### Usage
- **List running JVMs**:
  ```bash
  jps -l
  ```
  **Output**:
  ```
  8640 com.intellij.idea.Main
  30227 com.darioscript.jdktools.JdkToolsApplication
  25091 org.gradle.launcher.daemon.bootstrap.GradleDaemon
  30380 jdk.jcmd/sun.tools.jps.Jps
  ```
- **Kill a process**:
  ```bash
  kill -9 30227
  ```
  **Output**:
  ```
  Process finished with exit code 137 (interrupted by signal 9:SIGKILL)
  ```

### Notes
- Useful for identifying and managing running Java processes.
- Can help resolve issues like port conflicts (e.g., "Port 8080 was already in use").

## 4. jwebserver – Web Server
**Start Serving in One Line**

### Purpose
`jwebserver` is a simple command-line tool to serve static files, ideal for quick testing and lightweight web serving.

### Usage
- **Start the web server**:
  ```bash
  jwebserver
  ```
  **Output**:
  ```
  Binding to loopback by default...
  Serving /home/.../jdk-tools and subdirectories on 127.0.0.1 port 8000
  URL http://127.0.0.1:8000/
  ```
- **Start on a specific port**:
  ```bash
  jwebserver --directory /path-to-serve --port 4200
  ```
- **Open in browser**:
  ```bash
  open http://localhost:4200
  ```

### Notes
- No complex server setup required.
- Useful for serving static content during development.

## 5. jpackage – Java App Packager
**Bundle. Deliver. Run**

### Purpose
`jpackage` creates native installable packages (e.g., `.exe`, `.dmg`, `.rpm`, `.deb`) that include the application, dependencies, and a Java runtime.

### Usage
- **Create a Debian package**:
  ```bash
  jpackage --input . \
    --name jdk-tools \
    --main-jar jdk-tools.jar \
    --type deb \
    --app-version 1.0.0
  ```
- **Install the package**:
  ```bash
  sudo dpkg -i jdk-tools_1.0.0_amd64.deb
  ```
- **Run the application**:
  ```bash
  /opt/jdk-tools/bin/jdk-tools
  ```
  **Output**:
  ```
  . ____ _ __ _ _
  /\\ / ___'_ __ _ _(_)_ __ __ _ \ \ \ \
  ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
  \\/ ___)| |_)| | | | | || (_| | ) ) ) )
   ' |____| .__|_| |_|_| |_\__, | / / / /
  =========|_|==============|___/=/_/_/_/
  :: Spring Boot :: (v3.4.5)
  ...
  ```

### Notes
- Creates self-contained applications that don’t require a pre-installed JRE.
- Can be used with `jlink` for custom runtime images.
- Supports multiple package formats for cross-platform deployment.

## 6. javac – Java Compiler
**Turning Code into Bytecode Magic**

### Purpose
`javac` compiles Java source files (`.java`) into bytecode (`.class`) for execution on the JVM.

### Usage
- **Basic compilation**:
  ```bash
  javac Player.java
  ```
- **Specify output directory**:
  ```bash
  javac -d target/classes Player.java
  ```
- **Enable warnings**:
  ```bash
  javac -Xlint:serial Player.java
  ```
  **Output**:
  ```
  Player.java:5: warning: [serial] serializable class Player has no definition of serialVersionUID
  class Player implements Serializable {
  ^
  1 warning
  ```
- **Compile with a module**:
  ```bash
  javac -d target/classes \
    --module-path target/libs/awesome.jar \
    src/main/java/module-info.java \
    src/main/java/com/darioscript/Player.java
  ```
- **Example Player class**:
  ```java
  class Player implements Serializable {
    public void celebrate() {
      System.out.println("Siuuuuuuuuu!!!! 🕺⚽🔥");
    }
  }
  ```

### Notes
- Essential for preparing Java code for execution.
- Use `-Xlint` to catch potential issues like missing `serialVersionUID`.

## 7. jar – Java Archive Tool
**Box It, Seal It, Ship It**

### Purpose
`jar` creates and manages Java Archive (JAR) files, a ZIP-based format for distributing Java applications and libraries.

### Usage
- **Create a JAR with entry point**:
  ```bash
  jar --create --file target/libs/player.jar \
    --main-class com.darioscript.Player \
    -C target/classes .
  ```
- **View contents**:
  ```bash
  jar --list --file target/libs/player.jar
  ```
- **Extract all files**:
  ```bash
  jar --extract --file player.jar
  ```
- **Extract a specific file**:
  ```bash
  jar --extract --file target/libs/player.jar META-INF/MANIFEST.MF
  ```
  **Output (MANIFEST.MF)**:
  ```
  Manifest-Version: 1.0
  Created-By: 24.0.1 (Eclipse Adoptium)
  Main-Class: com.darioscript.Player
  ```

### Notes
- Used in build pipelines and for modular packaging with `jmod` and `jlink`.
- JAR files are essential for distributing Java libraries and applications.

## 8. java – Application Launcher
**Push the Button. Run the App**

### Purpose
`java` launches and runs Java applications from `.class` files, `.java` sources, or modules, serving as the JVM entry point.

### Usage
- **Run compiled classes**:
  ```bash
  cd target/classes/
  java com.darioscript.Player
  ```
  **Output**:
  ```
  Siuuuuuuuuu!!!! 🕺⚽🔥
  ```
- **Run with a module**:
  ```bash
  java --module-path target/libs -m com.darioscript.player
  ```
- **Run source files (Java 11+)**:
  ```bash
  java Player.java
  ```
  **Output**:
  ```
  Siuuuuuuuuu!!!! 🕺⚽🔥
  ```
- **Run with dependencies (Java 21+)**:
  ```bash
  java -cp "target/libs/*" Player.java
  ```
- **Run with JVM options**:
  ```bash
  java -Xmx10m -Denv=prod Player.java Mbappe Vini
  ```
  **Output**:
  ```
  Arguments: [Mbappe, Vini]
  JVM Options: [-Xmx10m, -Denv=prod, --add-modules=ALL-DEFAULT]
  ```

### Notes
- JVM behavior can be customized with options like memory limits (`-Xmx`) and system properties (`-D`).
- Supports direct source file execution since Java 11.

## 9. javadoc – Documentation Generator
**Code Speaks. Docs Explain**

### Purpose
`javadoc` generates HTML documentation from Javadoc comments in source code, essential for creating readable API documentation.

### Usage
- **Generate docs for a single class**:
  ```bash
  javadoc Player.java -d apidocs
  ```
  **Output directory structure**:
  ```
  apidocs/
  ├── index.html
  ├── com/
  │   └── darioscript/
  │       ├── Player.html
  │       ├── package-tree.html
  │       └── package-summary.html
  └── resources/
      ├── glass.png
      └── x.png
  ```

### Notes
- Often integrated into CI/CD pipelines for automatic documentation.
- Supports Markdown in Javadoc comments (JEP 467) for easier writing.
- Clean documentation indicates a well-maintained API.

## 10. javap – Class File Disassembler
**Peek into the Bytecode Brain**

### Purpose
`javap` disassembles `.class` files to display bytecode and method signatures, useful for debugging and understanding compiled code.

### Usage
- **Show public API of a class**:
  ```bash
  javap -public java.lang.Boolean
  ```
  **Output**:
  ```
  Compiled from "Boolean.java"
  public final class java.lang.Boolean implements java.io.Serializable,
      java.lang.Comparable<java.lang.Boolean>,
      java.lang.constant.Constable {
    public static final java.lang.Boolean TRUE;
    public static final java.lang.Boolean FALSE;
    public static final java.lang.Class<java.lang.Boolean> TYPE;
    public static java.lang.Boolean valueOf(boolean);
    public static java.lang.Boolean valueOf(java.lang.String);
    public static java.lang.String toString(boolean);
    ...
    public static int compare(boolean, boolean);
    public static boolean logicalAnd(boolean, boolean);
    public static boolean logicalOr(boolean, boolean);
    public static boolean logicalXor(boolean, boolean);
    public java.util.Optional<java.lang.constant.DynamicConstantDesc<java.lang.Boolean>>;
    public int compareTo(java.lang.Object);
  }
  ```

### Notes
- Helps verify compiled code and understand JVM internals.
- Useful for learning how Java classes are structured at the bytecode level.

## 11. jdb – Java Debugger
**Step Through Bugs. Fix with Style**

### Purpose
`jdb` is a command-line debugger for finding and fixing bugs in Java applications, allowing runtime inspection and modification.

### Usage
- **Debug an application**:
  ```bash
  jdb com.darioscript.Player
  ```
  **Example session**:
  ```
  Initializing jdb ...
  > stop at com.darioscript.Player:12
  Deferring breakpoint com.darioscript.Player:12.
  It will be set after the class is loaded.
  > run
  run com.darioscript.Player
  Set uncaught java.lang.Throwable
  Set deferred uncaught java.lang.Throwable
  >
  VM Started: Set deferred breakpoint com.darioscript.Player:12
  Arguments: []
  Breakpoint hit: "thread=main", com.darioscript.Player.main(), line=12 bci=23
  12 player.celebrate();
  main[1] list
  8
  9 public static void main(String[] args) {
  10     System.out.println("Arguments: " + java.util.Arrays.toString(args));
  11     var player = new Player();
  12 =>  player.celebrate();
  13 }
  14 }
  main[1] print player
   player = "com.darioscript.Player@7bbaa5a2"
  ```

### Notes
- Requires compilation with debugging information (`javac -g`).
- Allows stepping through code and inspecting variables.

## 12. jdeps – Dependency Analyzer
**Trace Every Import’s Footprint**

### Purpose
`jdeps` analyzes class and module dependencies, generating reports to understand relationships and modularize applications.

### Usage
- **Detect module dependencies**:
  ```bash
  jdeps --print-module-deps \
    --class-path "BOOT-INF/lib/*" \
    --multi-release 24 \
    --ignore-missing-deps \
    --recursive jdk-tools.jar
  ```
  **Output**:
  ```
  java.base,java.compiler,java.desktop,java.instrument,java.management,
  java.net.http,java.prefs,java.rmi,java.scripting,java.security.jgss,
  java.sql.rowset,jdk.jfr,jdk.net,jdk.unsupported
  ```
- **Generate module-info.java**:
  ```bash
  jdeps --generate-module-info . --ignore-missing-deps jdk-tools.jar
  ```
  **Output**:
  ```
  # writing to ./jdk.tools/module-info.java
  module jdk.tools {
    exports org.springframework.boot.loader.jar;
    exports org.springframework.boot.loader.jarmode;
    exports org.springframework.boot.loader.launch;
    exports org.springframework.boot.loader.log;
    ...
  }
  ```

### Notes
- Essential for migrating to Java 9+ modules.
- Helps identify and resolve unnecessary dependencies.

## 13. jlink – Custom Runtime Image Creator
**Assemble Your Java, Your Way**

### Purpose
`jlink` creates optimized custom runtime images by assembling modules and their dependencies, reducing size and improving performance.

### Usage
- **Create a custom runtime image**:
  ```bash
  jlink --add-modules java.base,java.logging \
    --output my-app-jre
  ```
  **Output directory structure**:
  ```
  my-app-jre/
  ├── bin/
  │   ├── java
  │   └── ...
  ├── conf/
  │   └── ...
  ├── lib/
  │   ├── modules
  │   └── ...
  ├── release
  └── ...
  ```

### Notes
- Ideal for creating small, standalone applications.
- Reduces JVM size and attack surface in production.

## 14. jmod – Java Module Tool
**Build Java One Module at a Time**

### Purpose
`jmod` manages and packages JMOD files, a format for modular Java code in the Java Platform Module System (JPMS).

### Usage
- **Create a JMOD file**:
  ```bash
  jmod create --class-path target/classes \
    --module-path target/classes \
    --main-class com.darioscript.Player \
    --module-version 1.0.0 modules/player.mod
  ```
- **List contents**:
  ```bash
  jmod list modules/player.mod
  ```
  **Output**:
  ```
  classes/module-info.class
  classes/com/darioscript/Player.class
  ```
- **Describe a JMOD file**:
  ```bash
  jmod describe modules/player.mod
  ```
  **Output**:
  ```
  com.darioscript.jdktools@1.0.0
  exports com.darioscript
  requires java.base mandated
  main-class com.darioscript.Player
  ```

### Notes
- Used for modularization in Java 9+.
- Combines with `jlink` for custom runtime images.

## 15. jinfo – JVM Config Inspector
**JVM Settings, Exposed**

### Purpose
`jinfo` queries and modifies configuration information of a running Java process, providing insights into JVM properties and flags.

### Usage
- **Check a VM flag**:
  ```bash
  jinfo -flag HeapDumpOnOutOfMemoryError 112688
  ```
  **Output**:
  ```
  -XX:-HeapDumpOnOutOfMemoryError
  ```
- **Enable a VM flag**:
  ```bash
  jinfo -flag +HeapDumpOnOutOfMemoryError 112688
  ```
  **Output**:
  ```
  -XX:+HeapDumpOnOutOfMemoryError
  ```
- **View system properties**:
  ```bash
  jinfo -sysprops 112688 | grep java.runtime
  ```
  **Output**:
  ```
  java.runtime.name=OpenJDK Runtime Environment
  java.runtime.version=24.0.1+9
  ```

### Notes
- Useful for debugging and performance analysis.
- Some properties may not be modifiable at runtime.

## 16. jconsole – JVM Monitoring Console
**Visualize Your Java’s Vitals**

### Purpose
`jconsole` provides a graphical interface for monitoring JVM memory, threads, CPU usage, and JMX beans in real-time.

### Usage
- **Monitor a local JVM**:
  ```bash
  jconsole 112688
  ```
- **Connect to a remote JVM**:
  ```bash
  jconsole localhost:9999
  ```

### Notes
- Allows dynamic management of heap dumps and garbage collection.
- Ideal for real-time performance tracking.

## 17. jcmd – JVM Diagnostic Command Tool
**Talk Directly to the JVM**

### Purpose
`jcmd` sends diagnostic and management commands to running JVMs, consolidating many JDK tools into one command.

### Usage
- **List running JVMs**:
  ```bash
  jcmd
  ```
  **Output**:
  ```
  112688 com.darioscript.jdktools.JdkToolsApplication
  ```
- **List available commands**:
  ```bash
  jcmd 112688 help | grep GC
  ```
  **Output**:
  ```
  GC.heap_dump
  GC.heap_info
  GC.run
  ...
  ```
- **View system properties**:
  ```bash
  jcmd 112688 VM.system_properties
  ```
- **Force garbage collection**:
  ```bash
  jcmd 112688 GC.run
  ```
- **Dump heap**:
  ```bash
  jcmd 112688 GC.heap_dump /tmp/heap.hprof
  ```

### Notes
- A versatile tool for JVM introspection and diagnostics.
- Supports heap dumps, thread dumps, and more.

## 18. jfr – Flight Recorder
**Capture Performance Mid-Flight**

### Purpose
`jfr` collects and analyzes performance and diagnostic data from running Java applications with low overhead.

### Usage
- **View garbage collection data**:
  ```bash
  jfr view gc recording.jfr
  ```
  **Output**:
  ```
  Start       GC ID  Type                     Heap Before GC  Heap After GC  Longest Pause
  --------    -----  ------------------------ --------------  -------------  -------------
  17:43:20    1      Young Garbage Collection  20.0 MB         10.5 MB        2.82 ms
  17:43:20    2      Old Garbage Collection   10.5 MB         11.5 MB        1.48 ms
  17:43:21    3      Young Garbage Collection  40.5 MB         13.1 MB        3.06 ms
  17:43:21    4      Young Garbage Collection  32.1 MB         14.3 MB        3.40 ms
  17:43:21    5      Young Garbage Collection  18.3 MB         14.3 MB        1.87 ms
  17:43:21    6      Old Garbage Collection   14.3 MB         14.3 MB        3.61 ms
  ```
- **View allocation by site**:
  ```bash
  jfr view allocation-by-site recording.jfr
  ```
  **Output**:
  ```
  Method                                              Allocation Pressure
  --------------------------------------------------- -------------------
  java.util.Arrays.copyOf(byte[], int)                30.83%
  java.util.concurrent.ConcurrentHashMap.initTable()  9.53%
  java.util.zip.InflaterInputStream.<init>(...)       7.95%
  ```

### Notes
- Records JVM events like thread activity and garbage collection.
- Use with `-XX:StartFlightRecording:filename=recording.jfr,duration=5m` to generate recordings.

## 19. jhsdb – HotSpot Debugger
**Postmortem for Dead JVMs**

### Purpose
`jhsdb` attaches to running Java processes or analyzes core dumps to diagnose and debug JVM issues.

### Usage
- **Start the interactive GUI debugger**:
  ```bash
  jhsdb hsdb --pid 112688
  ```

### Notes
- Useful for diagnosing JVM crashes and runtime behavior.
- Provides detailed postmortem analysis.

## 20. jmap – Memory Map Tool
**Explore the JVM’s Memory Landscape**

### Purpose
`jmap` displays memory usage, heap data, and class statistics for a Java process, aiding in memory issue diagnosis.

### Usage
- **ClassLoader statistics and metaspace usage**:
  ```bash
  jmap -clstats 112688
  ```
  **Output**:
  ```
  ClassLoader Parent CLD* Classes ChunkSz BlockSz Type
  0x00...     0x00... 0x00... 1       3072    1800    net.bytebuddy.uti
  0x00...     0x00... 0x00... 68      440320  431824  jdk.internal.loade
  0x00...     0x00... 0x00... 1       8192    2856    sun.reflect.misc.M
  0x00...     0x00... 0x00... 3424    9914368 9262392 <boot class loade
  227                 707584  426288  + hidden classes
  0x00...     0x00... 0x00... 0       0       0       org.springframewo
  0x00...     0x00... 0x00... 8689    42333184 42317464 jdk.internal.loade
  0x00...     0x00... 0x00... 0       0       0       org.hibernate.boot
  Total = 7           12410   53406720 52442624
  ChunkSz: Total size of all allocated metaspace chunks
  BlockSz: Total size of all allocated metaspace blocks
  ```

### Notes
- Effective for detecting memory leaks and excessive heap usage.
- Provides detailed memory statistics for troubleshooting.

## 21. jstack – Stack Trace Printer
**Reveal What Your Threads Are Up To**

### Purpose
`jstack` prints stack traces for all threads in a Java process, useful for debugging deadlocks, performance bottlenecks, and stuck threads.

### Usage
- **Print stack traces**:
  ```bash
  jstack 112688
  ```
  **Output**:
  ```
  "RMI Scheduler(0)" #39 [124830] daemon prio=5 os_prio=0 cpu=3.64ms elapsed...
  java.lang.Thread.State: TIMED_WAITING (parking)
      at jdk.internal.misc.Unsafe.park(java.base@24.0.1/Native Method)
  ...
  ```

### Notes
- Essential for diagnosing threading issues.
- Complements other diagnostic tools like `jcmd` and `jconsole`.

## 22. jstat – JVM Statistics Monitor
**JVM Metrics on Command**

### Purpose
`jstat` provides statistical data about JVM performance, such as garbage collection and memory usage.

### Usage
- **Analyze Young Generation usage**:
  ```bash
  jstat -gcnew 112688 60s 10
  ```
  **Output**:
  ```
  S0C    S1C    S0U    S1U    TT MTT DSS   EC      EU      YGC YGCT
  0.0    3072.0 0.0    2604.6 7  15  2048.0 24576.0 3072.0  48  0.107
  0.0    3072.0 0.0    2604.6 7  15  2048.0 24576.0 11264.0 48  0.107
  0.0    3072.0 0.0    2604.6 7  15  2048.0 24576.0 19456.0 48  0.107
  0.0    3072.0 0.0    2504.1 7  15  2048.0 24576.0 1024.0  49  0.109
  0.0    3072.0 0.0    2504.1 7  15  2048.0 24576.0 9216.0  49  0.109
  0.0    3072.0 0.0    2504.1 7  15  2048.0 24576.0 17408.0 49  0.109
  0.0    3072.0 0.0    2543.3 7  15  2048.0 23552.0 2048.0  50  0.111
  0.0    3072.0 0.0    2543.3 7  15  2048.0 23552.0 9216.0  50  0.111
  0.0    3072.0 0.0    2543.3 7  15  2048.0 23552.0 16384.0 50  0.111
  0.0    3072.0 0.0    2405.4 7  15  2048.0 23552.0 2048.0  51  0.113
  ```

### Notes
- Complements `jcmd` and `jconsole` for comprehensive JVM analysis.
- Effective for tracking memory leaks and garbage collection inefficiencies.

## 23. keytool – Keystore Management Tool
**Manage Your Cryptographic Keys**

### Purpose
`keytool` manages keys and certificates in keystores, used for securing Java applications with SSL/TLS, code signing, and encryption.

### Usage
- **Generate a keystore with a self-signed certificate**:
  ```bash
  keytool -genkeypair -alias mykey \
    -keyalg RSA -keysize 4096 -validity 3650 \
    -keystore mykeystore.p12 -storetype PKCS12 \
    -dname "CN=John Doe, O=Example Corp, C=US"
  ```
- **List keystore contents**:
  ```bash
  keytool -list -v -keystore mykeystore.p12
  ```
- **Export a certificate**:
  ```bash
  keytool -export \
    -alias mykey \
    -keystore mykeystore.jks \
    -file mycert.cer
  ```

### Notes
- Essential for securing Java applications.
- Keystores must be securely stored to protect sensitive keys.

## 24. jarsigner – JAR Signing Tool
**Sign with Trust, Run with Confidence**

### Purpose
`jarsigner` digitally signs JAR files and verifies their authenticity, ensuring the integrity of distributed Java applications.

### Usage
- **Sign a JAR**:
  ```bash
  jarsigner -keystore mykeystore.p12 \
    -storetype PKCS12 \
    -signedjar ./newjar.jar ./jdk-tools.jar mykey
  ```
- **Verify a JAR’s signature**:
  ```bash
  jarsigner -verify newjar.jar
  ```

### Notes
- Use timestamping to maintain signature validity after certificate expiration.
- PKCS#12 keystores are recommended for compatibility.
- Any changes to a signed JAR (e.g., manifest) invalidate the signature.

## 25. serialver – Serial Version Tool
**Lock Your Class Versions with Style**

### Purpose
`serialver` computes and displays the `serialVersionUID` for a class, ensuring compatibility during serialization and deserialization.

### Usage
- **Compute serialVersionUID**:
  ```bash
  serialver -classpath target/libs/player.jar com.darioscript.Player
  ```
  **Output**:
  ```
  com.darioscript.Player:
      private static final long serialVersionUID = 9043641422762034699L;
  ```

### Notes
- Prevents `InvalidClassException` during deserialization.
- Useful during refactoring or version control to maintain serialization compatibility.

## 26. rmiregistry – Remote Object Registry
**Register and Find Remote Objects**

### Purpose
`rmiregistry` creates a registry for Java RMI applications, enabling lookup and communication with remote objects.

### Usage
- **Start the RMI registry**:
  ```bash
  rmiregistry 1099
  ```
- **Specify a directory for stub files**:
  ```bash
  rmiregistry -J-Duser.dir=/path/to/dir 1099
  ```

### Notes
- `rmiregistry` Must be running before RMI clients or servers can interact.
- Default port is 1099, but it can be customized.


