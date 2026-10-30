# Assignment 4 — Docker Volumes

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will learn how Docker Volumes and Bind Mounts provide persistent storage for containers, deploying applications that retain logs and data even after containers are stopped or removed.

---

# Task 1 — Deploy a Standalone Application with Persistent Logs Using Bind Mounts

## Goal

Run an Nginx container (`myweb`) with a Bind Mount from `~/nginx-logs` to `/var/log/nginx`, verify the site loads, confirm logs are written, remove the container, and confirm the logs persist on the host.

### Evidence

#### Screenshot 1 — Output of `docker images`

![task 1](screenshots/Screenshot%20with%20Output%20of%20docker%20images_1.png)

---

#### Screenshot 2 — Output of `docker search nginx`

![task 1](screenshots/Screenshot%20with%20Output%20of%20docker%20search%20nginx_1.png)

---

#### Screenshot 3 — Successful `docker pull nginx` (if applicable)

![task 1](screenshots/Screenshot%20with%20Successful%20docker%20pull%20nginx%20(if%20applicable).png)

---

#### Screenshot 4 — Creation of the `~/nginx-logs` directory

![task 1](screenshots/Screenshot%20with%20Creation%20of%20the%20_nginx-logs%20directory.png)

---

#### Screenshot 5 — Output of `docker run` with the Bind Mount

![task 1](screenshots/Screenshot%20with%20Output%20of%20docker%20run%20with%20the%20Bind%20Mount.png)

---

#### Screenshot 6 — Output of `docker ps` showing the running `myweb` container

![task 1](screenshots/Screenshot%20with%20Output%20of%20docker%20ps%20showing%20the%20running%20myweb%20container.png)

---

#### Screenshot 7 — Browser displaying the Nginx Welcome Page

![task 1](screenshots/Screenshot%20with%20%20Browser%20displaying%20the%20Nginx.png)

---

#### Screenshot 8 — Output of `ls ~/nginx-logs` showing `access.log` and `error.log`

![task 1](screenshots/Screenshot%20with%20Output%20of%20docker%20ps%20showing%20the%20running%20myweb%20container.png)

---

#### Screenshot 9 — Successful removal of the container

![task 1](screenshots/Screenshot%20with%20Successful%20removal%20of%20the%20container.png)

---

#### Screenshot 10 — Output of `ls ~/nginx-logs` confirming the log files remain after the container has been removed

![task 1](screenshots/Screenshot%20with%20Output%20of%20docker%20ps%20showing%20the%20running%20myweb%20container.png)

---

# Task 2 — Deploy Two Containers with Persistent Data Using Docker Volumes

## Goal

Create a custom network `mynetwork` and a Docker volume `shared-data`, build and run a `backend` container that writes to the shared volume and a `frontend` container that reads from it, and confirm updates are reflected immediately.

### Evidence

#### Screenshot 1 — Project folder structure

![task 2](screenshots/Screenshot%20with%20Project%20folder%20structure.png)

---

#### Screenshot 2 — Output of `docker network create mynetwork`

![task 2](screenshots/Screenshot%20with%20Output%20of%20docker%20network%20create%20mynetwork.png)

---

#### Screenshot 3 — Output of `docker volume create shared-data`

![task 2](screenshots/Screenshot%20with%20Output%20of%20docker%20volume%20create%20shared-data.png)

---

#### Screenshot 4 — Backend Dockerfile

![task 2](screenshots/Screenshot%20with%20Backend%20Dockerfile_2.png)

---

#### Screenshot 5 — Successful backend image build

![task 2](screenshots/Screenshot%20with%20Successful%20backend%20image%20build.png)

---

#### Screenshot 6 — Frontend Dockerfile

![task 2](screenshots/Screenshot%20with%20Frontend%20Dockerfile_2.png)

---

#### Screenshot 7 — Successful frontend image build

![task 2](screenshots/Screenshot%20with%20Successful%20frontend%20image%20build_2.png)

---

#### Screenshot 8 — Output of `docker ps` showing both containers

![task 2](screenshots/Screenshot%20with%20Output%20of%20`docker%20ps`%20showing%20both%20containers.png)

---

#### Screenshot 9 — Successful execution of `docker exec backend curl http://localhost/write`

![task 2](screenshots/Screenshot%20with%20Successful%20execution%20of%20docker%20exec%20backend%20curl_http.png)

---

#### Screenshot 10 — Browser displaying "Hello from Backend!"

![task 2](screenshots/Screenshot%20with%20Browser%20displaying%20Hello%20from%20Backend.png)

---

#### Screenshot 11 — Browser displaying "Test Data 1" after the first update

![task 2](screenshots/Screenshot%20with%20Browser%20displaying%20Test%20Data%201%20after%20the%20first%20update.png)

---

#### Screenshot 12 — Browser displaying "Test Data 2 - New Update" after the second update

![task 2](screenshots/Screenshot%20with%20Browser%20displaying%20New%20Update%20after%20the%20second%20update.png)

---

# LinkedIn Post (Optional)

## Goal

Create a LinkedIn post covering the assignment, Docker Hub repository, steps performed, and key learning outcomes.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/es3pF875`

---

#### Screenshot — Published LinkedIn post

![LinkedIn](screenshots/Screenshot%20with%20published%20LinkedIn%20-3.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- Do not expose sensitive information

---

# Completion Checklist

- [✅] Task 1: Bind-mounted persistent logs verified before and after container removal (Screenshots 1–10)
- [✅] Task 2: Docker volume shared between frontend and backend verified (Screenshots 1–12)
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

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
