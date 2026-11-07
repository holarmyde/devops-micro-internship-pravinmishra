# Assignment 10 — Capstone: Deploy the Book Review App to Kubernetes

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

Deploy the Book Review App as a production-style three-tier application using Kubernetes manifests or Helm charts, with an accessible frontend, an internal backend, and a managed or persistent MySQL database.

---

## Submission Details

**Full Name:** Oluwafemi Aremu

**Kubernetes Platform:** Docker-Desktop

**Namespace:** book-review

**Frontend URL or IP:** https://app.docker.com/accounts/aremufemmy

**Access Method:** Public LoadBalancer, Ingress, or documented local access.

**GitHub Manifests or Helm Chart URL:** https://github.com/holarmyde?tab=repositories

**Architecture Diagram:** Add your diagram or link here.

---

# Task 1 — Prepare the Kubernetes Cluster and Project

## Goal

Prepare the cluster, application source, container images, and tooling needed for the deployment.

### Evidence

#### Screenshot 1 — Active Kubernetes context and Ready nodes

![task 1](screenshots/Screenshot%20with%20Active%20Kubernetes%20context%20and%20Ready%20nodes.png)

---

#### Screenshot 2 — If using Helm: Output of `helm version`

Required only if you choose Helm. Skip this screenshot if using YAML manifests.

Add your screenshot here, or write “Not applicable — using YAML manifests.”

---

# Task 2 — Design the Three-Tier Kubernetes Architecture

## Goal

Document the workloads, networking, configuration, storage, and permitted communication paths before deployment.

### Evidence

#### Screenshot 3 — Completed Kubernetes architecture diagram

![task 2](screenshots/Screenshot%20with%20Completed%20Kubernetes%20architecture%20diagram.png)

---

# Task 3 — Create ConfigMaps and Secrets

## Goal

Provide application configuration and credentials without embedding sensitive values in images or committing them to Git.

### Evidence

#### Screenshot 4 — ConfigMap configuration or describe output showing non-sensitive values

![task 3](screenshots/Screenshot%20with%20ConfigMap%20configuration%20or%20describe%20output_4.png)

---

#### Screenshot 5 — Secret references or describe output showing names and keys without sensitive values

![task 3](screenshots/Screenshot%20with%20Secret%20references%20or%20describe%20output%20showing_5.png)

---

# Task 4 — Deploy and Configure the MySQL Data Tier

## Goal

Provide persistent MySQL storage and allow the backend to connect without exposing the database publicly.

### Evidence

#### Screenshot 6 — Managed MySQL status or MySQL StatefulSet and PVC status

![task 4](screenshots/Screenshot%20with%20Managed%20MySQL%20status%20or%20MySQL%20StatefulSet%20and%20PVC%20status.png)

---

#### Screenshot 7 — Successful database connectivity or schema verification with credentials hidden

![task 4](screenshots/Screenshot%20with%20Successful%20database%20connectivity%20or%20schema.png)

---

# Task 5 — Deploy the Backend Application Tier

## Goal

Run one or two Node.js/Express backend replicas on port 3001 and expose them through an internal ClusterIP Service.

### Evidence

#### Screenshot 8 — Backend Deployment, ready Pods, and internal Service

![task 5](screenshots/Screenshot%20with%20Backend%20Deployment,%20ready%20Pods,%20and%20internal%20Service.png)

---

#### Screenshot 9 — Backend startup logs and database connectivity evidence

If the application does not log successful database connections, provide a successful database-backed request alongside the startup logs.

![task 5](screenshots/Screenshot%20with%20Backend%20startup%20logs%20and%20database%20connectivity%20evidence.png)

---

# Task 6 — Deploy the Frontend and Configure Nginx

## Goal

Run the Next.js frontend behind Nginx on port 80 and proxy API requests to the internal backend Service.

### Evidence

#### Screenshot 10 — Frontend Deployment and Ready Pods

![task 6](screenshots/Screenshot%20with%20Frontend%20Deployment%20and%20Ready%20Pods.png)

---

#### Screenshot 11 — Nginx reverse-proxy configuration showing the internal backend Service name and port 3001

![task 6](screenshots/Screenshot%20with%20Nginx%20reverse-proxy%20configuration%20showing%20d.png)

---

# Task 7 — Configure External and Internal Networking

## Goal

Expose the frontend and enforce the intended communication path between application tiers.

### Evidence

#### Screenshot 12 — Frontend LoadBalancer or Ingress configuration and assigned endpoint, or documented local access method

![task 7](screenshots/Screenshot%20with%20Frontend%20LoadBalancer%20or%20Ingress%20configuration.png)

