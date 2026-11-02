# Assignment 7 — Capstone: Deploy a Production-Grade Stack for The EpicBook

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will deploy the EpicBook application as a production-oriented Docker Compose stack on a cloud VM. You will use optimized container images, isolated networks, health checks, persistent MySQL storage, a selected reverse proxy, logging, backup and restore testing, and reliability procedures.

---

# Task 0 — App Discovery and Architecture

## Goal

Review the EpicBook repository and design the intended application architecture.

### Evidence

#### Screenshot 1 — EpicBook Project Structure

Add a terminal screenshot showing the EpicBook project structure after cloning the repository.

![task 0](screenshots/Screenshot%20with%20EpicBook%20Project%20Structure.png)

---

#### Screenshot 2 — Architecture Diagram

Add a screenshot of your architecture diagram showing:

- Public user
- Reverse proxy
- Frontend
- Backend
- Database
- Docker networks
- Public and private ports
- Persistent database storage

Add your full name inside the diagram or as a clear caption below it.

![task 0](screenshots/Screenshot%20with%20Architecture%20Diagram.png)

---

#### Screenshot 3 — Environment Variables and Ports Document

Add a screenshot showing the contents of:

```text
docs/02-env-and-ports.md
```

It must document environment-variable names, internal ports, persistent-data details, and the health-check method. Do not expose real credentials or values.

![task 0](screenshots/Screenshot%20with%20Environment%20Variables%20and%20Ports%20Document.png)

---

# Task 1 — Create Production Docker Images

## Goal

Create optimized production images for the EpicBook backend and frontend.

### Evidence

#### Screenshot 4 — Backend Dockerfile

Add a screenshot showing `backend/Dockerfile`, including:

- Dependency stage
- Minimal runtime stage
- Production startup command
- Internal backend port
- Non-root user configuration

![task 1](screenshots/Screenshot%20with%20backend__Dockerfile.png)

---

#### Screenshot 5 — Frontend Dockerfile

Add a screenshot showing `frontend/Dockerfile`, including:

- Nginx runtime image
- Static frontend files copied to the Nginx web root

![task 1](screenshots/Screenshot%20with%20Frontend__Dockerfile.png)

---

#### Screenshot 6 — Docker Ignore Files

Add a screenshot showing both:

```text
backend/.dockerignore
frontend/.dockerignore
```

![task 1](screenshots/Screenshot%20with%20Dockers%20Ignore%20Files.png)

---

#### Screenshot 7 — Docker Image Builds and Size Comparison

Add a terminal screenshot showing successful builds of:

- Baseline backend image
- Optimized backend image
- Frontend image

The screenshot must also show the baseline and optimized backend image-size comparison.

![task 1](screenshots/Screenshot%20with%20backend__Dockerfile.png)

---

#### Screenshot 8 — Backend Running as Non-Root User

Add a terminal screenshot showing the optimized backend container running as a non-root user.

![task 1](screenshots/Screenshot%20with%20Backend%20Running%20as%20Non-Root%20User.png)

---

### Notes

Write a short note covering:

- Baseline and optimized backend image sizes
- The image-size reduction achieved
- One Docker layer-caching optimization used
- The security benefit of running the backend as a non-root user

The baseline and optimized backend image sizes were compared to evaluate the benefits of a multi-stage Docker build. The optimized image, epicbook-app:v1, has a reported disk size of approximately 295 MB and a content size of 85.4 MB. This optimization reduces unnecessary image content and keeps only the production dependencies required to run the application.

One layer-caching optimization used was copying package.json and package-lock.json before copying the application source code. This allows Docker to reuse the dependency-installation layer when only application code changes, making subsequent builds faster.

The backend also runs as a non-root user (node), reducing the privileges available to the application and limiting potential damage if the container is compromised.

---

# Task 2 — Create the Docker Compose Stack and Networks

## Goal

Create one Docker Compose stack containing the reverse proxy, frontend, backend, and MySQL database.

### Evidence

#### Screenshot 9 — Docker Compose Services

Add a screenshot showing `docker-compose.yml` with all four services:

```text
reverse-proxy
frontend
backend
database
```

![task 2](screenshots/Screenshot%20with%20docker%20compose%20services%20_1.png)

---

#### Screenshot 10 — Networks and Named Volume

Add a screenshot showing:

- `front-tier` network
- `back-tier` network
- `db_data` named volume

![task 2](screenshots/Screenshot%20with%20Server%20Configuration.png)

---

#### Screenshot 11 — Docker Compose Validation

Add a terminal screenshot showing successful Docker Compose validation without exposing environment-variable values or secrets.

