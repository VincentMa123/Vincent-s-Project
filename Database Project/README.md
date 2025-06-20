# SUSTC PubMed Service

## Overview  
This Spring Boot–based microservice provides user authentication (register, login, logout) with role‑based access control, backed by a PostgreSQL database. It’s organized into two Gradle/Kotlin modules:  
- **sustc-api**: Defines entities, repositories, services, controllers, and security configuration.  
- **sustc-runner**: Contains the main application entry point and pulls in `sustc-api` as a dependency.

## Key Components

### sustc-api  
- **Model**  
  - `io.pubmed.model.User` — JPA entity for storing username, password (BCrypt‑hashed), and role (e.g. ROLE_USER, ROLE_ADMIN).  
- **DTO**  
  - `io.pubmed.dto.UserDto` — carries registration payload (username, password, optional role).  
- **Repository**  
  - `io.pubmed.repository.UserRepository` — extends `JpaRepository<User, Long>`, with `findByUsername()`.  
- **Service**  
  - `io.pubmed.service.CustomUserDetailsService` — implements `UserDetailsService` for Spring Security, loading `User` → `UserDetails`.  
- **Config**  
  - `io.pubmed.config.SecurityConfig` — configures HTTP security (CSRF disabled, `/api/auth/**` open, all else authenticated), form‑login, logout, and BCrypt password encoder.  
- **Controller**  
  - `io.pubmed.controller.AuthController`  
    - `POST /api/auth/register` — create new user  
    - `POST /login` (Spring Security default) — authenticate  
    - `GET  /logout` — invalidate session

### sustc-runner  
- **Application Entry**  
  - `io.pubmed.Application` — Spring Boot `@SpringBootApplication` main class  
- **Dependency**  
  - Declares `implementation(project(":sustc-api"))` in `build.gradle.kts`

## Prerequisites  
- **JDK 17+** (or matching your Spring Boot version)  
- **Gradle Wrapper** (included)  
- **PostgreSQL** (v10+) running locally or remotely  

## Setup & Configuration

1. **Clone & Import**  
   ```bash
   git clone <repo-url>
   cd <repo-root>
