# Assignment 6 — Deploy the Book Review App with Docker Compose

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will deploy the Book Review Application with Docker Compose using MySQL, a backend API, and a frontend user interface. You will configure health-gated startup, browser-facing API access, CORS, and persistent MySQL storage.

---

# Task 1 — Prepare the Project

## Goal

Prepare your fork of the Book Review App repository for Docker Compose deployment.

### Evidence

#### Screenshot 1 — Project Structure

Add a screenshot showing the project structure containing:

```text
frontend/
backend/
.env.example
.gitignore
docker-compose.yml
```

![task 1](screenshots/Screenshot%20with%20docs01-architecture-diagram.png)

---

#### Screenshot 2 — Environment and Docker Ignore Files

Add a screenshot showing the contents of:

```text
.env.example
.gitignore
frontend/.dockerignore
backend/.dockerignore
```

Ensure that no real passwords, tokens, or secrets are visible.

![task 1](screenshots/Screenshot%20with%20docs02-env-and-ports.md.png)

---

# Task 2 — Create or Confirm Application Dockerfiles

## Goal

Prepare Dockerfiles for the frontend and backend services and build both services through Docker Compose.

### Evidence

#### Screenshot 3 — Frontend Dockerfile

Add a screenshot showing the completed `frontend/Dockerfile`.

![task 2](screenshots/Screenshot%20with%20Add%20a%20screenshot%20showing%20the%20completed%20frontendDockerfile_R.png)

---

#### Screenshot 4 — Backend Dockerfile

Add a screenshot showing the completed `backend/Dockerfile`.

![task 2](screenshots/Screenshot%20with%20Add%20a%20screenshot%20showing%20the%20completed%20frontendDockerfile.png)

---

#### Screenshot 5 — Docker Compose Build

Add a screenshot of the terminal showing successful completion of:

```bash
docker compose build
```

![task 2](screenshots/Screenshot%20with%20Docker%20Compose%20Build.png)

---

# Task 3 — Create the Docker Compose Stack

## Goal

Create one `docker-compose.yml` file that builds and runs MySQL, backend, and frontend services.

### Evidence

#### Screenshot 6 — MySQL Service, Health Check, and Volume Mount

Add a screenshot showing the MySQL service in `docker-compose.yml`, including:

- MySQL image
- Environment variables
- MySQL health check
- `mysql_data` volume mount
- No published MySQL port

![task 3](screenshots/Screenshot%20with%20Docker%20Compose%20Build.png)

---

#### Screenshot 7 — Backend Configuration

Add a screenshot showing the backend service configuration, including:

- `depends_on` with `condition: service_healthy`
- Database host set to `mysql`
- Browser frontend origin configured for CORS
- Published backend port

![task 3](screenshots/Screenshot%20with%20Backend%20Configuration_tt.png)

---

#### Screenshot 8 — Frontend Configuration

Add a screenshot showing the frontend service configuration, including:

- Published frontend port
- `depends_on` for the backend service
- Browser-facing `NEXT_PUBLIC_API_URL`

![task 3](screenshots/Screenshot%20with%20condition%20service_healthy.png)

---

#### Screenshot 9 — Named Volume Definition

Add a screenshot showing the `mysql_data` volume definition in `docker-compose.yml`.

![task 3](screenshots/Screenshot%20with%20condition%20service_healthy.png)

---

# Task 4 — Start and Verify the Stack

## Goal

Build and start all services through one Docker Compose workflow.

### Evidence

#### Screenshot 10 — Docker Compose Service Status

Add a screenshot of the terminal showing:

```bash
docker compose ps
```

The output must show the MySQL, backend, and frontend services running. MySQL must show as healthy.

![task 4](screenshots/Screenshot%20with%20Docker%20Compose%20Service%20Status.png)

---

#### Screenshot 11 — MySQL and Backend Logs

Add a screenshot of the terminal showing:

```bash
docker compose logs mysql backend --tail=50
```

The logs must show MySQL readiness and successful backend database connection.

Add your screenshot here.

---

# Task 5 — Test End-to-End Application Functionality

## Goal

Verify that the Book Review App works through the browser.

### Evidence

#### Screenshot 12 — Successful Registration or Login

Add a browser screenshot showing successful user registration or login.

Add your full name as a clear caption directly below the screenshot.

