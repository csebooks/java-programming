
### Maven Build Tool for Real Projects

Maintaining complex classpaths and manually compiling files is painful. That’s where **build tools** come in.

Absolutely! Here's a simple and complete guide to help your readers **set up Maven** on their system (both Windows and Linux), followed by verifying the installation.

---

## Maven Setup Instructions

To build and manage Java projects efficiently, Maven is a must-have. Here's how to set it up on your system.

---

### Windows Setup

#### 1. **Download Maven**

Go to the official Apache Maven site:
[https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)

* Download the **binary zip archive** (e.g., `apache-maven-<version>-bin.zip`)
* Extract it to a folder, e.g., `C:\Programs\apache-maven-<version>`

#### 2. **Set Environment Variables**

   * Open Windows Powershell.
   * Set the installation path as follows:

   ```powershell
   setx MAVEN_HOME "C:\Programs\apache-maven-<version>"
   ```

   ```powershell
   setx PATH "%PATH%;%MAVEN_HOME%\bin"
   ```

#### 3. **Verify Installation**

Open Command Prompt and run:

```bash
mvn -v
```

You should see Maven version info.

---

### Linux Setup

#### 1. **Download Maven**

```bash
wget https://downloads.apache.org/maven/maven-3/<version>/binaries/apache-maven-<version>-bin.tar.gz
```

#### 2. **Extract the Archive**

```bash
tar -xvzf apache-maven-<version>-bin.tar.gz
sudo mv apache-maven-<version> /opt/maven
```

#### 3. **Set Environment Variables**

Edit your shell profile (`~/.bashrc`, `~/.zshrc`, etc.) and add:

```bash
export MAVEN_HOME=/opt/maven
export PATH=$MAVEN_HOME/bin:$PATH
```

Apply the changes:

```bash
source ~/.bashrc  # or source ~/.zshrc
```

#### 4. **Verify Installation**

```bash
mvn -v
```

You should see output with the Maven version, Java version, and JAVA\_HOME path.

---

Now Lets add below in `pom.xml`:

### You're Ready

```xml
<!-- pom.xml -->
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>app</artifactId>
  <version>1.0</version>
</project>
```

Run:

```bash
mvn package
```

JAR gets built inside `target/`.