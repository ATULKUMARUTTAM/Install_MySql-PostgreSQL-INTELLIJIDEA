# Installation Guide for MySQL, PostgreSQL, and IntelliJ IDEA Community Edition for Spring Boot Projects

This guide provides detailed, step-by-step instructions for installing **MySQL**, **PostgreSQL**, and **IntelliJ IDEA Community Edition** on **Windows** and **macOS**, tailored for setting up a development environment for **Spring Boot** projects. It is designed for researchers, developers, and beginners, including verification steps, troubleshooting tips, and configuration for a Spring Boot project with database connectivity.

## Prerequisites

- **Operating System**: Windows 10/11 (64-bit) or macOS 10.15 (Catalina) or later.
- **Admin/Root Privileges**: Required for installing software and modifying system settings.
- **Internet Connection**: Necessary for downloading installers and packages.
- **Disk Space**: At least 10 GB free for databases and IDE.
- **Memory**: 8 GB RAM minimum (16 GB recommended for development).
- **Existing Tools**: Assumes JDK 21, Maven, and Docker are installed (see [previous guide](#) for details).
- **Windows-Specific**: Windows Subsystem for Linux (WSL2) recommended for Docker integration.
- **macOS-Specific**: Rosetta 2 for Apple Silicon (M1/M2) if running non-native software.

---

## Windows Installation

### Step 1: Install MySQL

**MySQL** is a popular relational database for Spring Boot applications.

1. **Download MySQL Installer**:
   - Visit: [MySQL Community Downloads](https://dev.mysql.com/downloads/installer/)
   - Download the **MySQL Installer for Windows** (e.g., `mysql-installer-community-<version>.msi`).
2. **Run the Installer**:
   - Open the downloaded `.msi` file.
   - Choose **Developer Default** setup type (includes MySQL Server, Workbench, and connectors).
   - Follow the wizard, accepting defaults unless specific requirements exist.
   - Set a root password (e.g., `password`) and note it for later use.
   - Configure MySQL to run as a Windows service (default).
3. **Add MySQL to PATH** (optional, for command-line access):
   ```powershell
   [System.Environment]::SetEnvironmentVariable("Path", "$($Env:Path);C:\Program Files\MySQL\MySQL Server 8.0\bin", "User")
   ```
4. **Verify Installation**:
   - Open PowerShell (as admin, `Win + S`, type `PowerShell`, select **Run as administrator**):
     ```powershell
     mysql --version
     ```
   - **Expected Output**: Version details (e.g., `mysql  Ver 8.0.35 for Win64`).
   - Log in to MySQL:
     ```powershell
     mysql -u root -p
     ```
     - Enter the root password.
     - Run: `SHOW DATABASES;`
     - **Expected Output**: List of default databases (e.g., `information_schema`).
   - **If it fails**, ensure MySQL service is running (`services.msc`, check `MySQL80`) and the PATH is set correctly.

### Step 2: Install PostgreSQL

**PostgreSQL** is an advanced open-source relational database.

1. **Download PostgreSQL Installer**:
   - Visit: [PostgreSQL Downloads](https://www.postgresql.org/download/windows/)
   - Download the **Windows x86-64** installer for PostgreSQL 16 (e.g., `postgresql-16.1-1-windows-x64.exe`).
2. **Run the Installer**:
   - Open the downloaded `.exe` file.
   - Follow the wizard, accepting defaults or customizing the installation directory (e.g., `C:\Program Files\PostgreSQL\16`).
   - Set a superuser password (e.g., `postgres`) and note it.
   - Use default port `5432` unless conflicts exist.
   - Include **pgAdmin 4** (GUI tool) and **Stack Builder** (optional).
3. **Add PostgreSQL to PATH** (optional):
   ```powershell
   [System.Environment]::SetEnvironmentVariable("Path", "$($Env:Path);C:\Program Files\PostgreSQL\16\bin", "User")
   ```
4. **Verify Installation**:
   ```powershell
   psql --version
   ```
   - **Expected Output**: Version details (e.g., `psql (PostgreSQL) 16.1`).
   - Log in to PostgreSQL:
     ```powershell
     psql -U postgres
     ```
     - Enter the superuser password.
     - Run: `\l`
     - **Expected Output**: List of databases (e.g., `postgres`, `template0`).
   - **If it fails**, ensure PostgreSQL service is running (`services.msc`, check `postgresql-x64-16`) and the PATH is set.

### Step 3: Install IntelliJ IDEA Community Edition

**IntelliJ IDEA Community Edition** is a free IDE for Java and Spring Boot development.

1. **Download IntelliJ IDEA**:
   - Visit: [IntelliJ IDEA Downloads](https://www.jetbrains.com/idea/download/)
   - Download the **Community Edition** for Windows (e.g., `ideaIC-<version>.exe`).
2. **Run the Installer**:
   - Open the downloaded `.exe` file.
   - Follow the wizard, accepting defaults or customizing the installation directory.
   - Check options to:
     - Create a desktop shortcut.
     - Add to PATH (optional).
     - Associate `.java` files with IntelliJ.
   - Install and launch IntelliJ IDEA.
3. **Verify Installation**:
   - Open IntelliJ IDEA from the Start menu or desktop shortcut.
   - Check the welcome screen or create a new project to confirm it runs.
   - **If it fails**, ensure JDK 21 is installed (`java -version`) and retry the installation.

### Troubleshooting (Windows)

- **MySQL Installation Fails**: Check internet connectivity or reinstall with a lower version (e.g., 8.0.34) if compatibility issues arise.
- **PostgreSQL Connection Issues**: Verify port `5432` is free (`netstat -aon | findstr 5432`) and the service is running.
- **IntelliJ Fails to Start**: Ensure JDK is set in IntelliJ (`File > Project Structure > SDKs`) and at least 8 GB RAM is available.
- **Database Not Accessible**: Check firewall settings (`Windows Defender Firewall > Allow an app`) to allow MySQL (`3306`) and PostgreSQL (`5432`).

---

## macOS Installation

### Step 1: Install MySQL

1. **Install MySQL via Homebrew** (recommended):
   - Open Terminal (`Cmd + T` or Spotlight search `terminal`).
   - Ensure Homebrew is installed (see previous guide or run):
     ```bash
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
     ```
   - Install MySQL:
     ```bash
     brew install mysql
     ```
   - Start MySQL service:
     ```bash
     brew services start mysql
     ```
   - Secure MySQL installation (set root password, e.g., `password`):
     ```bash
     mysql_secure_installation
     ```
2. **Verify Installation**:
   ```bash
   mysql --version
   ```
   - **Expected Output**: Version details (e.g., `mysql  Ver 8.0.35`).
   - Log in to MySQL:
     ```bash
     mysql -u root -p
     ```
     - Enter the root password.
     - Run: `SHOW DATABASES;`
     - **Expected Output**: List of default databases.
   - **If it fails**, ensure MySQL service is running (`brew services list`) and restart if needed (`brew services restart mysql`).

### Step 2: Install PostgreSQL

1. **Install PostgreSQL via Homebrew**:
   ```bash
   brew install postgresql@16
   ```
   - Installs to `/opt/homebrew/Cellar/postgresql@16/<version>` (Apple Silicon) or `/usr/local/Cellar/postgresql@16/<version>` (Intel).
   - Start PostgreSQL service:
     ```bash
     brew services start postgresql@16
     ```
   - Initialize the default database with a superuser password (e.g., `postgres`):
     ```bash
     initdb -U postgres --auth-host=scram-sha-256 --pwfile=<(echo "postgres")
     ```
2. **Verify Installation**:
   ```bash
   psql --version
   ```
   - **Expected Output**: Version details (e.g., `psql (PostgreSQL) 16.1`).
   - Log in to PostgreSQL:
     ```bash
     psql -U postgres
     ```
     - Enter the superuser password.
     - Run: `\l`
     - **Expected Output**: List of databases.
   - **If it fails**, ensure the service is running (`brew services list`) and port `5432` is free (`lsof -i :5432`).

### Step 3: Install IntelliJ IDEA Community Edition

1. **Download IntelliJ IDEA**:
   - Visit: [IntelliJ IDEA Downloads](https://www.jetbrains.com/idea/download/)
   - Download the **Community Edition** for macOS (e.g., `ideaIC-<version>.dmg`).
2. **Install IntelliJ IDEA**:
   - Open the `.dmg` file and drag **IntelliJ IDEA CE** to the **Applications** folder.
   - Launch IntelliJ IDEA from Applications or Spotlight.
   - Grant permissions if prompted (e.g., for file access).
3. **Verify Installation**:
   - Open IntelliJ IDEA and check the welcome screen.
   - Create a new project to confirm it runs.
   - **If it fails**, ensure JDK 21 is installed (`java -version`) and Rosetta 2 is installed on Apple Silicon (`softwareupdate --install-rosetta`).

### Troubleshooting (macOS)

- **MySQL Installation Fails**: Run `brew update` or reinstall (`brew reinstall mysql`).
- **PostgreSQL Connection Issues**: Check port `5432` (`lsof -i :5432`) and ensure the service is running (`brew services restart postgresql@16`).
- **IntelliJ Fails to Start**: Verify JDK in IntelliJ (`File > Project Structure > SDKs`) and ensure sufficient RAM.
- **Database Access Denied**: Reset passwords (`mysqladmin -u root password 'newpassword'`) or check `pg_hba.conf` for PostgreSQL.

---

## Spring Boot Project Setup with MySQL and PostgreSQL

### Step 1: Create a Spring Boot Project

1. **Generate Project**:
   - Visit: [Spring Initializr](https://start.spring.io/)
   - Configure:
     - Build Tool: Maven
     - Language: Java
     - Java Version: 21
     - Dependencies: Spring Web, Spring Data JPA, MySQL Driver, PostgreSQL Driver
   - Download and extract to:
     - Windows: `C:\Projects\myapp`
     - macOS: `~/Projects/myapp`
2. **Open in IntelliJ IDEA**:
   - Launch IntelliJ IDEA Community Edition.
   - Select **Open** and navigate to the project folder.
   - IntelliJ will detect `pom.xml` and resolve dependencies.

### Step 2: Configure Databases in Spring Boot

1. **Update `pom.xml`**:
   - Ensure dependencies for MySQL and PostgreSQL:
     ```xml
     <dependencies>
       <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-data-jpa</artifactId>
       </dependency>
       <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
         <groupId>com.mysql</groupId>
         <artifactId>mysql-connector-j</artifactId>
         <scope>runtime</scope>
       </dependency>
       <dependency>
         <groupId>org.postgresql</groupId>
         <artifactId>postgresql</artifactId>
         <scope>runtime</scope>
       </dependency>
     </dependencies>
     ```
   - Add Spring Boot Maven Plugin:
     ```xml
     <plugin>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-maven-plugin</artifactId>
       <version>3.2.5</version>
       <executions>
         <execution>
           <goals>
             <goal>build-image</goal>
           </goals>
         </execution>
       </executions>
     </plugin>
     ```

2. **Configure `application.properties`**:
   - In `src/main/resources/application.properties`, configure both databases (use one at a time or profiles):
     ```properties
     # MySQL Configuration
     spring.datasource.url=jdbc:mysql://localhost:3306/myappdb?createDatabaseIfNotExist=true
     spring.datasource.username=root
     spring.datasource.password=password
     spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
     spring.jpa.hibernate.ddl-auto=update
     server.port=8080

     # PostgreSQL Configuration (comment out MySQL config to use)
     # spring.datasource.url=jdbc:postgresql://localhost:5432/myappdb
     # spring.datasource.username=postgres
     # spring.datasource.password=postgres
     # spring.datasource.driver-class-name=org.postgresql.Driver
     # spring.jpa.hibernate.ddl-auto=update
     # server.port=8080
     ```

3. **Create Database**:
   - **MySQL**:
     ```bash
     mysql -u root -p
     CREATE DATABASE myappdb;
     ```
   - **PostgreSQL**:
     ```bash
     psql -U postgres
     CREATE DATABASE myappdb;
     ```

### Step 3: Database Connectivity in IntelliJ IDEA Community Edition

**Note**: The Database Tools and SQL plugin is not available in IntelliJ IDEA Community Edition, so database management relies on external tools (e.g., MySQL Workbench, pgAdmin) or command-line clients. Alternatively, install the **Database Navigator** plugin for basic database support.[](https://www.jetbrains.com/help/idea/postgresql.html)[](https://www.jetbrains.com/help/idea/mysql.html)

1. **Install Database Navigator Plugin**:
   - In IntelliJ IDEA:
     - Go to `File > Settings > Plugins > Marketplace`.
     - Search for **Database Navigator** and install.
     - Restart IntelliJ IDEA.
2. **Configure MySQL Connection**:
   - Open **DB Browser** (View > Tool Windows > DB Browser).
   - Click `+` > **Data Source** > **MySQL**.
   - Fill in:
     - **Name**: `MySQL Local`
     - **Host**: `localhost`
     - **Port**: `3306`
     - **Database**: `myappdb`
     - **User**: `root`
     - **Password**: `password`
   - Click **Test Connection** (download driver if prompted).
   - Click **OK** to save.
3. **Configure PostgreSQL Connection**:
   - Click `+` > **Data Source** > **PostgreSQL**.
   - Fill in:
     - **Name**: `PostgreSQL Local`
     - **Host**: `localhost`
     - **Port**: `5432`
     - **Database**: `myappdb`
     - **User**: `postgres`
     - **Password**: `postgres`
   - Click **Test Connection** (download driver if prompted).
   - Click **OK** to save.
4. **Run SQL Queries**:
   - In DB Browser, right-click the data source and select **Open Console** (or press `F4`).
   - Example query (for MySQL or PostgreSQL):
     ```sql
     CREATE TABLE users (
       id BIGINT AUTO_INCREMENT PRIMARY KEY,
       name VARCHAR(50) NOT NULL
     );
     INSERT INTO users (name) VALUES ('Test User');
     SELECT * FROM users;
     ```

### Step 4: Build and Run Spring Boot Application

1. **Run in IntelliJ**:
   - Open the main class (e.g., `MyappApplication.java`).
   - Right-click and select **Run 'MyappApplication'**.
   - Alternatively, use Maven:
     ```bash
     mvn spring-boot:run
     ```
2. **Test the Application**:
   - Access `http://localhost:8080` (or your API endpoints) in a browser or Postman.
   - Verify database connectivity by performing CRUD operations (requires a REST controller).

### Step 5: Build Docker Image (Optional)

1. **Navigate to Project**:
   - Windows:
     ```powershell
     cd C:\Projects\myapp
     ```
   - macOS:
     ```bash
     cd ~/Projects/myapp
     ```
2. **Build Image**:
   ```bash
   mvn spring-boot:build-image -Dspring-boot.build-image.imageName=yourusername/myapp
   ```
3. **Run Container**:
   ```bash
   docker run -p 8080:8080 --link mysql-container:mysql --link postgres-container:postgres yourusername/myapp
   ```
   - Ensure MySQL/PostgreSQL containers are running:
     ```bash
     docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=password -p 3306:3306 mysql:latest
     docker run -d --name postgres-container -e POSTGRES_PASSWORD=postgres -p 5432:5432 postgres:latest
     ```

### Step 6: Push to Docker Hub

```bash
docker login
docker push yourusername/myapp
```

---

## Summary of Commands

| Task                          | Windows (PowerShell)                                      | macOS (Terminal)                                      |
|-------------------------------|----------------------------------------------------------|------------------------------------------------------|
| Install MySQL                 | Download `mysql-installer-community-<version>.msi`       | `brew install mysql`                                 |
| Install PostgreSQL            | Download `postgresql-16.1-1-windows-x64.exe`            | `brew install postgresql@16`                         |
| Install IntelliJ IDEA          | Download `ideaIC-<version>.exe`                         | Download `ideaIC-<version>.dmg`                     |
| Verify MySQL                  | `mysql --version`                                       | `mysql --version`                                    |
| Verify PostgreSQL             | `psql --version`                                        | `psql --version`                                     |
| Run Spring Boot               | `mvn spring-boot:run`                                   | `mvn spring-boot:run`                                |
| Build Docker Image            | `mvn spring-boot:build-image ...`                       | Same as Windows                                      |
| Run Docker Container          | `docker run -p 8080:8080 ...`                           | Same as Windows                                      |

---
