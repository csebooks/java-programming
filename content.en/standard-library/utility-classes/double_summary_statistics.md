---
title: 'DoubleSummaryStatistics'
weight: 12
---

### Definition

`DoubleSummaryStatistics` (in `java.util`) is a collector used in stream processing that automatically computes count, sum, min, average, and max of a stream of `double` values.

---

### Scenarios

- When processing numerical data streams (like scores, prices, or measurements).
- Quickly summarizing statistics without manual aggregation.
- Useful in analytics, reporting, and dashboards.

---

### Sample Usage

```java
import java.util.DoubleSummaryStatistics;
import java.util.stream.DoubleStream;

public class SummaryStatsExample {
    public static void main(String[] args) {
        DoubleStream scores = DoubleStream.of(86.5, 90.0, 75.2, 88.8);

        DoubleSummaryStatistics stats = scores.summaryStatistics();

        System.out.println("Count: " + stats.getCount());
        System.out.println("Sum: " + stats.getSum());
        System.out.println("Min: " + stats.getMin());
        System.out.println("Average: " + stats.getAverage());
        System.out.println("Max: " + stats.getMax());
    }
}
```

---

### Output

```
Count: 4
Sum: 340.5
Min: 75.2
Average: 85.125
Max: 90.0
```

---

### Notes

* Also available for `IntSummaryStatistics` and `LongSummaryStatistics`.
* Ideal when using `Stream` API to analyze numeric datasets.
* Eliminates the need to manually track all individual values.

