---
title: 'Stream API'
weight: 3
categories:
    - stream-api
    
references:
    videos:
        - youtube:v_5SKpfkI2U
    links:
        - https://dev.java/learn/api/streams/
        - https://www.danvega.dev/blog/stream-gatherers

---

> The Collections Framework is about storing and organizing your data in the memory of your JVM. You can see the Stream API as a companion framework to the Collections Framework, to process this data in a very efficient way. Indeed, you can open a stream on a collection to process the data it contains.Stream API provides a functional approach to processing collections of objects.

A stream is an object that does not store any data it's operations are divided into two categories:

1. **Intermediate operations** (like `filter`, `map`, and `sorted`) that return a new stream
2. **Terminal operations** (like `collect`, `forEach`, and `reduce`) that produce a result

```java
List.of("James", "Cameron", "Jhon", "Wick")
    .stream()    // Create a stream from names collection
        .filter(name -> name.startsWith("J")) // Intermediate operation
    .collect(Collectors.toList());        // Terminal operation
```

## Note 

1. You are allowed to call only one method on a stream, even if this method is intermediate. So it is useless, and sometimes dangerous, to store streams in fields or local variables. Writing methods that take streams as arguments may also be dangerous, because you cannot be sure that the stream you receive has not been already operated upon. A stream should be created and consumed on the spot.

2. A stream is an object connected to a source. It pulls the elements it processes from this source. This source should not be modified by the stream itself. Doing so will lead to unspecified results. In some cases, this source is immutable or read-only, so you will not be able to do that, but there are cases where you could.

## Stream Gatherers

> Stream Gatherers let us create custom intermediate operations to handle complex transformations more elegantly.

A Gatherer consists of four key components:

1. **Initializer**: Creates the initial state (HashMap in our example)
2. **Integrator**: Processes each element and updates the state
3. **Combiner**: Combines states when processing in parallel
4. **Finisher**: Processes the final state and emits results


Let's look at some fundamental Gatherer patterns that come built-in with JDK.

Lets consdier we have a blog post

```java
public record BlogPost(
        Long id,
        String title,
        String author,
        String content,
        String category,
        LocalDateTime publishedDate
) {}
```
And below are the sample blog posts