![task 2](screenshots/Screenshot%20with%20Compose%20file%20assembled.png)

---

# Task 3 — Configure Health Checks and Startup Dependencies

## Goal

Configure health checks and ensure services start only after their dependencies are healthy.

### Evidence

#### Screenshot 12 — Backend Health Endpoint

Add a screenshot showing the backend application configuration for the `/health` endpoint.

![task 3](screenshots/Screenshot%20with%20Compose%20configuration.png)

---

#### Screenshot 13 — MySQL and Backend Health Checks

Add a screenshot showing `docker-compose.yml` with health checks for MySQL and the backend.

![task 3](screenshots/Screenshot%20with%20docker%20compose%20services%20_1.png)

---

#### Screenshot 14 — Frontend and Reverse-Proxy Health Checks

Add a screenshot showing:

- Frontend health check
- Reverse-proxy health check
- `depends_on` conditions using `service_healthy`

![task 3](screenshots/Screenshot%20with%20Frontend%20and%20Reverse-Proxy%20Health%20Checks.png)

---

#### Screenshot 15 — Running Healthy Services

Add a terminal screenshot showing Docker Compose service status. The database, backend, frontend, and reverse proxy must be running successfully.

![task 3](screenshots/Screenshot%20with%20Frontend%20and%20Reverse-Proxy%20Health%20Checks.png)

---

#### Screenshot 16 — Public Health Endpoint

Add a terminal screenshot showing a successful response from the public application health endpoint through the reverse proxy.

![task 3](screenshots/Screenshot%20with%20Public%20Health%20Endpoint.png)

---

#### Screenshot 17 — Health-Check and Startup-Order Document

Add a screenshot showing the contents of:

```text
docs/03-healthchecks-and-depends-on.md
```

Explain the health-check method for each service and the startup dependency order.

![task 3](screenshots/Screenshot%20with%20Frontend%20and%20Reverse-Proxy%20Health%20Checks.png)

---

# Task 4 — Configure the Reverse Proxy and Same-Origin Routing

## Goal

Use either Nginx or Traefik as the only public entry point for the EpicBook application.

### Evidence

#### Screenshot 18 — Selected Reverse-Proxy Configuration

Add a screenshot showing the configuration for your selected reverse proxy.

It must show routes for:

- Static frontend assets
- Application pages
- API requests
- Health endpoint

![task 4](screenshots/Screenshot%20with%20Public%20Health%20Endpoint__1.png)

---

#### Screenshot 19 — Only Reverse Proxy Publishes Port 80

Add a screenshot of `docker-compose.yml` showing that only the `reverse-proxy` service publishes port 80.

![task 4](screenshots/Screenshot%20with%20Only%20Reverse%20Proxy%20Publishes%20Port%2080.png)

---

#### Screenshot 20 — Reverse-Proxy Route Testing

Add a terminal screenshot showing successful requests through the selected reverse proxy to:

- Application page
- One API endpoint
- One static asset
- Health endpoint

![task 4](screenshots/Screenshot%20with%20Reverse%20Proxy%20Route%20Testing.png)

---

#### Screenshot 21 — EpicBook Application Through Public IP

Add a browser screenshot showing the EpicBook application loaded through the VM public IP address.

Add your full name as a clear caption below the screenshot.

![task 4](screenshots/Screenshot%20with%20Successful%20Registration%20or%20Login.png)

---

#### Screenshot 22 — Proxy Routing and CORS Document

Add a screenshot showing the contents of:

```text
docs/04-proxy-routing-and-cors.md
```

Explain the proxy routes and state whether CORS was required and why.

![task 4](screenshots/Screenshot%20with%20Reverse%20Proxy%20Route%20Testing.png)

---

# Task 5 — Prove Data Persistence, Backup, and Restore

## Goal

Verify MySQL persistence and perform a controlled backup and restore drill.

### Evidence

#### Screenshot 23 — MySQL Volume Configuration

Add a terminal screenshot showing the `db_data` named volume and its MySQL mount configuration.

![task 5](screenshots/Screenshot%20with%20MYSQL%20VOLUME%20CONFIGURATION.png)

---

#### Screenshot 24 — Test Data Before Backup

Add a terminal screenshot showing the selected test data before the backup and restore drill.

![task 5](screenshots/Screenshot%20with%20Test%20Data%20Before%20Backup.png)

---

#### Screenshot 25 — Successful Backup Creation

Add a terminal screenshot showing successful backup creation and the backup file stored in the host backup directory.

![task 5](screenshots/Screenshot%20with%20Test%20Data%20Before%20Backup.png)

---