![task 5](screenshots/Screenshot%20with%20Successful%20Registration%20or%20Login.png)

---

#### Screenshot 13 — Created Book Review

Add a browser screenshot showing a created book review visible in the application.

Add your full name as a clear caption directly below the screenshot.

![task 5](screenshots/Screenshot%20with%20Created%20Book%20Review.png)

---

#### Screenshot 14 — CORS Verification

Add a browser developer-tools screenshot with:

- The Network tab showing a successful API request
- The Console drawer showing no CORS error after the API interaction

![task 5](screenshots/Screenshot%20with%20CORS%20Verification.png)

---

# Task 6 — Prove MySQL Data Persistence

## Goal

Verify that MySQL data remains after a non-destructive Docker Compose down/up cycle.

### Evidence

#### Screenshot 15 — Data Before Restart

Add a browser screenshot showing the registered user or created review before the down/up cycle.

Add your full name as a clear caption directly below the screenshot.

![task 6](screenshots/Screenshot%20with%20Data%20Before%20Restart.png )

---

#### Screenshot 16 — Non-Destructive Stack Restart

Add a screenshot of the terminal showing the non-destructive shutdown and restart:

```bash
docker compose down
docker compose up -d
docker compose ps
```

Do not use `docker compose down -v`.

![task 6](screenshots/Screenshot%20with%20Non-Destructive%20Stack%20Restart.png)

---

#### Screenshot 17 — Data After Restart

Add a browser screenshot showing the same registered user or review after the stack restarts.

Add your full name as a clear caption directly below the screenshot.

![task 6](screenshots/Screenshot%20with%20Successful%20Registration%20or%20Login.png)

---

# Task 7 — Explain Docker Compose Teardown Modes

## Goal

Explain the difference between preserving data and fully resetting a Docker Compose environment.

### Notes

Write a short explanation of 5–8 lines covering:

- What `docker compose down` removes and preserves
- Why named volumes should be kept when preserving MySQL data
- What happens when named volumes are removed
- When a full reset is useful
- Why a full reset must not be used before persistence evidence is captured

The `docker compose down` command removes the application containers and the Compose network but preserves named volumes by default. Named volumes should be kept because they store MySQL database files independently of the containers, ensuring data remains available after containers are recreated. When named volumes are removed using `docker compose down -v`, the stored database data is deleted. A full reset is useful when troubleshooting database issues, starting with a clean environment, or testing a fresh deployment. However, it must not be performed before persistence evidence is captured because it permanently removes the data needed to demonstrate that database records survive container removal and recreation.

---

# Final Public Frontend URL

**Frontend URL:** `http://20.125.41.17:3000`

Replace the placeholder with your working application URL.

---

# GitHub Repository URL

**Your Fork or Repository URL:** `https://github.com/holarmyde/book-review-app.git`

---

# LinkedIn Requirement

## Goal

Create a LinkedIn post about the Book Review App deployment and what you learned from using Docker Compose.

### Evidence

**LinkedIn Post URL:** `https://lnkd.in/p/eM68fy_i`

#### LinkedIn Post Screenshot

![LinkedIn](screenshots/Screenshot%20with%20LinkedIn%20Post%20Screenshot.png)

---

# Submission Checklist

- [✅ ] Book Review App repository forked and used
- [✅ ] `.env` excluded from Git tracking
- [✅ ] `.env.example` contains only safe placeholder values
- [✅ ] Frontend and backend Dockerfiles created or confirmed
- [✅ ] MySQL health check configured
- [✅ ] Backend waits for healthy MySQL
- [✅ ] Backend uses `mysql` as the database hostname
- [✅ ] Frontend API URL uses the VM public IP and backend port
- [✅] Backend CORS origin matches the frontend origin
- [✅] MySQL port 3306 is not publicly exposed
- [✅ ] Registration and login work
- [✅ ] Book review creation works
- [✅] Data persists after a non-destructive down/up cycle
- [✅] Screenshots 1–17 included
- [✅] Teardown explanation completed
- [✅] Public frontend URL included
- [✅] GitHub repository URL included
- [✅] LinkedIn post URL and screenshot included
- [✅] Full name visible in required terminal screenshots
- [✅] Browser screenshots include a full-name caption
- [✅] No sensitive information exposed

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