```java
private static List<BlogPost> createSampleBlogPosts() {
    return List.of(
            // Java Category
            new BlogPost(
                    1L,
                    "Getting Started with JDK 24 Stream Gatherers",
                    "John Doe",
                    "Learn how to use the exciting new Stream Gatherers feature in JDK 24 to process data more efficiently.",
                    "Java",
                    LocalDateTime.of(2025, 3, 18, 10, 15)),

            new BlogPost(
                    2L,
                    "Virtual Threads in JDK 24: Performance Analysis",
                    "Jane Smith",
                    "A deep dive into the performance improvements of virtual threads in JDK 24 compared to previous versions.",
                    "Java",
                    LocalDateTime.of(2025, 3, 15, 14, 30)),

            new BlogPost(
                    3L,
                    "Mastering Java Records for Clean Code",
                    "Alex Johnson",
                    "How to use Java Records effectively to reduce boilerplate and write more maintainable code.",
                    "Java",
                    LocalDateTime.of(2025, 3, 10, 9, 45)),

            new BlogPost(
                    4L,
                    "Pattern Matching for Switch: JDK 24 Updates",
                    "Sarah Williams",
                    "Explore the latest enhancements to pattern matching for switch expressions and statements in JDK 24.",
                    "Java",
                    LocalDateTime.of(2025, 3, 5, 16, 20)),

            new BlogPost(
                    5L,
                    "Java Memory Management Tuning Tips",
                    "Michael Brown",
                    "Expert advice on tuning JVM memory settings for optimal performance in production environments.",
                    "Java",
                    LocalDateTime.of(2025, 2, 28, 13, 10)),

            // Spring Category
            new BlogPost(
                    6L,
                    "Building Reactive APIs with Spring WebFlux",
                    "Emily Chen",
                    "A comprehensive guide to building scalable reactive APIs using Spring WebFlux and Project Reactor.",
                    "Spring",
                    LocalDateTime.of(2025, 3, 17, 11, 5)),

            new BlogPost(
                    7L,
                    "Spring Boot 3.2 New Features Overview",
                    "David Wilson",
                    "Discover what's new in Spring Boot 3.2 and how to leverage these features in your applications.",
                    "Spring",
                    LocalDateTime.of(2025, 3, 12, 15, 40)),

            new BlogPost(
                    8L,
                    "Secure Authentication with Spring Security 7",
                    "Olivia Martinez",
                    "Implementing robust authentication mechanisms using the latest Spring Security features.",
                    "Spring",
                    LocalDateTime.of(2025, 3, 8, 10, 0)),

            new BlogPost(
                    9L,
                    "Spring Data JPA Best Practices",
                    "Daniel Lee",
                    "Tips and tricks for efficient database access with Spring Data JPA repositories.",
                    "Spring",
                    LocalDateTime.of(2025, 3, 1, 9, 30)),

            new BlogPost(
                    10L,
                    "Microservices Orchestration with Spring Cloud",
                    "Sophia Garcia",
                    "How to use Spring Cloud to build and manage a resilient microservices architecture.",
                    "Spring",
                    LocalDateTime.of(2025, 2, 22, 14, 15)),

            // Architecture Category
            new BlogPost(
                    11L,
                    "Event-Driven Architecture Patterns",
                    "James Taylor",
                    "A deep dive into event-driven architecture patterns and their implementation in modern systems.",
                    "Architecture",
                    LocalDateTime.of(2025, 3, 16, 16, 50)),

            new BlogPost(
                    12L,
                    "Domain-Driven Design in Practice",
                    "Emma White",
                    "Real-world examples of applying Domain-Driven Design principles to complex business domains.",
                    "Architecture",
                    LocalDateTime.of(2025, 3, 11, 11, 25)),

            new BlogPost(
                    13L,
                    "Migrating Monoliths to Microservices",
                    "Ryan Miller",
                    "A step-by-step guide to breaking down monolithic applications into microservices.",
                    "Architecture",
                    LocalDateTime.of(2025, 3, 6, 13, 45)),

            new BlogPost(
                    14L,
                    "API Design Best Practices",
                    "Isabella Clark",
                    "Principles and guidelines for designing robust, scalable, and developer-friendly APIs.",
                    "Architecture",
                    LocalDateTime.of(2025, 2, 25, 9, 20)),

            // Database Category
            new BlogPost(
                    15L,
                    "PostgreSQL Performance Tuning Guide",
                    "Noah Anderson",
                    "Advanced techniques for optimizing PostgreSQL database performance in high-load environments.",
                    "Database",
                    LocalDateTime.of(2025, 3, 19, 10, 30)),

            new BlogPost(
                    16L,
                    "MongoDB Schema Design Strategies",
                    "Ava Thomas",
                    "Best practices for designing efficient and scalable MongoDB schemas for various use cases.",
                    "Database",
                    LocalDateTime.of(2025, 3, 14, 14, 45)),

            new BlogPost(
                    17L,
                    "Redis Caching Patterns for Java Applications",
                    "William Turner",
                    "Implementing effective caching strategies using Redis in Java-based applications.",
                    "Database",
                    LocalDateTime.of(2025, 3, 9, 11, 15)),

            new BlogPost(
                    18L,
                    "Graph Databases: Neo4j vs. JanusGraph",
                    "Sofia Parker",
                    "A comparative analysis of Neo4j and JanusGraph for different graph database use cases.",
                    "Database",
                    LocalDateTime.of(2025, 3, 4, 15, 35)),

            // DevOps Category
            new BlogPost(
                    19L,
                    "GitOps Workflow with Kubernetes and ArgoCD",
                    "Lucas Scott",
                    "Implementing a GitOps approach to continuous delivery using Kubernetes and ArgoCD.",
                    "DevOps",
                    LocalDateTime.of(2025, 3, 20, 9, 10)),

            new BlogPost(
                    20L,
                    "Infrastructure as Code with Terraform",
                    "Mia Rodriguez",
                    "Best practices for managing infrastructure as code using Terraform across cloud providers.",
                    "DevOps",
                    LocalDateTime.of(2025, 3, 13, 13, 25)),

            new BlogPost(
                    21L,
                    "Monitoring Java Applications with Prometheus and Grafana",
                    "Benjamin Walker",
                    "Setting up comprehensive monitoring for Java applications using Prometheus metrics and Grafana dashboards.",
                    "DevOps",
                    LocalDateTime.of(2025, 3, 7, 16, 40)),

            new BlogPost(
                    22L,
                    "Docker Optimization Techniques",
                    "Charlotte Hall",
                    "Tips and tricks for creating smaller, faster, and more secure Docker images for your applications.",
                    "DevOps",
                    LocalDateTime.of(2025, 3, 2, 10, 50)),

            // Testing Category
            new BlogPost(
                    23L,
                    "Property-Based Testing in Java",
                    "Henry Young",
                    "Using property-based testing libraries to automatically generate test cases and find edge cases.",
                    "Testing",
                    LocalDateTime.of(2025, 3, 17, 15, 5)),

            new BlogPost(
                    24L,
                    "End-to-End Testing with Selenium and Cucumber",
                    "Lily Adams",
                    "Building maintainable end-to-end tests using Selenium WebDriver and Cucumber BDD.",
                    "Testing",
                    LocalDateTime.of(2025, 3, 11, 9, 15)),

            new BlogPost(
                    25L,
                    "Test-Driven Development by Example",
                    "Andrew Turner",
                    "A practical guide to applying Test-Driven Development principles in real-world projects.",
                    "Testing",
                    LocalDateTime.of(2025, 3, 3, 14, 55)),

            // Web Development Category
            new BlogPost(
                    26L,
                    "Modern Frontend Development with React 19",
                    "Zoe Campbell",
                    "Exploring the latest features and best practices in React 19 for building interactive UIs.",
                    "Web Development",
                    LocalDateTime.of(2025, 3, 19, 13, 40)),

            new BlogPost(
                    27L,
                    "Progressive Web Apps in 2025",
                    "Nathan Brown",
                    "How to leverage the latest browser capabilities to build fast, reliable progressive web apps.",
                    "Web Development",
                    LocalDateTime.of(2025, 3, 14, 10, 25)),

            new BlogPost(
                    28L,
                    "CSS Architecture for Large Applications",
                    "Audrey Martin",
                    "Strategies for organizing and maintaining CSS in large-scale web applications.",
                    "Web Development",
                    LocalDateTime.of(2025, 3, 8, 15, 20)),

            // Performance Category
            new BlogPost(
                    29L,
                    "JVM Profiling Tools Comparison",
                    "Owen Allen",
                    "A detailed comparison of various profiling tools for diagnosing performance issues in JVM-based applications.",
                    "Performance",
                    LocalDateTime.of(2025, 3, 15, 11, 35)),

            new BlogPost(
                    30L,
                    "Web Application Performance Optimization",
                    "Grace Wilson",
                    "Techniques for improving the loading speed and runtime performance of web applications.",
                    "Performance",
                    LocalDateTime.of(2025, 3, 10, 14, 5)),

            new BlogPost(
                    31L,
                    "Memory Leak Detection in Java Applications",
                    "Ethan Davis",
                    "How to identify, diagnose, and fix memory leaks in Java applications using modern tools.",
                    "Performance",
                    LocalDateTime.of(2025, 3, 5, 9, 50)),

            // Security Category
            new BlogPost(
                    32L,
                    "OWASP Top 10 for Java Developers",
                    "Scarlett Thompson",
                    "Understanding and mitigating the OWASP Top 10 security risks in Java applications.",
                    "Security",
                    LocalDateTime.of(2025, 3, 18, 15, 30)),

            new BlogPost(
                    33L,
                    "Secure Coding Practices for REST APIs",
                    "Sebastian Wright",
                    "Essential security practices for building robust and secure REST APIs.",
                    "Security",
                    LocalDateTime.of(2025, 3, 12, 11, 55)),

            new BlogPost(
                    34L,
                    "Authentication with OAuth 2.1 and OpenID Connect",
                    "Victoria Nelson",
                    "Implementing modern authentication flows using OAuth 2.1 and OpenID Connect protocols.",
                    "Security",
                    LocalDateTime.of(2025, 3, 7, 10, 40)),

            new BlogPost(
                    35L,
                    "Securing Microservices Architecture",
                    "Thomas Harris",
                    "Strategies for implementing security in distributed microservices-based systems.",
                    "Security",
                    LocalDateTime.of(2025, 3, 2, 15, 15)),

            new BlogPost(
                    36L,
                    "Stream Gatherers: A Complete Tutorial",
                    "John Doe",
                    "This comprehensive tutorial walks through all aspects of Stream Gatherers in JDK 24 with practical examples.",
                    "Java",
                    LocalDateTime.of(2025, 3, 19, 9, 30)),

            new BlogPost(
                    37L,
                    "Comparing Stream Collectors and Gatherers",
                    "Sarah Williams",
                    "Learn how Stream Gatherers in JDK 24 improve upon the existing Collectors API with examples of both approaches.",
                    "Java",
                    LocalDateTime.of(2025, 3, 16, 14, 15))
    );
}
```