---

#### Screenshot 13 — Internal backend Service and NetworkPolicy or equivalent restriction, with access-test evidence

![task 7](screenshots/Screenshot%20with%20Internal%20backend%20Service%20and%20NetworkPolicy.png)

---

# Task 8 — Validate the Application End to End

## Goal

Prove that the frontend, backend, and MySQL database work together and collect operational evidence.

### Evidence

#### Screenshot 14 — `kubectl get all` output for the book-review namespace

![task 8](screenshots/Screenshot%20with%20`kubectl%20get%20all`%20output%20for%20the%20book-review%20namespace.png)

---

#### Screenshot 15 — Functional Book Review App in the browser with the URL visible, including evidence of a successful user flow

![task 8](screenshots/Screenshot%20with%20`kubectl%20get%20all`%20output%20for%20the%20book-review%20namespace.png)

---

#### Screenshot 16 — Frontend or Nginx container logs

![task 8](screenshots/Screenshot%20with%20Frontend%20or%20Nginx%20container%20logs.png)

---

#### Screenshot 17 — Backend logs and evidence of a successful database-backed operation

![task 8](screenshots/Screenshot%20with%20Backend%20logs%20and%20evidence%20of%20a%20successful%20database.png)

---

#### Screenshot 18 — Optional: Kubernetes dashboard

![task 8](screenshots/Screenshot%20with%20Optional_%20Kubernetes%20dashboard.png)

Multiple images may be included under a screenshot number when needed to demonstrate the complete user flow.

---

# Task 9 — Optional: Implement Production Improvements

## Goal

Add selected improvements without breaking the required three-tier architecture.

### Evidence

#### Screenshot 19 — Optional: Evidence of selected Helm, HPA, probe, or TLS improvements

![task 9](screenshots/Screenshot%20with%20Optional_Implement%20Production%20Improvements.png)

---

# Task 10 — Publish Your Book Review App Deployment on LinkedIn

## Goal

Explain your three-tier architecture, demonstrate the working application, and share what you learned.

### Evidence

#### Screenshot 20 — Published LinkedIn post about your Book Review App deployment on Kubernetes

![LinkedIn](screenshots/Screenshot%20with%20kubernates%20linkedIn%20post_10.png)

### LinkedIn Post URL

`https://lnkd.in/p/ex9b6PtK`

---

# Final Summary

## What Worked

Describe the application components and user flows you successfully deployed and tested.

Successfully deployed the Book Review Application to a local Kubernetes environment using Docker Desktop.

The application consists of the following components:

Frontend: Next.js application serving the user interface.

Backend: Node.js and Express API handling registration, login, books, and reviews.

Database: MySQL deployed using a Kubernetes StatefulSet with persistent volume storage.

Reverse Proxy: Nginx routing browser requests to the frontend and backend services.

Kubernetes: Deployments, Pods, Services, ConfigMaps, Secrets, StatefulSets, and PersistentVolumeClaims used to manage the application.

The following functionality and infrastructure were verified:

MySQL database connected successfully.

Database schema and tables were initialized.

Three sample books were stored in the database.

Backend API returned all three books successfully with HTTP 200.

User registration and login worked successfully.

Frontend, backend, MySQL, and Nginx workloads were deployed and running.

The application was accessible through http://localhost:8080/ using Kubernetes port-forwarding.

Kubernetes resources and service configurations were inspected and documented.

One outstanding issue remained: although the backend API returned the three books successfully, the frontend homepage continued displaying "No books available." This was still under investigation.

## Issues and Fixes

Describe the main issues encountered, their causes, and how you resolved them.

Several issues were encountered during deployment and troubleshooting:

Database connectivity: The backend initially experienced database connection/configuration issues. The database configuration was reviewed, SSL settings were corrected for the local MySQL environment, and database connectivity was successfully established.

CORS errors: Registration and login initially failed because the backend did not allow requests originating from http://localhost:8080. The ALLOWED_ORIGINS ConfigMap value was updated to include the application origin, and the backend Deployment was restarted. Registration and login subsequently worked.

Frontend API configuration: The frontend API URL was affected by environment-variable configuration and Windows Git Bash path conversion. The ConfigMap was corrected to use /api, and the homepage request was changed to use the relative /api/books endpoint.

Homepage book listing: The backend API returned all three books, but the homepage continued displaying "No books available" after rebuilding and redeploying the frontend. Further browser Network and Console troubleshooting was required.

LoadBalancer external IP: Docker Desktop Kubernetes did not assign an external IP to the Nginx LoadBalancer Service. Port-forwarding was used to access the application locally at http://localhost:8080/.

