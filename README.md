# Thonmaker – Azure DevSecOps CI/CD (Docker → ACR → App Service) + CMMC Level 2 Mapping

Thonmaker is a containerized web application deployed through an end-to-end **Azure DevOps CI/CD pipeline**. This repo is built as a **portfolio-grade DevSecOps project** that demonstrates secure software delivery using **Docker**, **Azure Container Registry (ACR)**, **Azure App Service (Web App for Containers)**, and security scans (**Semgrep SAST** + **Trivy image scanning**), with **Managed Identity** and **RBAC** for registry access.

 Successful deployment validation page: **“Thonmaker Docker CI/CD working!”**

---

## What This Project Demonstrates

- Automated CI/CD with Azure DevOps Pipelines
- Docker image build and repeatable deployments
- Static Application Security Testing (SAST) with **Semgrep**
- Container vulnerability scanning with **Trivy**
- Private image storage in **ACR**
- Secure image pulls by App Service using **System-Assigned Managed Identity** + **AcrPull**
- Auditable build/deploy logs suitable for compliance evidence

---

## Architecture Overview

**Flow:**
1. Developer pushes code to `main`
2. Azure DevOps Pipeline runs
3. Semgrep scans source code (SAST)
4. Docker image is built and tagged with `$(Build.BuildId)`
5. Trivy scans the built image for CVEs
6. Image is pushed to ACR
7. App Service pulls the image from ACR using Managed Identity
8. App Service restarts container and serves the app

---

## Technologies Used

- **Azure DevOps Pipelines** – CI/CD automation
- **Self-hosted Windows build agent** – pipeline execution environment
- **Docker** – container build and packaging
- **Semgrep** – SAST (static code scanning)
- **Trivy** – container vulnerability scanning (CVEs)
- **Azure Container Registry (ACR)** – private image registry
- **Azure App Service (Linux Container)** – container hosting
- **Managed Identity + RBAC** – secure, passwordless ACR image pulls
- **Git** – version control

---

## CMMC Level 2 (NIST SP 800-171) – 110 Control Mapping

CMMC Level 2 aligns directly to **NIST SP 800-171**, which consists of **110 security requirements** across **14 control families**.  
This repository provides **technical implementation evidence** for these controls through secure CI/CD, container security, identity management, and audit logging.

Below is the complete **CMMC Level 2 control set** with implementation notes indicating how this project supports each family.

---

### 1. Access Control (AC) – 22 Controls
AC-1 through AC-22  
**Implementation Evidence:**
- Azure RBAC
- Managed Identity (AcrPull)
- Least privilege access to ACR and App Service
- No hard-coded credentials in pipeline or code

---

### 2. Awareness and Training (AT) – 2 Controls
AT-1 through AT-2  
**Implementation Evidence:**
- Secure DevOps practices documented in README
- Pipeline transparency for developer awareness

---

### 3. Audit and Accountability (AU) – 9 Controls
AU-1 through AU-9  
**Implementation Evidence:**
- Azure DevOps pipeline logs
- Build, scan, push, and deploy audit trails
- Immutable build IDs for traceability

---

### 4. Configuration Management (CM) – 9 Controls
CM-1 through CM-9  
**Implementation Evidence:**
- Infrastructure and pipeline defined as code (YAML)
- Dockerfile-based builds
- Version-controlled configuration in Git

---

### 5. Identification and Authentication (IA) – 11 Controls
IA-1 through IA-11  
**Implementation Evidence:**
- Azure AD authentication
- Managed Identity for service-to-service auth
- No shared secrets or passwords

---

### 6. Incident Response (IR) – 3 Controls
IR-1 through IR-3  
**Implementation Evidence:**
- Vulnerability detection via Semgrep and Trivy
- CI/CD failure conditions support incident detection workflows

---

### 7. Maintenance (MA) – 6 Controls
MA-1 through MA-6  
**Implementation Evidence:**
- Controlled updates through pipeline execution
- No direct production access; all changes via CI/CD

---

### 8. Media Protection (MP) – 9 Controls
MP-1 through MP-9  
**Implementation Evidence:**
- Container images stored in private ACR
- No local or removable media used for deployment artifacts

---

### 9. Personnel Security (PS) – 2 Controls
PS-1 through PS-2  
**Implementation Evidence:**
- Access controlled via Azure AD identities
- Role-based access enforcement

---

### 10. Physical Protection (PE) – 6 Controls
PE-1 through PE-6  
**Implementation Evidence:**
- Azure data center physical security (inherited control)

---

### 11. Risk Assessment (RA) – 3 Controls
RA-1 through RA-3  
**Implementation Evidence:**
- Continuous risk identification via SAST and container scanning
- CVE detection integrated into pipeline

---

### 12. Security Assessment (CA) – 3 Controls
CA-1 through CA-3  
**Implementation Evidence:**
- Pipeline execution logs
- Repeatable, auditable security scans
- Deployment validation page confirms operational state

---

### 13. System and Communications Protection (SC) – 16 Controls
SC-1 through SC-16  
**Implementation Evidence:**
- HTTPS-only App Service
- Secure container networking
- No exposed registry or backend services

---

### 14. System and Information Integrity (SI) – 7 Controls
SI-1 through SI-7  
**Implementation Evidence:**
- Semgrep SAST (static code analysis)
- Trivy container vulnerability scanning
- Detection of HIGH/CRITICAL CVEs
- Controlled deployment pipeline

---

## Summary

- **Total CMMC Level 2 Controls:** 110  
- **Control Families:** 14  
- **Implementation Type:** Technical + Engineering Evidence  
- **Assessment Readiness:** Portfolio-grade, auditable, and extensible

This project is **not a CMMC certification claim**, but a **technical implementation reference** demonstrating how DevSecOps pipelines can support and enforce CMMC Level 2 security requirements.


## Local Development

### Build the image locally
```bash
docker build -t thonmaker:local .