### Fixed Window

A fixed window gatherer processes elements in non-overlapping groups of a fixed size:

```java
public static void fixedWindowExample(List<BlogPost> posts) {
    // Group posts into batches of 3
    System.out.println("Posts in batches of 3:");
    posts.stream()
            .limit(9) // Limit to 9 posts for clarity
            .gather(Gatherers.windowFixed(3))
            .forEach(batch -> {
                System.out.println("\nBatch:");
                batch.forEach(post -> System.out.println("  - " + post.title()));
            });
}
```


### Sliding Window

A sliding window moves through the elements one at a time:

```java
public static void slidingWindowExample(List<BlogPost> posts) {
    // Show posts in sliding windows of size 2
    System.out.println("Posts in sliding windows of size 2:");
    posts.stream()
            .limit(5) // Limit to 5 posts for clarity
            .gather(Gatherers.windowSliding(2))
            .forEach(window -> {
                System.out.println("\nWindow:");
                window.forEach(post -> System.out.println("  - " + post.title()));
            });
}
```

### Fold
The fold operation accumulates results:

```java
public static void foldExample(List<BlogPost> posts) {
    // Concatenate all blog post titles
    posts.stream()
            .limit(5) // Limit to 5 posts for clarity
            .gather(Gatherers.fold(
                    () -> "All titles: ",
                    (result, post) -> result + post.title() + ", "
            ))
            .findFirst()
            .ifPresent(System.out::println);
}
```

