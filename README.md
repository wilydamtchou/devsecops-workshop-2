
# DigiBank – Modular Monolith Banking Application
### Cloud Computing Professional License – University of the Mountains
### Workshop 2 – Static Security Analysis (SAST)
### By Engineer Willy Damtchou – July 2026


## 🏦 1. Introduction

DigiBank is a modular monolith banking application built with Spring Boot and Maven.  
It serves as the practical foundation for the DevSecOps learning path of the UCC152‑2 course.

Workshop 2 focuses on **static security analysis (SAST)**, applying the Shift Left principle to detect vulnerabilities early in the development lifecycle.

> “Workshop 2 is the first step explicitly dedicated to the security analysis of the DigiBank application.”

---

# 🧱 2. Architecture Overview

## 2.1 Modular Monolith Structure

The project is organized into five Maven modules:


digibank-parent/
├── common-module
├── customer-module
├── account-module
├── transfer-module
└── digibank-web

### common-module
Cross-cutting concerns: exceptions, API responses, shared utilities.

### customer-module
Customer creation, validation, retrieval.

### account-module
Account creation, balance management, debit/credit operations.

### transfer-module
Financial transfers between accounts.

### digibank-web
Spring Boot entry point, REST controllers, configuration, Swagger/OpenAPI.

---

# ⚙️ 3. Technical Stack

| Layer | Technology |
|------|------------|
| Language | Java 17 |
| Framework | Spring Boot 3.x |
| Build | Maven 3.9.x |
| Database | PostgreSQL |
| Testing | JUnit 5, Mockito |
| Mutation Testing | PITest |
| Static Analysis | SonarQube |
| Dependency Security | OWASP Dependency-Check |
| CI/CD | GitHub Actions |
| Documentation | Springdoc OpenAPI |

> “The tools selected for the workshop cover development, static analysis, dependency checking, test quality, and pipeline automation.”

---

# 🔐 4. DevSecOps & SAST Workflow

Workshop 2 applies the **Shift Left** principle: detect vulnerabilities as early as possible.

## Tools used

### ✔ SonarQube
Detects vulnerabilities, code smells, hotspots, coverage.

### ✔ OWASP Dependency-Check
Detects CVEs in dependencies.

### ✔ PITest
Evaluates robustness of tests via mutation testing.

---

# 🛠️ 5. Workshop 2 – Step-by-Step Workflow

## Step 1 — Prepare the Environment

Install:

- Java 17
- Maven 3.9
- IntelliJ IDEA
- Docker Desktop
- PostgreSQL
- Git & GitHub

Verify:


java -version
mvn -version
git –version
docker –version

---

## Step 2 — Open & Build DigiBank


mvn clean install

Reload Maven modules in IntelliJ.

---

## Step 3 — Configure SAST Tools in the Parent POM

Add:

- SonarQube plugin
- OWASP Dependency-Check plugin
- PITest plugin
- Surefire plugin
- Centralized version management

> “It makes sense to add the versions of the control tools from Workshop 2 to the parent POM.”

---

## Step 4 — Run Static Analysis

### SonarQube

Start SonarQube:


docker run -d –name sonarqube -p 9000:9000 sonarqube:lts-community

Run scan:


mvn clean verify sonar:sonar -Dsonar.login=YOUR_TOKEN

---

### OWASP Dependency-Check


mvn org.owasp:dependency-check-maven:check -DnvdApiKey=YOUR_KEY

Report:


target/dependency-check-report.html

---

### PITest Mutation Testing


mvn org.pitest:pitest-maven:mutationCoverage

Report:


target/pit-reports/

---

## Step 5 — Analyze Vulnerabilities

Categories:

- Insufficient input validation
- Overly revealing error messages
- Sensitive data exposure
- Weak business rules
- Secrets in configuration
- Outdated dependencies
- Fragile exception handling

> “The goal is not to passively launch a scan… but to understand the meaning of alerts.”

---

## Step 6 — Remediate Vulnerabilities

Examples:

### ✔ Remove secrets from configuration
Database passwords must not appear in `application-dev.yml`.

### ✔ Strengthen DTO validation
Add `@Pattern`, `@NotBlank`, `@Email`, etc.

### ✔ Improve error handling
Use standardized API error envelopes.

### ✔ Harden business logic
Validate transfer rules inside services.

### ✔ Update vulnerable dependencies
Based on Dependency-Check report.

### ✔ Improve mutation coverage
Add tests for edge cases.

---

## Step 7 — Final Validation

Run full pipeline:


mvn clean
mvn test
mvn clean verify
mvn dependency:tree
mvn org.owasp:dependency-check-maven:check
mvn org.pitest:pitest-maven:mutationCoverage
mvn clean verify sonar:sonar -Dsonar.login=YOUR_TOKEN

---

# 🚀 6. GitHub Actions CI/CD

Pipeline includes:

- Build
- Tests
- Dependency-Check
- PITest
- Artifact upload

> “This pipeline checks out the code, installs Java 17, rebuilds DigiBank, runs tests, scans dependencies, and produces PITest reports.”

---

# 📦 7. Running DigiBank Locally

### Start PostgreSQL
Create DB `digibankdb`.

### Set environment variables


SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/digibankdb
SPRING_DATASOURCE_USERNAME=digibank
SPRING_DATASOURCE_PASSWORD=digibank123

### Run application


mvn spring-boot:run -pl digibank-web

---

# 🧪 8. API Documentation

Swagger/OpenAPI:


/swagger-ui.html
/api-docs

---

# 🎯 9. Learning Objectives Achieved

Students can:

- Explain SAST & Shift Left
- Identify static vulnerabilities
- Use SonarQube, Dependency-Check, PITest
- Remediate code & configuration weaknesses
- Deliver a hardened version of DigiBank
- Prepare for DAST in Workshop 3

---

# 🏁 10. Conclusion

DigiBank is now:

- Functionally stable
- Architecturally clean
- Instrumented for SAST
- Hardened against common vulnerabilities
- Ready for DAST
- Prepared for containerization & delivery pipelines