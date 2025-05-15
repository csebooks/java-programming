---
title: 'Date'
weight: 3
---

> Dates and times in Java have evolved significantly over the years. Here’s how the main types stack up, why the newer `java.time` API was needed, and when to use each.

---

#### `java.util.Date` & `java.sql.Timestamp`

* **Representation**: Internally holds a `long` of milliseconds since the Unix epoch (January 1, 1970 UTC).
* **Usage**:

  ```java
  Date now = new Date();
  long ms = now.getTime();
  ```
* **Drawbacks**:

  * Mutable and not thread-safe
  * Many of its methods are deprecated (e.g. `getYear()`)
  * Lacks clear separation between date-only, time-only, and timezone data
* **`Timestamp`** extends `Date` to add nanosecond precision for JDBC; primarily used when reading/writing SQL `TIMESTAMP` columns.

---

#### The Modern `java.time` API (Java 8+)

Designed by the team behind Joda-Time, this API fixes the pain points of the old classes. All types are **immutable**, **thread-safe**, and clearly modeled.

---

##### `LocalDate`

* **What it holds**: A date without time or timezone (year-month-day).
* **When to use**: Birthdays, anniversaries, or any date that doesn’t need a time component.

  ```java
  LocalDate today = LocalDate.now();
  LocalDate birthday = LocalDate.of(1990, Month.JULY, 19);
  ```

---

##### `LocalTime`

* **What it holds**: A time-of-day without date or timezone (hour-minute-second).
* **When to use**: Business hours, daily schedules, or any time-only value.

  ```java
  LocalTime meeting = LocalTime.of(14, 30);
  ```

---

##### `LocalDateTime`

* **What it holds**: Combines `LocalDate` and `LocalTime`—still no timezone.
* **When to use**: Logging events on the local machine, user input without zone context.

  ```java
  LocalDateTime appointment = LocalDateTime.of(2025, 5, 20, 9, 0);
  ```

---

##### `ZonedDateTime` & `OffsetDateTime`

* **`ZonedDateTime`**: A date-time with a full timezone (e.g. Europe/Paris).
* **`OffsetDateTime`**: A date-time with a fixed offset from UTC (e.g. +05:30).
* **When to use**: Scheduling across time zones, storing instants in a human-readable form.

  ```java
  ZonedDateTime parisMeeting = ZonedDateTime.of(2025,5,20,14,0,0,0, ZoneId.of("Europe/Paris"));
  ```

---

##### `Instant`

* **What it holds**: A point on the global timeline (UTC), measured in seconds and nanoseconds from the epoch.
* **When to use**: Timestamps for logging, measuring elapsed time, or interoperation with legacy `Date`/`Timestamp`.

  ```java
  Instant now = Instant.now();
  Date legacy = Date.from(now);
  ```

---

##### `Duration` & `Period`

* **`Duration`**: Amount of time in seconds/nanoseconds (time-based).
* **`Period`**: Amount of time in years/months/days (date-based).
* **When to use**:

  * `Duration` for measuring spans like “5 hours, 30 minutes.”
  * `Period` for calendar-based spans like “3 years, 2 months.”

  ```java
  Duration flight = Duration.ofHours(8).plusMinutes(15);
  Period age = Period.between(birthday, today);
  ```

---

### Why the New API?

1. **Clarity** – Clear separation of date-only vs. time-only vs. combined vs. zoned types.
2. **Immutability** – Thread-safe by design, no surprises from shared mutable state.
3. **Fluent API** – Methods like `plusDays()`, `withMonth()`, or `truncatedTo()` make transformations easy to read.
4. **Interoperability** – Built-in converters to/from legacy types (`Date`, `Calendar`, `Timestamp`).
5. **Powerful Parsing/Formatting** – `DateTimeFormatter` with ISO standards and custom patterns.

---

By choosing the right type—`LocalDate` for dates, `LocalTime` for times, `ZonedDateTime` for zone-aware moments, and `Duration`/`Period` for spans—you’ll write code that’s clearer, safer, and less error-prone.
