# Thonmaker – Azure DevSecOps CI/CD (Docker → ACR → App Service)
## SAST (Semgrep) + Container Scanning (Trivy) + DAST (OpenVAS)
## CMMC Level 2 / NIST SP 800-171 (110 Controls) Mapping

Thonmaker is a containerized web application deployed through an **end-to-end Azure DevSecOps pipeline**. 
This repository is built as a **portfolio-grade DevSecOps and security engineering project** demonstrating how **secure CI/CD**, **container security**, and **dynamic vulnerability scanning** are implemented in real-world enterprise environments.

The project integrates **SAST**, **container vulnerability scanning**, and **DAST** using **Semgrep**, **Trivy**, and **OpenVAS**, enforced through **Azure DevOps pipelines** and **self-hosted infrastructure**.

Successful deployment validation page: 
**“Thonmaker Docker CI/CD working!”**

---

## What This Project Demonstrates

- End-to-end CI/CD with **Azure DevOps Pipelines**
- **Self-hosted build agents** (Windows + Linux VM)
- Docker image build, tagging, and deployment
- **Static Application Security Testing (SAST)** with Semgrep
- **Intentional security gating** (pipeline fails on findings)
- **Container vulnerability scanning** with Trivy
- **Dynamic Application Security Testing (DAST)** with OpenVAS
- Private image storage using **Azure Container Registry (ACR)**
- Secure deployments to **Azure App Service (Containers)**
- **Managed Identity + RBAC** for passwordless ACR access
- Auditable logs and artifacts suitable for compliance evidence

---

## Architecture Overview

### High-Level Flow

1. Developer pushes code to `main`
2. Azure DevOps Pipeline triggers
3. **Semgrep SAST** scans source code 
   - Pipeline **fails intentionally** on findings (security gate)
4. Docker image is built and tagged with `$(Build.BuildId)`
5. **Trivy** scans the container image for HIGH / CRITICAL CVEs
6. Image is pushed to **Azure Container Registry (ACR)**
7. **Azure App Service** pulls image using **Managed Identity (AcrPull)**
8. Application is deployed and validated
9. **OpenVAS (DAST)** scans the running service from a dedicated VM

---

## Pipeline & Security Evidence (Screenshots)

### Azure DevSecOps Pipeline Overview
![Pipeline Overview](docs/screenshots/pipeline-overview.png)

---

### SAST – Semgrep (Fail by Design)
This screenshot shows the Semgrep SAST stage **failing intentionally** to enforce secure coding standards.

![Semgrep Fail](docs/screenshots/semgrep-fail.png)

---

### DAST – OpenVAS Dashboard
OpenVAS running on a dedicated Azure Linux VM performing dynamic vulnerability scans against the deployed application.

![OpenVAS Dashboard](docs/screenshots/OpenVas.png)

---

### OpenVAS Scan Results
Results from a completed OpenVAS scan (DAST), used as audit and compliance evidence.

![scan-results](docs/screenshots/scan-results.png)

---

## Technologies Used

- **Azure DevOps Pipelines** – CI/CD automation
- **Self-Hosted Windows Agent** – build & pipeline execution
- **Self-Hosted Linux VM** – security scanning & DAST
- **Docker** – container packaging
- **Semgrep** – SAST (static code analysis)
- **Trivy** – container vulnerability scanning (CVEs)
- **OpenVAS (Greenbone)** – DAST (dynamic vulnerability scanning)
- **Azure Container Registry (ACR)** – private image registry
- **Azure App Service (Linux Containers)** – application hosting
- **Managed Identity + RBAC** – secure image pulls
- **Git / GitHub** – version control

---

## Security Testing Strategy

### SAST – Semgrep (Intentional Failure)

Semgrep is configured as a **security gate** in the pipeline.

- Findings **cause the pipeline to fail**
- This behavior is **intentional**
- Demonstrates:
  - Shift-left security
  - Enforced secure coding standards
  - CI/CD security gating

> A failing Semgrep stage indicates successful enforcement, not a broken pipeline.

---

### Container Security – Trivy

- Trivy scans built Docker images
- Detects OS and dependency CVEs
- Focuses on **HIGH and CRITICAL** severity issues
- Results are logged and auditable

---

### DAST – OpenVAS (Greenbone)

Dynamic scanning is performed using **OpenVAS**, running on a **dedicated Azure Linux VM**.