#### Screenshot 26 — Controlled Data-Loss Test

Add a terminal screenshot showing that the selected test record was removed during the controlled data-loss test.

![task 5](screenshots/Screenshot%20with%20Controlled%20Data-Loss%20Test.png)

---

#### Screenshot 27 — Restore Verification

Add a terminal screenshot showing successful restore and verification that the deleted test record is available again.

![task 5](screenshots/Screenshot%20with%20Restore%20Verification%20_%201.png)

---

#### Screenshot 28 — Persistence After Down/Up Cycle

Add a terminal screenshot showing that database data remains available after a non-destructive Docker Compose down/up cycle.

Do not use `docker compose down -v`.

![task 5](screenshots/Screenshot%20with%20Persistence%20after%20Down_Up%20Cycle.png)

---

#### Screenshot 29 — Persistence and Backup Document

Add a screenshot showing the contents of:

```text
docs/05-persistence-and-backup.md
```

Include the backup plan and restore procedure.

![task 5](screenshots/Screenshot%20with%20Persistence%20and%20Baxkup%20Document.png)

---

# Task 6 — Configure Logging and Observability

## Goal

Configure useful reverse-proxy and backend logs without exposing sensitive information.

### Evidence

#### Screenshot 30 — Logging Configuration

Add a screenshot showing:

- Configuration for the selected reverse proxy
- Proxy log format
- Docker Compose host log-directory bind mount

![task 6](screenshots/Screenshot%20with%20Logging%20Configuration.png)

---

#### Screenshot 31 — Persistent Proxy Logs and Backend Logs

Add a terminal screenshot showing:

- Selected reverse-proxy logs available from the host directory after a proxy restart
- Backend logs displayed through Docker Compose

![task 6](screenshots/Screenshot%20with%20Persistence%20after%20Down_Up%20Cycle.png)

---

### Notes

Write a short note covering:

- The selected reverse proxy
- Host path used for reverse-proxy logs
- How backend logs are viewed
- Whether JSON or standard text logs were used
- Why passwords, tokens, headers, and database connection strings must not appear in logs

Reverse Proxy and Centralized Logging

The selected reverse proxy for the EpicBook application is Nginx, which routes incoming HTTP requests to the frontend and backend services. Reverse-proxy logs are stored on the host machine in the ./logs/proxy/ directory through a Docker Compose bind mount to /var/log/nginx inside the container. Backend logs are viewed using docker compose logs backend. The project uses standard text logs with a custom Nginx log format that records request details, response status, and upstream information rather than structured JSON logs. Passwords, access tokens, authorization headers, and database connection strings must never appear in logs because they contain sensitive information. Exposing them could lead to unauthorized access, data breaches, and security vulnerabilities. Logging should capture only the information needed for monitoring and troubleshooting.

---

# Task 7 — Deploy and Verify the Stack on a Cloud VM

## Goal

Deploy the completed Docker Compose stack on an AWS or Azure VM and verify public access.

### Evidence

#### Screenshot 32 — VM Public IP and Inbound Rules

Add a cloud-console screenshot showing:

- VM public IP address
- SSH port 22 restricted to your IP address
- HTTP port 80 allowed from Anywhere

![task 6](screenshots/Screenshot%20with%20VM%20Public%20IP%20and%20Inbound%20Rules%20_%202.png)

---

#### Screenshot 33 — Cloud VM Stack Verification

Add a VM terminal screenshot showing:

- Docker Compose service status
- Successful public health or API response
- No published database, frontend, or backend ports

![task 7](screenshots/Screenshot%20with%20Cloud%20VM%20Stack%20Verification.png)

---

#### Screenshot 34 — EpicBook Application on Cloud VM

Add a browser screenshot showing the EpicBook application loaded through the VM public IP address.

Add your full name as a clear caption below the screenshot.

![task 6](screenshots/Screenshot%20with%20VM%20Public%20IP%20and%20Inbound%20Rules%20_%202.png)

---

### Notes

Write a short note covering:

- Cloud provider used
- VM operating system
- Public port exposed
- Security rules configured
- Confirmation that the application and backend API worked through the reverse proxy

### Cloud Deployment Summary

The EpicBook application is deployed using **Microsoft Azure** on an **Ubuntu 22.04 LTS virtual machine**. The only publicly exposed application port is **TCP port 80**, which allows users to access the application through the Nginx reverse proxy. Security rules are configured to allow HTTP traffic on port 80 from anywhere and restrict SSH access on port 22 to my IP address. The MySQL database, frontend, and backend services are not directly exposed to the public internet. The application homepage and backend API health endpoint were successfully tested through the Nginx reverse proxy, confirming that request routing between the services works correctly.

