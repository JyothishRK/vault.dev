	
---

# Building and Deploying the Java Server JAR File

This guide explains how to manually build the Java server JAR file and deploy it to your VM.

---

## Prerequisites

### Required Software

1. **Java Development Kit (JDK)**
    
    - **Version:** Java 17 or Java 21 (recommended)
        
    - **Reason:** Gradle 8.10.2 requires Java 17+ for building
        
    - **Note:** The compiled JAR runs on Java 8+ (as per `build.gradle`)
        
2. **Gradle Wrapper**
    
    - Located inside the project folder (`app/gradlew` or `app/gradlew.bat`)
        
    - No separate Gradle installation required
        

### Verify Java Installation

**Windows:**

```powershell
java -version
```

**Linux/Mac:**

```bash
java -version
```

---

## Building the JAR File

### Manual Build (Windows)

**PowerShell:**

```powershell
cd app
$env:JAVA_HOME = "C:\Program Files\Java\jdk-17"
.\gradlew.bat clean build -x test
```

**Command Prompt:**

```cmd
cd app
set JAVA_HOME=C:\Program Files\Java\jdk-17
gradlew.bat clean build -x test
```

### Manual Build (Linux/Mac)

```bash
cd app
export JAVA_HOME=/path/to/jdk-17
chmod +x gradlew
./gradlew clean build -x test
```

### Build Options

- **Skip tests (faster):** `-x test`
    
- **Clean build:** `clean build`
    
- **Full build:** `build` (includes tests)
    

### Expected Output

```
BUILD SUCCESSFUL in Xs
```

**JAR Location:**

```
app/build/libs/ej2-webservices-0.0.14-SNAPSHOT.jar
```

**Details:**

- Type: Spring Boot executable JAR
    
- Size: ~23 MB
    
- Runnable with `java -jar`
    

---

## Using the JAR File

### Running Locally

```bash
java -jar ej2-webservices-0.0.14-SNAPSHOT.jar
```

With JVM memory options:

```bash
java -Xmx512m -Xms256m -jar ej2-webservices-0.0.14-SNAPSHOT.jar
```

Run on a different port:

```bash
java -jar ej2-webservices-0.0.14-SNAPSHOT.jar --server.port=8080
```

Background execution (Linux/Mac):

```bash
nohup java -jar ej2-webservices-0.0.14-SNAPSHOT.jar > server.log 2>&1 &
```

---

## Deployment to VM

### 1. Copy JAR to VM

```bash
scp app/build/libs/ej2-webservices-0.0.14-SNAPSHOT.jar user@vm-ip:/path/to/destination/
```

### 2. SSH into VM

```bash
ssh user@vm-ip
```

### 3. Run the JAR on VM

```bash
cd /path/to/destination
java -jar ej2-webservices-0.0.14-SNAPSHOT.jar
```

---

## Running as a Linux Service (systemd)

Create service file:

`/etc/systemd/system/ej2-webservices.service`

```ini
[Unit]
Description=Syncfusion EJ2 Web Services
After=network.target

[Service]
Type=simple
User=your-user
WorkingDirectory=/path/to/jar
ExecStart=/usr/bin/java -jar /path/to/ej2-webservices-0.0.14-SNAPSHOT.jar
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Enable and start:

```bash
sudo systemctl daemon-reload
sudo systemctl enable ej2-webservices
sudo systemctl start ej2-webservices
sudo systemctl status ej2-webservices
```

---

## Troubleshooting

### Build Issues

#### JAVA_HOME is invalid

Set correct path:

**Windows:**

```powershell
$env:JAVA_HOME = "C:\Program Files\Java\jdk-17"
```

**Linux/Mac:**

```bash
export JAVA_HOME=/path/to/jdk-17
```

#### Unsupported class file major version / Java version mismatch

Possible Gradle cache issue. Clear cache:

**Windows:**

```powershell
Remove-Item -Path "$env:USERPROFILE\.gradle\caches" -Recurse -Force
```

**Linux/Mac:**

```bash
rm -rf ~/.gradle/caches
```

#### Build failed

- Check internet connectivity
    
- Ensure Maven repos accessible
    
- Verify dependency versions in `app/build.gradle`
    

---

### Runtime Issues

#### No main manifest attribute

Use:

```
ej2-webservices-0.0.14-SNAPSHOT.jar
```

(not `-plain.jar`)

#### Port already in use

Change port:

```properties
server.port=8081
```

OR free the port:

```bash
lsof -ti:8080 | xargs kill -9
```

#### OutOfMemoryError

Increase heap:

```bash
java -Xmx1024m -Xms512m -jar ej2-webservices-0.0.14-SNAPSHOT.jar
```

---

## systemd Service Management

View logs:

```bash
sudo journalctl -u ej2-webservices -f
```

Restart:

```bash
sudo systemctl restart ej2-webservices
```

Stop:

```bash
sudo systemctl stop ej2-webservices
```

---

## Project Configuration

### Build Configuration (`app/build.gradle`)

- Spring Boot: **2.7.18**
    
- Java source/target: **1.8**
    
- Gradle: **8.10.2**
    

### App Configuration (`app/src/main/resources/application.properties`)

- Max file size: **100MB**
    
- Max request size: **100MB**
    

---

## Quick Reference

### Build Commands

**Windows**

```powershell
cd app
$env:JAVA_HOME="C:\Program Files\Java\jdk-17"
.\gradlew.bat clean build -x test
```

**Linux/Mac**

```bash
cd app
export JAVA_HOME=/path/to/jdk-17
./gradlew clean build -x test
```

### Run Commands

```bash
java -jar ej2-webservices-0.0.14-SNAPSHOT.jar
java -Xmx512m -jar ej2-webservices-0.0.14-SNAPSHOT.jar
nohup java -jar ej2-webservices-0.0.14-SNAPSHOT.jar > server.log 2>&1 &
```

### Important File Paths

- JAR: `app/build/libs/ej2-webservices-0.0.14-SNAPSHOT.jar`
    
- Gradle Wrapper: `app/gradlew` / `app/gradlew.bat`
    
- Build Config: `app/build.gradle`
    
- App Config: `app/src/main/resources/application.properties`
    

---

## Version History

**0.0.14-SNAPSHOT**

- Gradle 8.10.2
    
- Spring Boot 2.7.18
    
- Java 17+ required for build
    
- Java 8+ for runtime
    

---
