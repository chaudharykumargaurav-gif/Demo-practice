# minimal-springboot

Minimal Spring Boot project with a REST API endpoint.

## Prerequisites

You need Java JDK 17+ and Apache Maven installed.

### Quick Setup (Windows)

If you see "java is not recognized" or "mvn is not recognized", run the automated setup script:

**PowerShell (run as Administrator):**
```powershell
cd C:\Users\gaura\IdeaProjects\minimal-springboot
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\setup-java-maven.ps1
```

This script will:
- Install Java JDK 17 (Eclipse Temurin) via winget
- Install Apache Maven via winget
- Set JAVA_HOME and add to PATH
- Verify installations

After running the script, **open a NEW PowerShell window** and verify:
```powershell
java -version
mvn -version
```

### Manual Installation (if script fails)

1. **Install Java JDK 17+:**
   ```powershell
   winget install --id EclipseAdoptium.Temurin.17.JDK -e
   ```

2. **Install Maven:**
   ```powershell
   winget install --id Apache.Maven -e
   ```

3. **Set JAVA_HOME (PowerShell as Admin):**
   ```powershell
   $javaPath = (Get-Command java).Path
   $jdkRoot = Split-Path $javaPath -Parent | Split-Path -Parent
   setx JAVA_HOME $jdkRoot -m
   ```

4. **Restart your terminal** and verify: `java -version` and `mvn -version`

## Build and Run

**PowerShell:**
```powershell
cd C:\Users\gaura\IdeaProjects\minimal-springboot

# Build the project
mvn clean package

# Run the application
mvn spring-boot:run
```

Or run the JAR directly:
```powershell
java -jar target\minimal-springboot-0.0.1-SNAPSHOT.jar
```

## Test

Once the application is running, test the endpoint:

**PowerShell:**
```powershell
Invoke-RestMethod http://localhost:8080/hello
```

**Browser:**
Open http://localhost:8080/hello

**Expected response:**
```
Hello, world
```

## Project Structure

```
minimal-springboot/
├── pom.xml                                      # Maven configuration
├── src/main/java/com/example/demo/
│   ├── DemoApplication.java                    # Main Spring Boot application
│   └── HelloController.java                    # REST controller with /hello endpoint
└── src/main/resources/
    └── application.properties                   # App configuration (port 8080)
```

## Troubleshooting (Again making some changes to verify the github account)
### "java is not recognized"
- Run the `setup-java-maven.ps1` script as Administrator
- Or manually install Java and ensure JAVA_HOME is set
- Open a NEW terminal after installation

### "mvn is not recognized" (MAKING ONE MORE CHANGE) checking 
- Run the `setup-java-maven.ps1` script as Administrator
- Or manually install Maven via `winget install --id Apache.Maven -e`
- Open a NEW terminal after installation

### Port 8080 already in use (just adding something this line, nothing functionality)
Change the port in `src/main/resources/application.properties`:
```properties
server.port=8081
```

### For Git Bash users
After running setup script, add to `~/.bashrc`:
```bash
export JAVA_HOME="/c/Program Files/Eclipse Adoptium/jdk-17.x.x.x-hotspot"
export PATH="$JAVA_HOME/bin:$PATH"
```

