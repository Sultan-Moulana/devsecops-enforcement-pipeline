# Automated DevSecOps CI/CD Enforcement Pipeline

## Objective
Designed and implemented an automated security enforcement pipeline to integrate continuous security testing directly into the Software Development Life Cycle (SDLC). This architecture demonstrates a "shift-left" security approach, ensuring that vulnerable code and dependencies are blocked by automated Quality Gates before reaching a staging or production environment.

## Architecture & Tool Stack
* **CI/CD Orchestration:** Jenkins (Containerized via Docker)
* **Static Application Security Testing (SAST):** SonarQube Community Edition
* **Software Composition Analysis (SCA):** AquaSec Trivy
* **Dynamic Application Security Testing (DAST):** OWASP ZAP (Zed Attack Proxy)
* **Infrastructure as Code:** Docker Compose

## Pipeline Workflow
The pipeline targets a purposely vulnerable Node.js web application (OWASP Juice Shop) to demonstrate the automated detection and blocking of security flaws.

1. **Code Checkout:** Fetches the target application codebase from the repository.
2. **SAST Analysis (SonarQube):** Performs static code analysis to identify hardcoded secrets, injection flaws, and insecure configurations. 
3. **SAST Quality Gate:** The pipeline pauses to await a webhook response from SonarQube. If critical vulnerabilities are detected, the build is instantly aborted.
4. **Image Build:** Containerizes the application code using Docker.
5. **SCA Scan (Trivy):** Scans the built Docker image layers for outdated or vulnerable open-source dependencies. Enforces a strict Quality Gate by returning `exit-code 1` to crash the pipeline if any `CRITICAL` severity CVEs are found.
6. **Staging Deployment:** Dynamically spins up the containerized application on a temporary port.
7. **DAST Scan (OWASP ZAP):** Executes an active baseline spider and scan against the live container to identify runtime vulnerabilities (e.g., missing security headers, Cross-Origin misconfigurations). Extracts a comprehensive HTML security report for review.
8. **Teardown & Archival:** Destroys the temporary staging environment and archives the ZAP security report as a build artifact in Jenkins.

## Business Value
By implementing hard Quality Gates at both the static analysis and container scanning stages, this architecture prevents known vulnerabilities from being merged or deployed. It bridges the gap between development and security, reducing the organizational attack surface without compromising deployment velocity.

---
### About the Developer
This project was engineered to showcase practical, hands-on DevSecOps implementation. I hold a First Class BSc (Hons) in Computer Science from the University of Westminster and a CompTIA Security+ certification. My background includes a year of hands-on experience as a Software Engineer leveraging Jenkins, Docker, and SonarQube for CI/CD deployments and QA automation, which I am now fully pivoting toward specialized cybersecurity and application security architecture.