
# DevOps Midterm Starter (Spring Boot + Jenkins + Nexus)

This repository contains a minimal Spring Boot app plus the CI/CD scaffolding you need to satisfy the midterm tasks:

1. Commit a Spring Boot project in Java into GitHub.
2. Use **dockerized Jenkins** to integrate with GitHub.
3. Write a **Jenkinsfile** with basic stages; build a **fat jar** during the build.
4. Run **Nexus** in Docker and **publish** the artifact to it.

## Quick Start

```bash
# 1) Start Jenkins and Nexus (first time may take a few minutes)
docker compose up -d

# 2) Open Jenkins: http://localhost:8080  (admin user will be created interactively)
#    Open Nexus:   http://localhost:8081  (complete the first-run wizard)
```

### Nexus Setup (once)
1. Sign in to Nexus (admin) and create two hosted repositories:
   - `maven-releases` (hosted)
   - `maven-snapshots` (hosted)
2. Make note of the admin username/password (or create a dedicated `jenkins` user).
3. In Jenkins, create **Credentials → Username & Password** with ID `nexus-creds` to match the Nexus user.

### Jenkins Setup (once)
- Jenkins runs from the custom Docker image in `jenkins/Dockerfile` with **JDK 17** and **Maven** preinstalled.
- Create a **Pipeline** job pointing to your GitHub repo URL (use a webhook or Poll SCM).
- Ensure the job uses this repository's `Jenkinsfile`.
- First run will build and attempt to deploy to Nexus.

## Local build (optional)
```bash
# Run tests & package a fat jar locally
docker run --rm -v "$PWD":/ws -w /ws maven:3.9-eclipse-temurin-17 mvn -B clean package
```

## Endpoints
- `GET /api/hello` → `{"message":"Hello, DevOps!"}`
