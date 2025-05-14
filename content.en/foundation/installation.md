---
title: 'Installation'
weight: 4
---

Java is not bundled with your OS by default—you need to install the **JDK** to start writing and running Java programs.

Here’s how you can set it up manually using `.zip` (Windows) or `.tar.gz` (Linux) files.

---

### **Windows Setup (Using .zip)**

1. **Download the JDK**

   * Go to [https://jdk.java.net](https://jdk.java.net) or your preferred JDK vendor.
   * Download the **.zip** version for Windows.

2. **Extract the JDK**

   * Extract it to a location like:
     `C:\Java\jdk-<Version>`

3. **Set Environment Variables**
   * Open Windows Powershell.
   * Set the value of the environment variable to your JDK (or JRE) installation path as follows:

   ```powershell
   setx JAVA_HOME "C:\Java\jdk-<Version>"
   ```

   ```powershell
   setx PATH "%PATH%;%JAVA_HOME%\bin"
   ```

4. **Verify Installation**

   Open New Command Prompt and run:

   ```cmd
   java -version
   javac -version
   ```

   You should see the installed version printed.

---

### 🐧 **Linux Setup (Using .tar.gz)**

1. **Download the JDK**

   * Visit [https://jdk.java.net](https://jdk.java.net)
   * Download the **.tar.gz** version for Linux.

2. **Extract the JDK**

   ```bash
   tar -xvzf jdk-21_linux-x64_bin.tar.gz
   sudo mv jdk-21 /opt/java/
   ```

3. **Set Environment Variables**

   Edit `~/.bashrc` (or `~/.zshrc` if you're using zsh):

   ```bash
   export JAVA_HOME=/opt/java/jdk-21
   export PATH=$JAVA_HOME/bin:$PATH
   ```

   Then apply the changes:

   ```bash
   source ~/.bashrc
   ```

4. **Verify Installation**

   ```bash
   java -version
   javac -version
   ```

---

Now your JDK setup is ready—on both Windows and Linux!
You're all set to compile and run Java programs like a pro. ☕

Perfect! Let’s keep the momentum going with a simple and beginner-friendly section on writing and running your **first Java program**.

---

### ✍️ Your First Java Program

Now that the JDK is set up, let’s write, compile, and run a simple Java program. This will help confirm everything works and show how the development flow fits together.

---

### 📄 1. Create a Java File

Open a text editor (like Notepad, VS Code, or nano) and write the following:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Java!");
    }
}
```

Save the file as:

```
HelloWorld.java
```

✅ **Note:** The filename *must* match the class name (`HelloWorld`).

---

### 🚀 2. Run the Program

Now run the compiled bytecode using the JVM:

```bash
java HelloWorld.java
```

You should see:

```
Hello, Java!
```

🎉 **That’s it!** You've just written, compiled, and run your first Java program!
