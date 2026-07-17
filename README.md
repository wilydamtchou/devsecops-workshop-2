# DigiBank - Digital Core Banking System

## 📋 Project Overview

**DigiBank** is a digital banking application developed as part of the **UCC152-2 - Introduction to Security in DevOps** course for the Cloud Computing Professional License at Université des Montagnes.

This project serves as **Workshop 1 (TP1)** and provides a realistic foundation for learning DevSecOps principles. It is a modular monolithic application implemented with Spring Boot, designed to demonstrate secure development practices, automated testing, and continuous integration.

### Academic Context

- **Course**: UCC152-2 - Introduction to Security in DevOps
- **Program**: Professional License in Cloud Computing
- **Institution**: Université des Montagnes
- **Instructor**: Ing. Willy Damtchou
- **Date**: July 2026

---

## 🎯 Workshop Objectives

This practical work aims to achieve the following pedagogical objectives:

### General Objective

Design, implement, test, and prepare for deployment a DigiBank application in a Spring Boot modular monolithic architecture, integrating from the first version the foundations necessary for a coherent DevSecOps approach.

### Specific Objectives

By the end of this workshop, students will be able to:

- **A1.1**: Explain the role of the DigiBank project in the course progression and justify the choice of a modular monolithic architecture for a first pedagogical implementation
- **A1.2**: Create a Maven Spring Boot parent project organized into several business modules with clear separation of responsibilities
- **A1.3**: Implement the fundamental layers of each module (entities, repositories, services, controllers, DTOs, validations, exceptions)
- **A1.4**: Expose consistent REST APIs for DigiBank's main operations
- **A1.5**: Produce at least one index view presenting the application and providing access to Swagger/OpenAPI documentation
- **A1.6**: Write unit tests with JUnit and integration scenarios with Cucumber
- **A1.7**: Prepare the application for Docker container execution and automation in a GitHub Actions pipeline
- **A1.8**: Deliver a healthy, stable, and exploitable base for future SAST, DAST, and container/dependency security workshops

---

## 🏗️ Architecture

### Why a Modular Monolith?

DigiBank follows a **modular monolithic architecture** rather than microservices for several pedagogical reasons:

1. **Simplicity**: Reduces operational complexity while maintaining clear boundaries
2. **Learning Focus**: Allows students to focus on secure development without distributed system complexity
3. **Testability**: Easier to test as a single deployable unit
4. **Shift Left Security**: Proper structure is a prerequisite for effective DevSecOps
5. **Evolution Path**: Can later evolve to microservices if needed

### Project Structure

```
digibank-parent/
├── common-module           # Shared classes, exceptions, API responses
├── customer-module         # Customer management domain
├── account-module          # Bank account management domain
├── transfer-module         # Transfer and transaction management domain
└── digibank-web           # Main entry point, configuration, REST APIs
```

### Module Responsibilities

| Module | Responsibility | Key Features |
|--------|---------------|--------------|
| **common-module** | Shared elements | Business exceptions, generic API responses, utilities |
| **customer-module** | Customer management | Customer creation, retrieval, validation |
| **account-module** | Account management | Account creation, balance management, account types |
| **transfer-module** | Transfer management | Transfer execution, transaction history |
| **digibank-web** | Web application | Spring Boot entry point, REST controllers, OpenAPI documentation, home page |

### Layered Architecture

Each business module follows a clean layered architecture:

- **Entity**: JPA data model
- **Repository**: Data access layer (Spring Data JPA)
- **Service**: Business logic layer
- **Controller**: REST API exposure
- **DTO**: Data Transfer Objects (Request/Response)
- **Validation**: Declarative validations (Bean Validation)

---

## 🛠️ Technology Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| **Java** | 17 | Development language |
| **Spring Boot** | 3.3.2 | Application framework |
| **Maven** | 3.9.x | Build and dependency management |
| **PostgreSQL** | 16 | Relational database |
| **Spring Data JPA** | - | Data persistence |
| **Spring Validation** | - | Input validation |
| **Springdoc OpenAPI** | 2.6.0 | API documentation (Swagger) |
| **JUnit** | 5.10.2 | Unit testing |
| **Mockito** | - | Mocking for tests |
| **Cucumber** | 7.18.0 | BDD (Behavior-Driven Development) testing |
| **H2 Database** | - | In-memory database for tests |
| **Docker** | Latest | Containerization |
| **GitHub Actions** | - | CI/CD pipeline |

---

## 📦 Prerequisites

### Required Software