### Scan
The scan operation is similar to fold but emits intermediate results:

```java
public static void scanExample(List<BlogPost> posts) {
    // Build a progressive summary of post titles
    System.out.println("Progressive title concatenation:");
    posts.stream()
            .limit(5) // Limit to 5 posts for clarity
            .gather(Gatherers.scan(
                    () -> "Titles so far: ",
                    (result, post) -> result + post.title() + ", "
            ))
            .forEach(System.out::println);
}
```

### Custom Gatherer

Now let's solve our "recent posts by category" problem using a custom Gatherer

```java
public static <K> Gatherer<BlogPost, Map<K, List<BlogPost>>, Map.Entry<K, List<BlogPost>>> groupByWithLimit(
        Function<? super BlogPost, ? extends K> keyExtractor,
        int limit,
        Comparator<? super BlogPost> comparator) {

    return Gatherer.of(
            // Initialize with an empty map to store our grouped items
            HashMap<K, List<BlogPost>>::new,

            // Process each blog post
            (map, post, downstream) -> {
                // Get the key for this blog post (e.g., the category)
                K key = keyExtractor.apply(post);

                // Add this post to its group (creating the group if needed)
                map.computeIfAbsent(key, k -> new ArrayList<>()).add(post);

                // Continue processing the stream
                return true;
            },

            // Combiner for parallel streams - just use the first map in this simple case
            (map1, map2) -> map1,

            // When all posts have been processed, emit the results
            (map, downstream) -> {
                map.forEach((key, posts) -> {
                    // Sort the posts and limit to the specified number
                    List<BlogPost> limitedPosts = posts.stream()
                            .sorted(comparator)
                            .limit(limit)
                            .toList();

                    // Emit a Map.Entry with the key and limited posts
                    downstream.push(Map.entry(key, limitedPosts));
                });
            }
    );
}
```

Using this custom Gatherer, our solution becomes much cleaner:

```java
public static void groupByWithLimit(List<BlogPost> posts) {
    // Use our custom gatherer to create a "Recent Posts By Category" view
    Map<String, List<BlogPost>> recentPostsByCategory = posts.stream()
            .gather(groupByWithLimit(
                    BlogPost::category,    // Group by category
                    3,                     // Show only 3 recent posts per category
                    Comparator.comparing(BlogPost::publishedDate).reversed() // Newest first
            ))
            .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));

    printRecentPostsByCategory(recentPostsByCategory);
}
```