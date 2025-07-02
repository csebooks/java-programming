---
title: 'Networking'
weight: 8
categories:
    - network
---

### Definition

Java provides built-in support for networking through its `java.net` package, enabling communication over TCP/IP protocols. This allows developers to write client-server applications, access web services, and manage sockets and URLs directly.

---

### Modules Involved

- `java.net` – Core networking APIs (Socket, ServerSocket, URL, HttpURLConnection)
- `java.nio.channels` – Non-blocking network channels
- `java.net.http` – Modern HTTP Client API (since Java 11)

---

### Core Concepts and APIs

#### 1. URL and HttpURLConnection

Used for accessing resources over HTTP and other protocols.

```java
URL url = new URL("https://example.com");
HttpURLConnection con = (HttpURLConnection) url.openConnection();
con.setRequestMethod("GET");

int status = con.getResponseCode();
System.out.println("Status Code: " + status);
````

#### 2. Modern HTTP Client (Java 11+)

A more modern and asynchronous HTTP client built on top of `java.net`.

```java
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://example.com"))
    .build();

HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
System.out.println(response.body());
```

#### 3. Socket Programming (TCP)

For building lower-level client-server applications.

**Server Side:**

```java
ServerSocket server = new ServerSocket(8080);
Socket client = server.accept();
InputStream in = client.getInputStream();
System.out.println("Client connected");
```

**Client Side:**

```java
Socket socket = new Socket("localhost", 8080);
OutputStream out = socket.getOutputStream();
out.write("Hello".getBytes());
```

#### 4. DatagramSocket (UDP)

For connectionless communication with lower overhead.

```java
DatagramSocket socket = new DatagramSocket();
InetAddress address = InetAddress.getByName("localhost");
byte[] buffer = "Ping".getBytes();
DatagramPacket packet = new DatagramPacket(buffer, buffer.length, address, 9999);
socket.send(packet);
```

---

### Use Cases

* Building REST clients and calling web APIs
* Developing web servers and TCP/UDP-based services
* Real-time communication applications like chat, multiplayer games
* IoT and device communication

---

Java's networking capabilities are broad — from simple HTTP calls to full-blown socket servers. The modern `HttpClient` makes HTTP-based communication much easier and robust, while low-level sockets are still powerful tools for custom protocols.