Before starting, ensure you have the following installed:

- **Java JDK 17**
- **Apache Maven 3.9+**
- **PostgreSQL 15+** (or Docker for containerized execution)
- **Docker Desktop** (optional but recommended)
- **Git**
- **IntelliJ IDEA** (Community or Ultimate edition recommended)
- **Web Browser** (modern)
- **Postman** (optional, for manual API testing)

### Environment Verification

Run these commands to verify your environment:

```bash
java -version          # Should show Java 17
mvn -version           # Should show Maven 3.9+
git --version          # Should show Git version
docker --version       # Should show Docker version
docker compose version # Should show Docker Compose version
psql --version         # Should show PostgreSQL version
```

### Important Environment Setup Notes

- **JAVA_HOME**: Ensure this environment variable points to JDK 17
- **PATH**: Maven, Java, Git, and Docker executables must be in your PATH
- **IDE Configuration**: Enable Maven auto-import in IntelliJ IDEA
- **Encoding**: Set UTF-8 as default encoding

---

## 🚀 Part 1: Understanding Context & Architecture

### 1.1 Why DigiBank for DevSecOps Learning?

DigiBank simulates a digital banking system, which concentrates several characteristics of modern systems:

- Identity management
- Protection of personal and financial data
- API exposure
- Transactional integrity
- Operation traceability
- Access control
- Availability requirements
- Industrial deployment artifacts

This realistic context allows students to work on a sufficiently rich perimeter to illustrate DevSecOps logic without falling into an artificially simplified case.

### 1.2 Functional Scope

The first version of DigiBank covers:

- **Customer Management**: Create and retrieve customers
- **Account Management**: Create and retrieve bank accounts
- **Transfers**: Execute simple transfers between accounts
- **Transaction History**: View transaction history
- **Home Page**: Simple welcome view presenting DigiBank
- **API Documentation**: Automatically generated Swagger/OpenAPI documentation

### 1.3 Why Not Start with Microservices?

Starting with microservices would introduce premature complexity:

- Inter-service communication
- Service discovery and routing
- Distributed observability
- Multiple deployment orchestration
- Failure tolerance
- Consistency between services

The modular monolith serves as an ideal pedagogical compromise: it allows establishing business boundaries, working on application layers, documenting APIs, writing tests, and preparing security—without dispersing students into distributed infrastructure problems that are still premature.

---

## 🔧 Part 2: Environment Setup & Project Creation

### 2.1 Setting Up PostgreSQL

#### Option 1: Local PostgreSQL Installation

Create the DigiBank database:

```sql
CREATE DATABASE digibankdb;
CREATE USER digibank WITH ENCRYPTED PASSWORD 'digibank123';
GRANT ALL PRIVILEGES ON DATABASE digibankdb TO digibank;
```

#### Option 2: PostgreSQL with Docker (Recommended)

Launch PostgreSQL in a container:

```bash
docker run -d \
  --name digibank-postgres \
  --restart unless-stopped \
  -e POSTGRES_DB=digibankdb \
  -e POSTGRES_USER=digibank \
  -e POSTGRES_PASSWORD=digibank123 \
  -p 5432:5432 \
  -v digibank-postgres-data:/var/lib/postgresql/data \
  postgres:16
```

Verify the container is running:

```bash
docker ps
docker logs digibank-postgres
```

### 2.2 Creating the Project in IntelliJ IDEA

Follow these steps to create the DigiBank project:

1. **Open IntelliJ IDEA**
2. **Select**: New Project
3. **Choose**: Maven as project type
4. **Select**: JDK 17
5. **Configure Parent Project**:
   - **GroupId**: `com.m2ibank`
   - **ArtifactId**: `digibank-parent`
   - **Version**: `1.0.0-SNAPSHOT`
6. **Location**: `~/projects/ucc152/digibank-parent`
7. **Finish** project creation

### 2.3 Creating the Parent POM

Create the root `pom.xml` with the following content:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>

  <groupId>com.m2ibank</groupId>
  <artifactId>digibank-parent</artifactId>
  <version>1.0.0-SNAPSHOT</version>
  <packaging>pom</packaging>
  <name>digibank-parent</name>
  <description>DigiBank modular monolith parent project</description>

  <modules>
    <module>common-module</module>
    <module>customer-module</module>
    <module>account-module</module>
    <module>transfer-module</module>
    <module>digibank-web</module>
  </modules>

  <properties>
    <java.version>17</java.version>
    <spring.boot.version>3.3.2</spring.boot.version>
    <springdoc.version>2.6.0</springdoc.version>
    <cucumber.version>7.18.0</cucumber.version>
    <junit.version>5.10.2</junit.version>
    <maven.compiler.plugin.version>3.13.0</maven.compiler.plugin.version>
  </properties>

  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>${spring.boot.version}</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>

  <build>
    <pluginManagement>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>${maven.compiler.plugin.version}</version>
          <configuration>
            <source>${java.version}</source>
            <target>${java.version}</target>
          </configuration>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>