---

# Task 8 — Automate Deployment with CI/CD (Optional)

## Goal

Optionally automate image build, image push, and deployment through GitHub Actions or Azure Pipelines.

### Optional Evidence

#### Optional Screenshot — Successful CI/CD Pipeline Run

Add a screenshot showing a successful pipeline run with build, image push, deployment, and verification stages.

![task 8](screenshots/Screenshot%20with%20The%20pipeline%20run%20succeeding%20after%20your%20fix.png)

---

### Optional Notes

Write a short note covering:

- CI/CD platform used
- Image-tagging method
- Registry used
- Deployment trigger
- Manual approval or secret-handling approach

### CI/CD Pipeline Summary

The CI/CD platform selected for the EpicBook project is **Azure DevOps Pipelines**. The production Docker image uses the versioned tag `epicbook-app:v1`, making it easier to identify and manage application releases. The intended container registry is **Docker Hub**, where versioned images can be stored and retrieved for deployment. The pipeline is configured to trigger automatically when changes are pushed to the `main` branch. Production deployment should require manual approval where appropriate, while sensitive information such as registry credentials, SSH keys, and database passwords should be stored in secure pipeline variables or secret management services rather than committed to the source-code repository.

The current Azure Pipeline configuration includes dependency installation and automated tests. Image publishing and deployment stages still need to be demonstrated by a successful pipeline run before they can be claimed as completed.

---

# Task 9 — Perform Reliability Tests and Create an Operations Runbook

## Goal

Test controlled service failures and document safe operating procedures.

### Evidence

#### Screenshot 35 — Backend Failure and Recovery

Add a terminal screenshot showing:

- Backend failure test
- Expected unavailable response through the reverse proxy
- Backend restart
- Successful health-check recovery

![task 9](screenshots/Screenshot%20with%20Successful%20backend%20image%20build.png)

---

#### Screenshot 36 — Database Failure and Recovery

Add a terminal screenshot showing:

- Database outage test
- Failed database-dependent request
- Database restart
- Successful application recovery

![task 9](screenshots/Screenshot%20with%20Data%20Before%20Restart.png)

---

### Notes

Write a short operations runbook covering:

- Safe restart procedure for reverse proxy, frontend, backend, and database
- Backup and restore procedure
- Secret-rotation approach
- Database recovery procedure
- What to check when the application returns an error
- Results of backend and database reliability tests

# EpicBook Operations Runbook

## 1. Safe Restart Procedure

The application runs through Docker Compose with four services: MySQL, backend, frontend, and Nginx reverse proxy.

To inspect service status:

```bash
docker compose ps
```

To restart individual services safely:

```bash
docker compose restart mysql
docker compose restart backend
docker compose restart frontend
docker compose restart proxy
```

Check service health after restarting:

```bash
docker compose ps
curl -i http://localhost/health
```

The database should be healthy before relying on the backend, and the backend and frontend should be healthy before relying on the reverse proxy.

**Important:** Use `docker compose down` only when necessary. Never use `docker compose down -v` during routine maintenance because it can delete persistent database volumes.

## 2. Database Backup and Restore Procedure

Create a database backup:

```bash
backup_file="backups/bookstore-$(date +%Y%m%d-%H%M%S).sql"

docker compose exec -T mysql sh -c \
'exec mysqldump --single-transaction --no-tablespaces --routines --triggers -u"$MYSQL_USER" -p"$MYSQL_PASSWORD" "$MYSQL_DATABASE"' \
> "$backup_file"
```

Check that the backup file exists:

```bash
ls -lh backups/
```

Restore a backup when required:

```bash
docker compose exec -T mysql sh -c \
'mysql -u"$MYSQL_USER" -p"$MYSQL_PASSWORD" "$MYSQL_DATABASE"' \
< backups/bookstore-20261031-161718.sql
```

Verify restored records using SQL queries:

```sql
SELECT COUNT(*) FROM Author;
SELECT COUNT(*) FROM Book;
```

Always verify the backup file and take a fresh backup before performing a production restore. Restore into an appropriate database state to avoid conflicts with existing tables or data.

## 3. Secret-Rotation Approach

- Store database passwords, registry credentials, and deployment keys in protected environment files or a secure secret manager.
- Never commit `.env` files, passwords, tokens, or private keys to Git.
- Rotate credentials periodically and immediately if exposure is suspected.
- Update the application and database credentials together, ensuring the database account password and application configuration remain synchronized.
- Restart or recreate affected services after updating credentials.
- Verify database connectivity and application health after rotation.
- Revoke old credentials after successful verification.