**Why a VM?**
- Realistic enterprise architecture
- Isolated security tooling
- Ability to scan deployed services and infrastructure
- Supports internal and external targets

**Scan Types Used**
- **Fast Scan** – frequent validation
- **Full and Fast Scan** – deep, scheduled assessments

OpenVAS scans:
- Deployed application endpoints
- Network-exposed services
- Misconfigurations and known vulnerabilities

Reports can be exported (PDF / XML / CSV) and used as **audit evidence**.

---

## Self-Hosted Infrastructure

### Azure VM – Security & Agent Host

The Azure VM runs:
- Azure DevOps **self-hosted agent**
- **OpenVAS (Greenbone)** services

This enables:
- Custom tooling
- DAST scans
- Security workloads isolated from managed agents
- Greater control over compliance tooling

---

## How to Demo This Project

1. Push code to the `main` branch
2. Azure DevOps pipeline starts automatically
3. Semgrep runs and **fails intentionally** if findings exist
4. Docker image builds and is scanned with Trivy
5. Image is pushed to ACR
6. App Service deploys the container
7. Application loads successfully
8. OpenVAS runs DAST scans against the live service
9. Results are reviewed and exported as evidence

---

## CMMC Level 2 (NIST SP 800-171) – 110 Control Mapping

CMMC Level 2 aligns directly to **NIST SP 800-171**, consisting of **110 security requirements** across **14 control families**.

This repository provides **technical and engineering implementation evidence** supporting these requirements through CI/CD, container security, identity management, logging, and vulnerability scanning.

---

### 1. Access Control (AC) – 22 Controls 
**AC-1 through AC-22**
- Azure RBAC
- Managed Identity (AcrPull)
- Least-privilege access
- No embedded credentials

---

### 2. Awareness and Training (AT) – 2 Controls 
**AT-1 through AT-2**
- Documented secure DevSecOps practices
- Pipeline transparency

---

### 3. Audit and Accountability (AU) – 9 Controls 
**AU-1 through AU-9**
- Azure DevOps pipeline logs
- Immutable build IDs
- Scan and deployment audit trails

---

### 4. Configuration Management (CM) – 9 Controls 
**CM-1 through CM-9**
- Pipeline-as-code (YAML)
- Dockerfile-based builds
- Version-controlled configuration

---

### 5. Identification and Authentication (IA) – 11 Controls 
**IA-1 through IA-11**
- Azure AD authentication
- Managed Identity usage
- No shared secrets

---

### 6. Incident Response (IR) – 3 Controls 
**IR-1 through IR-3**
- Automated detection via SAST, CVE scanning, and DAST
- Pipeline failures act as incident triggers

---

### 7. Maintenance (MA) – 6 Controls 
**MA-1 through MA-6**
- Controlled updates via CI/CD
- No direct production changes

---

### 8. Media Protection (MP) – 9 Controls 
**MP-1 through MP-9**
- Private container registry
- No removable media usage

---

### 9. Personnel Security (PS) – 2 Controls 
**PS-1 through PS-2**
- Role-based access via Azure AD
- Identity-controlled permissions

---

### 10. Physical Protection (PE) – 6 Controls 
**PE-1 through PE-6**
- Azure datacenter physical security (inherited)

---

### 11. Risk Assessment (RA) – 3 Controls 
**RA-1 through RA-3**
- Continuous vulnerability identification
- Automated SAST, container scanning, and DAST

---

### 12. Security Assessment (CA) – 3 Controls 
**CA-1 through CA-3**
- Repeatable, auditable scans
- Pipeline and OpenVAS reports

---

### 13. System and Communications Protection (SC) – 16 Controls 
**SC-1 through SC-16**
- HTTPS-only endpoints
- Secure container networking
- Isolated registries

---

### 14. System and Information Integrity (SI) – 7 Controls 
**SI-1 through SI-7**
- Semgrep SAST
- Trivy CVE scanning
- OpenVAS DAST
- Controlled deployment pipeline

---

## Summary

- **CMMC Level 2 Controls Addressed:** 110 
- **Control Families:** 14 
- **Security Coverage:** SAST + Container Scanning + DAST 
- **Implementation Type:** Technical, auditable, and reproducible 

This project **does not claim CMMC certification**, but provides a **realistic technical implementation reference** showing how DevSecOps pipelines can support **CMMC Level 2 / NIST SP 800-171** requirements.

--- 