```

### 2.4 Creating Sub-Modules

In IntelliJ IDEA, create each sub-module:

1. **Right-click** on parent project
2. **Select**: New → Module
3. **Type**: Maven
4. **Parent**: Inherit from digibank-parent
5. **Module Name**: Choose from the list below

Create these modules in order:

- `common-module`
- `customer-module`
- `account-module`
- `transfer-module`
- `digibank-web`

### 2.5 Creating Module POMs

#### common-module/pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>

  <parent>
    <groupId>com.m2ibank</groupId>
    <artifactId>digibank-parent</artifactId>
    <version>1.0.0-SNAPSHOT</version>
  </parent>

  <artifactId>common-module</artifactId>
  <name>common-module</name>
  <packaging>jar</packaging>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
  </dependencies>
</project>
```

#### customer-module/pom.xml (and similar for account-module, transfer-module)

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>

  <parent>
    <groupId>com.m2ibank</groupId>
    <artifactId>digibank-parent</artifactId>
    <version>1.0.0-SNAPSHOT</version>
  </parent>

  <artifactId>customer-module</artifactId>
  <name>customer-module</name>
  <packaging>jar</packaging>

  <dependencies>
    <dependency>
      <groupId>com.m2ibank</groupId>
      <artifactId>common-module</artifactId>
      <version>${project.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
      <groupId>org.postgresql</groupId>
      <artifactId>postgresql</artifactId>
      <scope>runtime</scope>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>org.mockito</groupId>
      <artifactId>mockito-core</artifactId>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

#### digibank-web/pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>

  <parent>
    <groupId>com.m2ibank</groupId>
    <artifactId>digibank-parent</artifactId>
    <version>1.0.0-SNAPSHOT</version>
  </parent>

  <artifactId>digibank-web</artifactId>
  <name>digibank-web</name>
  <packaging>jar</packaging>

  <dependencies>
    <!-- Internal modules -->
    <dependency>
      <groupId>com.m2ibank</groupId>
      <artifactId>common-module</artifactId>
      <version>${project.version}</version>
    </dependency>
    <dependency>
      <groupId>com.m2ibank</groupId>
      <artifactId>customer-module</artifactId>
      <version>${project.version}</version>
    </dependency>
    <dependency>
      <groupId>com.m2ibank</groupId>
      <artifactId>account-module</artifactId>
      <version>${project.version}</version>
    </dependency>
    <dependency>
      <groupId>com.m2ibank</groupId>
      <artifactId>transfer-module</artifactId>
      <version>${project.version}</version>
    </dependency>

    <!-- Spring Boot starters -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>

    <!-- OpenAPI/Swagger -->
    <dependency>
      <groupId>org.springdoc</groupId>
      <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
      <version>${springdoc.version}</version>
    </dependency>

    <!-- Database -->
    <dependency>
      <groupId>org.postgresql</groupId>
      <artifactId>postgresql</artifactId>
      <scope>runtime</scope>
    </dependency>

    <!-- Testing -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>com.h2database</groupId>
      <artifactId>h2</artifactId>
      <scope>test</scope>
    </dependency>

    <!-- Cucumber -->
    <dependency>
      <groupId>io.cucumber</groupId>
      <artifactId>cucumber-java</artifactId>
      <version>${cucumber.version}</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>io.cucumber</groupId>
      <artifactId>cucumber-spring</artifactId>
      <version>${cucumber.version}</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>io.cucumber</groupId>
      <artifactId>cucumber-junit-platform-engine</artifactId>
      <version>${cucumber.version}</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.junit.platform</groupId>
      <artifactId>junit-platform-suite</artifactId>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

### 2.6 Creating Standard Maven Directory Structure

For each module, create the standard Maven directory structure:

```
module-name/
├── src/
│   ├── main/
│   │   ├── java/
│   │   └── resources/
│   └── test/
│       ├── java/
│       └── resources/
└── pom.xml
```

### 2.7 Creating .gitignore

Create a `.gitignore` file at the project root:

```gitignore
target/
!.mvn/wrapper/maven-wrapper.jar
!**/src/main/**/target/
!**/src/test/**/target/
.kotlin

### IntelliJ IDEA ###
.idea/modules.xml
.idea/jarRepositories.xml
.idea/compiler.xml
.idea/libraries/
*.iws
*.iml
*.ipr

### Eclipse ###
.apt_generated
.classpath
.factorypath
.project
.settings
.springBeans
.sts4-cache

### NetBeans ###
/nbproject/private/
/nbbuild/
/dist/
/nbdist/
/.nb-gradle/
build/
!**/src/main/**/build/
!**/src/test/**/build/

### VS Code ###
.vscode/

### Mac OS ###
.DS_Store

### Logs ###
logs/
*.log

### Environment files ###
.env
```

### 2.8 Initial Configuration Files

Create `digibank-web/src/main/resources/application.yml`:

```yaml
spring:
  application:
    name: digibank
  profiles:
    active: dev
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    open-in-view: false
    properties:
      hibernate:
        format_sql: true
        jdbc:
          time_zone: UTC
  thymeleaf:
    cache: false
  sql:
    init:
      mode: never

server:
  port: 8080
  error:
    include-message: never
    include-binding-errors: never
    include-stacktrace: never

springdoc:
  api-docs:
    path: /api-docs
  swagger-ui:
    path: /swagger-ui.html
    operationsSorter: method
    tagsSorter: alpha

logging:
  level:
    root: INFO
    org.springframework.web: INFO
    org.hibernate.SQL: DEBUG
    org.hibernate.type.descriptor.sql.BasicBinder: TRACE
```

Create `digibank-web/src/main/resources/application-dev.yml`:

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/digibankdb
    username: digibank
    password: digibank123
    driver-class-name: org.postgresql.Driver
  jpa:
    hibernate:
      ddl-auto: update
```

Create `digibank-web/src/main/resources/application-test.yml`:

```yaml
spring:
  datasource:
    url: jdbc:h2:mem:digibanktestdb
    driver-class-name: org.h2.Driver
    username: sa
    password:
  jpa:
    hibernate:
      ddl-auto: create-drop
    show-sql: false
```

### 2.9 Initialize Git Repository

```bash
cd digibank-parent
git init
git add .
git commit -m "Initialize DigiBank multi-module project structure"
```

### 2.10 First Maven Build

Test the project structure:

```bash
mvn clean install
```

This command should resolve all dependencies and compile the project successfully.

### 2.11 Reload Maven Project in IntelliJ IDEA

After all POM modifications:

1. Click on **Reload All Maven Projects** in Maven view
2. Verify all modules appear correctly
3. Verify Spring Boot dependencies are resolved
4. Verify no POM is marked as invalid

---

## 💻 Part 3: Source Code Implementation

### 3.1 Implementation Strategy

We will implement the modules in this order:

1. **common-module**: Shared exceptions and API responses
2. **customer-module**: Customer management
3. **account-module**: Account management
4. **transfer-module**: Transfer management
5. **digibank-web**: Main application and configuration

### 3.2 common-module Implementation

#### Step 1: Create ApiResponse Class

File: `common-module/src/main/java/com/m2ibank/common/api/ApiResponse.java`

```java
package com.m2ibank.common.api;

import java.time.LocalDateTime;

public class ApiResponse<T> {
    private boolean success;
    private String message;
    private T data;
    private LocalDateTime timestamp;

    public ApiResponse() {
        this.timestamp = LocalDateTime.now();
    }

    public ApiResponse(boolean success, String message, T data) {
        this.success = success;
        this.message = message;
        this.data = data;
        this.timestamp = LocalDateTime.now();
    }

    public static <T> ApiResponse<T> success(String message, T data) {
        return new ApiResponse<>(true, message, data);
    }

    public static <T> ApiResponse<T> failure(String message, T data) {
        return new ApiResponse<>(false, message, data);
    }

    // Getters and setters
    public boolean isSuccess() { return success; }
    public void setSuccess(boolean success) { this.success = success; }
    public String getMessage() { return message; }
    public void setMessage(String message) { this.message = message; }
    public T getData() { return data; }
    public void setData(T data) { this.data = data; }
    public LocalDateTime getTimestamp() { return timestamp; }
    public void setTimestamp(LocalDateTime timestamp) { this.timestamp = timestamp; }
}
```

#### Step 2: Create Exception Classes

File: `common-module/src/main/java/com/m2ibank/common/exception/ResourceNotFoundException.java`

```java
package com.m2ibank.common.exception;

public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

File: `common-module/src/main/java/com/m2ibank/common/exception/BusinessException.java`

```java
package com.m2ibank.common.exception;

public class BusinessException extends RuntimeException {
    public BusinessException(String message) {
        super(message);
    }
}
```

### 3.3 customer-module Implementation

Refer to the existing implementation in your project. The key components are:

- **Entity**: `Customer.java` with JPA annotations
- **DTOs**: `CustomerRequest.java` with validation, `CustomerResponse.java`
- **Repository**: `CustomerRepository.java` extending JpaRepository
- **Service**: `CustomerService.java` with business logic
- **Controller**: `CustomerController.java` with REST endpoints

### 3.4 account-module and transfer-module Implementation

Similar pattern to customer-module. Check your existing code for complete implementations.

### 3.5 digibank-web Implementation

#### Main Application Class

File: `digibank-web/src/main/java/com/m2ibank/web/DigiBankApplication.java`

```java
package com.m2ibank.web;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.domain.EntityScan;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;

@SpringBootApplication
@ComponentScan(basePackages = "com.m2ibank")
@EntityScan(basePackages = "com.m2ibank")
@EnableJpaRepositories(basePackages = "com.m2ibank")
public class DigiBankApplication {
    public static void main(String[] args) {
        SpringApplication.run(DigiBankApplication.class, args);
    }
}
```

**Critical**: The `@ComponentScan`, `@EntityScan`, and `@EnableJpaRepositories` annotations with `basePackages = "com.m2ibank"` are essential for Spring to detect all components across modules.

#### Global Exception Handler

File: `digibank-web/src/main/java/com/m2ibank/web/exception/GlobalExceptionHandler.java`

Check your existing implementation for the complete exception handler.

### 3.6 Compile and Run

```bash
# Compile entire project
mvn clean install

# Run the application
mvn spring-boot:run -pl digibank-web
```

---

## 🎨 Part 4: Configuration, Home Page, Swagger & Data Initialization

### 4.1 Home Page Controller

File: `digibank-web/src/main/java/com/m2ibank/web/controller/HomeController.java`

Check your existing implementation.

### 4.2 Home Page Template

File: `digibank-web/src/main/resources/templates/index.html`

Your project already has a complete home page template.

### 4.3 OpenAPI Configuration

File: `digibank-web/src/main/java/com/m2ibank/web/config/OpenApiConfig.java`

Check your existing OpenAPI configuration.

### 4.4 Data Initialization

File: `digibank-web/src/main/java/com/m2ibank/web/bootstrap/DataInitializer.java`

Your project initializes demo customers and accounts automatically.

### 4.5 Verification Steps

1. **Start the application**:
   ```bash
   mvn spring-boot:run -pl digibank-web
   ```

2. **Access the home page**: http://localhost:8080/

3. **Access Swagger UI**: http://localhost:8080/swagger-ui.html

4. **Test API Endpoints** through Swagger

---

## 🧪 Part 5: Unit Tests with JUnit

### 5.1 Test Strategy

Three levels of testing:

1. **Unit Tests**: Business logic in isolation (Mockito)
2. **Integration Tests**: Component cooperation (Spring Boot Test)
3. **Cucumber BDD**: Functional scenarios

### 5.2 Running Unit Tests

```bash
# Run all unit tests
mvn test

# Run tests for specific module
mvn test -pl customer-module

# Run tests with detailed output
mvn test -X
```

### 5.3 Test Coverage

Your project includes unit tests for:
- `CustomerServiceTest`
- `AccountServiceTest`
- `TransferServiceTest`

---

## 🥒 Part 6: Cucumber BDD Tests

### 6.1 Cucumber Test Structure

Your project has:
- **Configuration**: `CucumberSpringConfiguration.java`
- **Runner**: `RunCucumberTest.java`
- **Features**: `customer_management.feature`, `transfer_management.feature`
- **Step Definitions**: `CustomerStepDefinitions.java`, `TransferStepDefinitions.java`

### 6.2 Running Cucumber Tests

```bash
# Run integration tests with Cucumber
mvn verify

# View Cucumber reports
open digibank-web/target/cucumber-reports/cucumber.html
```

---

## 🐳 Part 7: Docker & Deployment

### 7.1 Running with Docker Compose

```bash
# Build JAR
mvn clean package -DskipTests

# Start all services
docker compose up --build

# View logs
docker compose logs -f

# Stop services
docker compose down
```

### 7.2 Access Points

- **Application**: http://localhost:8080/
- **Swagger UI**: http://localhost:8080/swagger-ui.html
- **PostgreSQL**: localhost:5432

---

## 📚 API Documentation

Once running, access:

- **Home Page**: http://localhost:8080/
- **Swagger UI**: http://localhost:8080/swagger-ui.html
- **OpenAPI JSON**: http://localhost:8080/api-docs

### Main Endpoints

#### Customer Management
- `POST /api/customers` - Create customer
- `GET /api/customers` - List all customers
- `GET /api/customers/{id}` - Get customer by ID

#### Account Management
- `POST /api/accounts` - Create account
- `GET /api/accounts` - List all accounts
- `GET /api/accounts/{id}` - Get account by ID
- `GET /api/accounts/customer/{customerId}` - Get accounts by customer

#### Transfer Management
- `POST /api/transfers` - Execute transfer
- `GET /api/transfers` - List all transfers
- `GET /api/transfers/account/{accountId}` - Get transfers for account

---

## 🔐 Security Considerations

### Current Implementation (TP1)

✅ **Implemented**:
- Input validation with Bean Validation
- Layered architecture separation
- Centralized exception handling
- Automated tests
- Externalized configuration
- No hardcoded secrets in code

⚠️ **Future Improvements (TP2, TP3, TP4)**:
- Static analysis (SAST)
- Dynamic testing (DAST)
- Container security
- Dependency scanning
- Authentication/Authorization
- Secret management
- Encryption

---

## 🛠️ Useful Commands

### Maven Commands

```bash
mvn clean install              # Build entire project
mvn test                       # Run unit tests
mvn verify                     # Run all tests
mvn spring-boot:run -pl digibank-web  # Run application
mvn dependency:tree            # Show dependencies
```

### Docker Commands

```bash
docker compose up --build      # Build and start
docker compose logs -f         # View logs
docker compose down            # Stop services
docker compose down -v         # Stop and remove volumes
```

### Git Commands

```bash
git status                     # Check status
git add .                      # Stage changes
git commit -m "message"        # Commit changes
git push                       # Push to remote
```

---

## 📝 Project Structure Summary

```
digibank-parent/
├── .github/workflows/
│   └── digibank-ci.yml
├── common-module/
├── customer-module/
├── account-module/
├── transfer-module/
├── digibank-web/
├── Dockerfile
├── docker-compose.yml
├── pom.xml
└── README.md
```

---

## 🎓 Learning Outcomes

Students learn:

### Technical Skills
- Maven multi-module projects
- Spring Boot development
- RESTful API design
- JPA/Hibernate
- Unit testing (JUnit/Mockito)
- BDD testing (Cucumber)
- API documentation (Swagger)
- Docker containerization
- CI/CD (GitHub Actions)

### DevSecOps Principles
- Shift Left Security
- Modular architecture
- Automated testing
- Infrastructure as Code
- Continuous Integration

---

## 🚀 Next Steps

### Workshop 2 (TP2): SAST
- SonarQube integration
- Static code analysis
- Security scanning

### Workshop 3 (TP3): DAST
- Penetration testing
- OWASP ZAP
- API security testing

### Workshop 4 (TP4): Container Security
- Docker hardening
- Dependency scanning
- Secret management

---

## 📞 Support & Resources

### Technical Documentation
- [Spring Boot Docs](https://docs.spring.io/spring-boot/)
- [Spring Data JPA](https://docs.spring.io/spring-data/jpa/)
- [Cucumber](https://cucumber.io/docs/)
- [Docker](https://docs.docker.com/)
- [PostgreSQL](https://www.postgresql.org/docs/)

---

## ✅ Workshop Completion Checklist

- [ ] Environment setup completed
- [ ] PostgreSQL database created
- [ ] Multi-module Maven project created
- [ ] All modules implemented
- [ ] Home page created
- [ ] Swagger configured
- [ ] Unit tests passing
- [ ] Cucumber tests passing
- [ ] Dockerfile created
- [ ] docker-compose.yml created
- [ ] GitHub Actions configured
- [ ] Application runs successfully
- [ ] Git repository initialized

---

**Version**: 1.0.0-SNAPSHOT
**Last Updated**: July 2026
**Status**: ✅ TP1 Completed - Ready for TP2 (SAST)

---

**🎯 End of DigiBank Workshop 1 Documentation**
