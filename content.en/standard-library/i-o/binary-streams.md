---
title: 'Binary streams'
weight: 2
categories:
    - binary-streams
---

> Byte streams are used for **raw binary data**, such as images, audio files, and other non-text files.

* **InputStream** – Abstract class for reading byte streams.
* **OutputStream** – Abstract class for writing byte streams.

#### Common Implementations:

* `FileInputStream` / `FileOutputStream` – Read/write bytes from/to files.
* `BufferedInputStream` / `BufferedOutputStream` – Buffer for performance.
* `DataInputStream` / `DataOutputStream` – Read/write primitive types in binary.



```java
try (FileInputStream in = new FileInputStream("image.jpg");
     FileOutputStream out = new FileOutputStream("copy.jpg")) {
    byte[] buffer = new byte[1024];
    int bytesRead;
    while ((bytesRead = in.read(buffer)) != -1) {
        out.write(buffer, 0, bytesRead);
    }
}
```

## Serialization

> **Serialization** is the process of converting a **Java object into a stream of bytes**, so that it can be:

* Saved to a file
* Sent over a network
* Stored in memory (e.g., caching)

The **reverse process** is called **Deserialization**—reconstructing the object from the byte stream.


## Why Serialization?

* Save program state
* Share objects between systems
* Remote Method Invocation (RMI)
* Java Messaging, Caching, Distributed Systems

Java provides built-in serialization via the `java.io.Serializable` marker interface.

```java
public class MyClass implements Serializable {
    // no methods to implement — it's a marker interface
}
```


```java
import java.io.*;

// 1. Create a Serializable class
class Person implements Serializable {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String toString() {
        return name + " (" + age + ")";
    }
}

public class SerializeDemo {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        Person person = new Person("Alice", 30);

        // 2. Serialize object to a file
        try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("person.ser"))) {
            out.writeObject(person);
        }

        // 3. Deserialize object from file
        Person deserialized;
        try (ObjectInputStream in = new ObjectInputStream(new FileInputStream("person.ser"))) {
            deserialized = (Person) in.readObject();
        }

        System.out.println("Deserialized: " + deserialized);
    }
}
```

### Output

```shell
Deserialized: Alice (30)
```

## Notes

* `serialVersionUID` should be defined for version control:

  ```java
  private static final long serialVersionUID = 1L;
  ```

* **Transient fields** are **not serialized**:

  ```java
  private transient String password;
  ```

* Avoid serializing sensitive data directly.



```goat
[Person Object]
    |
    v
ObjectOutputStream ---> person.ser (binary file)
    ^
    |
ObjectInputStream <--- Deserialize from file
```

## When Not to Use Serialization

* For large-scale systems (prefer JSON, Protobuf, etc.)
* When you want full cross-language compatibility
* When you need complete control over data format

### Externalizable

`> Externalizable` is a Java interface that **gives you full control** over how an object is serialized and deserialized.

Unlike `Serializable` (which uses **default serialization**), `Externalizable` requires you to **explicitly implement** how your object's data is written to and read from a stream.

### Interface Definition

```java
public interface Externalizable extends Serializable {
    void writeExternal(ObjectOutput out) throws IOException;
    void readExternal(ObjectInput in) throws IOException, ClassNotFoundException;
}
```

### Key Points

| Feature                        | `Serializable`              | `Externalizable`                          |
| ------------------------------ | --------------------------- | ----------------------------------------- |
| Control over serialization     | ❌ No (Java handles it)      | ✅ Yes (you control everything)            |
| Requires manual implementation | ❌ No                        | ✅ Yes (`writeExternal`, `readExternal`)   |
| Performance                    | 🟡 Slower due to reflection | ✅ Potentially faster                      |
| Used for                       | Simplicity                  | Custom format, partial fields, versioning |


### Example

```java
import java.io.*;

class User implements Externalizable {
    private String name;
    private int age;

    public User() {
        // Required public no-arg constructor
    }

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
        out.writeUTF(name);
        out.writeInt(age);
    }

    @Override
    public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
        name = in.readUTF();
        age = in.readInt();
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}

public class ExternalizableDemo {
    public static void main(String[] args) throws Exception {
        User user = new User("Sathish", 28);

        try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("user.ser"))) {
            out.writeObject(user);
        }

        User restored;
        try (ObjectInputStream in = new ObjectInputStream(new FileInputStream("user.ser"))) {
            restored = (User) in.readObject();
        }

        System.out.println("Deserialized: " + restored);
    }
}
```

---

### Important Notes

* You **must provide a public no-argument constructor** — otherwise deserialization will fail.
* You’re responsible for **writing and reading fields in the same order**.
* Only fields you explicitly write are persisted.


```goat
[User Object]
    |
    v
writeExternal(ObjectOutput out)
    -> user.ser
    <- readExternal(ObjectInput in)
[Reconstructed User Object]
```

### Use `Externalizable` When:

* You want **fine-grained control** over what is serialized (e.g., skip cache-heavy fields)
* You need **backward compatibility** or **custom formats**
* You care about **performance and space**