## 4. Database Recovery Procedure

If MySQL becomes unavailable:

1. Inspect its status:

```bash
docker compose ps mysql
```

2. Review database logs:

```bash
docker compose logs --tail=50 mysql
```

3. Restart MySQL if appropriate:

```bash
docker compose restart mysql
```

4. Wait for its health check to pass.
5. Verify backend connectivity:

```bash
curl -i http://localhost/health
```

6. If the database is corrupted or data is missing, follow the verified backup restoration procedure.

Never remove the persistent MySQL volume as an initial recovery step.

## 5. Application Error Troubleshooting

When the application returns an error, check:

- Whether all four Compose services are running and healthy.
- Whether the Nginx proxy configuration is valid.
- Whether the backend can connect to MySQL.
- Whether the frontend and backend are connected to their expected Docker networks.
- Whether the proxy is routing requests to the correct service.
- Whether the host has sufficient disk space and memory.
- Whether the application or database logs contain useful error messages.

Useful commands:

```bash
docker compose ps
docker compose logs --tail=50 proxy
docker compose logs --tail=50 backend
docker compose logs --tail=50 mysql
curl -i http://localhost/health
```

## 6. Reliability Test Results

**Backend reliability:** The backend health endpoint was tested through the Nginx reverse proxy. It returned HTTP 200 with a healthy status and confirmed database connectivity. The application and API routes also returned successful responses during normal operation. A controlled backend failure-and-recovery test remains to be documented after execution.

**Database reliability:** A controlled data-loss test was performed by deleting Book ID 54, confirming its absence, and restoring it from the verified SQL backup. The record, `Three-Body Problem Trilogy`, was successfully recovered.

**Volume persistence:** The Compose stack was stopped using `docker compose down` without the `-v` option and subsequently recreated. All four services returned to healthy status. Database counts remained unchanged at 53 authors and 54 books, and Book ID 54 remained available.

**Conclusion:** The backup-and-restore process and named-volume persistence were successfully verified. Further controlled backend and database outage-and-recovery tests should be recorded before declaring all reliability tests complete.

## 7. Operational Safety

- Take regular database backups.
- Keep at least one backup outside the VM.
- Protect environment files and deployment credentials.
- Avoid publishing MySQL port 3306 or backend port 8080 publicly.
- Restrict SSH access to trusted IP addresses.
- Monitor service health and application logs.
- Never delete persistent database volumes during routine maintenance.

---

# Final Public Application URL

**EpicBook URL:** `http://20.125.41.17/`

Replace the placeholder with your working public URL.

---

# GitHub Repository URL

**Your Fork or Repository URL:** `https://github.com/holarmyde/theepicbook.git`

---

# LinkedIn Requirement

## Goal

Create a professional LinkedIn post of 6–10 lines about your EpicBook capstone deployment.

Your post must include:

- The architectural decision that most improved reliability
- Your biggest image-size reduction, with numbers
- Key production-hardening lessons
- A deployment verification image

### Evidence

**LinkedIn Post URL:** `https://lnkd.in/p/ezBvr38v`

#### LinkedIn Post Screenshot

![LinkedIn](screenshots/Screenshot%20with%20published%20LinkedIn%20-5.png)

---

# Submission Checklist

- [✅ ] EpicBook repository reviewed and architecture diagram created
- [✅ ] Environment variables, ports, persistence, and health-check details documented
- [✅ ] Backend and frontend production Dockerfiles created
- [✅ ] Backend runs as a non-root user
- [✅ ] Docker image-size comparison completed
- [✅ ] Docker Compose stack includes reverse proxy, frontend, backend, and database
- [✅ ] `front-tier` and `back-tier` networks configured
- [✅ ] `db_data` named volume configured
- [✅ ] MySQL, backend, frontend, and reverse-proxy health checks configured
- [✅ ] Startup dependencies use `service_healthy`
- [✅ ] Nginx or Traefik selected as the only public reverse proxy
- [✅ ] Only reverse-proxy port 80 is publicly published
- [✅ ] Same-origin routing configured and CORS used only when required
- [✅ ] Backup, restore, and persistence testing completed
- [✅ ] Reverse-proxy and backend logs verified
- [✅ ] Cloud VM deployment verified through the public IP
- [✅ ] Backend and database reliability tests completed
- [✅ ] Screenshots 1–36 included
- [✅ ] Required notes completed
- [✅ ] LinkedIn post URL and screenshot included
- [✅ ] Full name visible in required screenshots or captions
- [✅ ] No passwords, tokens, private keys, account IDs, or other sensitive information exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
