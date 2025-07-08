---
title: 'Watch Service'
weight: 4
---

> The **Watch Service API** in Java (from `java.nio.file`) allows a program to monitor a directory for **file system events** such as creation, deletion, or modification of files. It provides a non-blocking, event-driven way to react to changes in the file system, making it ideal for tasks like auto-reload, logging, and file synchronization.


### **Common Use Cases:**

* Live reload of configuration files
* Triggering actions on new file uploads
* Monitoring directories for backup or sync
* Watching temp/log folders to clean up files
* Building custom file system observers

---

###  **Sample Usage: Watch for File Events**

```java
import java.io.IOException;
import java.nio.file.*;

public class WatchServiceExample {
    public static void main(String[] args) throws IOException, InterruptedException {
        Path pathToWatch = Paths.get("your/directory");
        WatchService watchService = FileSystems.getDefault().newWatchService();

        pathToWatch.register(
            watchService,
            StandardWatchEventKinds.ENTRY_CREATE,
            StandardWatchEventKinds.ENTRY_DELETE,
            StandardWatchEventKinds.ENTRY_MODIFY
        );

        System.out.println("Watching directory: " + pathToWatch);

        while (true) {
            WatchKey key = watchService.take(); // blocks until an event occurs

            for (WatchEvent<?> event : key.pollEvents()) {
                WatchEvent.Kind<?> kind = event.kind();
                Path changed = (Path) event.context();
                System.out.println("Event kind: " + kind + " - File affected: " + changed);
            }

            boolean valid = key.reset();
            if (!valid) {
                break; // Exit if directory is inaccessible
            }
        }
    }
}
```

---

###  **Key Concepts:**

| Component                 | Purpose                                     |
| ------------------------- | ------------------------------------------- |
| `WatchService`            | Core service to monitor file system events  |
| `WatchKey`                | Represents a registration and tracks events |
| `WatchEvent.Kind<?>`      | Type of event (create, modify, delete)      |
| `StandardWatchEventKinds` | Constants for supported event types         |

---

###  **Limitations:**

* Works **only on directories**, not individual files
* Might miss rapid successive events (implementation-dependent)
* Platform-specific limitations may apply (e.g., symbolic links)