These challenges provided practical experience in troubleshooting Kubernetes networking, application configuration, API routing, CORS, and container deployments.

## Tools and Sources Used

List the documentation and tools that helped you complete the assignment.

Tools:

Docker Desktop

Docker CLI

Kubernetes (kubectl)

Git and Git Bash

Visual Studio Code / Notepad

MySQL

Node.js and npm

Next.js

Express.js

Nginx

Browser Developer Tools

Documentation and learning sources:

Kubernetes Documentation: https://kubernetes.io/docs/

Docker Documentation: https://docs.docker.com/

MySQL Documentation: https://dev.mysql.com/doc/

Next.js Documentation: https://nextjs.org/docs

Express.js Documentation: https://expressjs.com/

Nginx Documentation: https://nginx.org/en/docs/

Project source repository: https://github.com/pravinmishraaws/book-review-app

## Optional Improvements

Describe any optional improvements implemented, or write “Not attempted.”

Not attempted beyond the configuration and troubleshooting work described above.

Potential future improvements include:

Installing and configuring Helm charts.

Implementing Horizontal Pod Autoscaling (HPA).

Adding or improving readiness and liveness probes.

Configuring TLS/HTTPS.

Setting up monitoring and centralized logging.

Completing the frontend homepage book-listing issue.

Implementing automated CI/CD deployment pipelines.

## Cleanup or Reuse Plan

State which resources were removed or why the environment is being retained.

The Kubernetes environment is being retained temporarily for further troubleshooting and verification of the application.

The Docker Desktop Kubernetes cluster, application Deployments, Services, MySQL StatefulSet, and persistent storage will remain available until the remaining frontend issue and final assignment evidence have been completed.

After final verification and submission, the environment can be cleaned up by deleting the book-review namespace and its associated resources. Persistent storage and any data required for future reuse should be backed up before cleanup.

---

# Submission Instructions

- Include your Kubernetes manifests or Helm chart in the linked repository.
- Submit Secret examples containing placeholders only.
- Include Screenshot 1, Screenshots 3–17, and Screenshot 20.
- Include Screenshot 2 only if using Helm.
- Screenshots 18–19 are optional.
- Keep screenshot numbering consistent with the guidelines.
- Use clear, readable screenshots showing relevant configuration, commands, and outputs.
- Make your full name visible in required screenshots.
- Complete the submission details and Final Summary.
- Include your published LinkedIn post URL.
- Capture required evidence before cleanup.
- Do not expose passwords, tokens, JWT secrets, actual Secret values, kubeconfig contents, cloud credentials, or confidential account information.

---

# Completion Checklist

- [✅] Selected and configured a supported Kubernetes platform
- [✅] Confirmed the context and Ready nodes (Screenshot 1)
- [✅] Created and selected the book-review namespace
- [✅] Inspected the application repository
- [✅ ] Prepared images accessible to the cluster
- [✅ ] Confirmed Helm if using charts (Screenshot 2, conditional)
- [✅ ] Created the architecture diagram (Screenshot 3)
- [✅ ] Created ConfigMaps and Secrets (Screenshots 4–5)
- [✅ ] Excluded actual Secret values from Git and screenshots
- [✅ ] Provisioned private managed MySQL or persistent Kubernetes MySQL (Screenshot 6)
- [✅ ] Initialized the required database and schema
- [✅ ] Verified database connectivity or schema (Screenshot 7)
- [✅ ] Deployed one or two backend replicas listening on port 3001
- [✅ ] Created the backend ClusterIP Service (Screenshot 8)
- [✅ ] Verified backend-to-database connectivity (Screenshot 9)
- [✅ ] Deployed one or two frontend replicas behind Nginx (Screenshot 10)
- [✅ ] Configured API proxying through the internal backend Service (Screenshot 11)
- [✅ ] Configured and documented frontend access (Screenshot 12)
- [✅ ] Enforced backend access restrictions (Screenshot 13)
- [✅ ] Confirmed that the backend and database are not publicly exposed
- [✅ ] Captured resource status for book-review (Screenshot 14)
- [✅ ] Tested application functionality and database writes (Screenshots 15 and 17)
- [✅ ] Verified saved data after application restart
- [✅ ] Captured frontend and backend logs (Screenshots 16–17)
- [✅ ] Included optional dashboard or improvement evidence if attempted (Screenshots 18–19)
- [✅ ] Published the LinkedIn post (Screenshot 20)
- [✅ ] Included the LinkedIn post URL
- [✅ ] Documented issues, fixes, and useful sources
- [✅ ] Recorded cleanup status or reuse plans
- [✅ ] No sensitive data exposed

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
