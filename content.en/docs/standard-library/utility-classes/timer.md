---
title: 'Timer and TimerTask'
weight: 6
---


`Timer` and `TimerTask` in `java.util` provide a simple way to **schedule tasks** for one-time execution or repeated execution at fixed intervals.

- `Timer` is the scheduler.
- `TimerTask` is the task to be run.

---

### Scenarios

- Running periodic background tasks (e.g., health checks, status updates).
- Scheduling reminders, timeouts, or cleanup tasks.
- Creating simple delayed execution logic without external libraries.

---

### Sample Usage

```java
import java.util.Timer;
import java.util.TimerTask;

public class TimerExample {
    public static void main(String[] args) {
        Timer timer = new Timer();

        TimerTask task = new TimerTask() {
            public void run() {
                System.out.println("Task executed at: " + System.currentTimeMillis());
            }
        };

        // Schedule the task to run after 2 seconds delay, then repeat every 3 seconds
        timer.scheduleAtFixedRate(task, 2000, 3000);
    }
}
```

### One-time task example

```java
timer.schedule(new TimerTask() {
    public void run() {
        System.out.println("This runs once after 5 seconds.");
    }
}, 5000);
```

### Notes

* For concurrent and more flexible scheduling, use `ScheduledExecutorService` from `java.util.concurrent` in modern Java applications.
* Always `cancel()` the `Timer` when done to free resources.

