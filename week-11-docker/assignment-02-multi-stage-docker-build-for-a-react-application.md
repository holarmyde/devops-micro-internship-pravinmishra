# Assignment 2 — Multi-Stage Docker Build for a React Application

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build both a single-stage and an optimized multi-stage Docker image for a React application, compare the resulting image sizes, and deploy the optimized version using a production-ready Nginx runtime container.

---

# Task 1 — Prepare the Project

## Goal

Clone `https://github.com/pravinmishraaws/my-react-app.git` and create a `.dockerignore` excluding `node_modules`, `build`, and `.env`.

### Evidence

#### Screenshot 1 — Contents of the `.dockerignore` file

![task 1](screenshots/Screenshot%20with%20Contents%20of%20the%20dockerignore%20file.png)

---

# Task 2 — Create a Single-Stage Docker Image

## Goal

Create `Dockerfile.single`, build `react-single`, and run it on port 3000.

### Evidence

#### Screenshot 2 — Contents of `Dockerfile.single`

![task 2](screenshots/Screenshot%20with%20Contents%20of%20Dockerfile.single.png)

---

#### Screenshot 3 — Browser displaying the application running from the single-stage container

![task 2](screenshots/Screenshot%20with%20Browser%20displaying%20the%20application.png)

---

# Task 3 — Create a Multi-Stage Docker Build

## Goal

Create a multi-stage Dockerfile with separate build and Nginx runtime stages, build `react-multistage`, and run it on port 80.

### Evidence

#### Screenshot 4 — Contents of the multi-stage Dockerfile

![task 3](screenshots/Screenshot%20with%20Contents%20of%20the%20multi-stage%20Dockerfile.png)

---

#### Screenshot 5 — Browser displaying the application running from the multi-stage container

![task 3](screenshots/Screenshot%20with%20Browser%20displaying%20the%20app.png)

---

# Task 4 — Compare Docker Image Sizes

## Goal

Compare the single-stage and multi-stage image sizes and calculate the percentage reduction.

### Evidence

#### Screenshot 6 — Docker image list showing both image sizes

![task 4](screenshots/Screenshot%20with%20Docker%20image%20list%20showin%20both%20img.png )

---

# Task 5 — Analyze the Optimization Results

## Goal

Write a 5–8 line analysis covering the percentage reduction, security benefits, reduced attack surface, faster distribution, and one build-caching optimization used.

### Evidence

#### Screenshot 7 — Analysis included in your submission document

![task 5](screenshots/Screenshot%20with%20analysis%20submission%20doc.png)

---

### Notes

### Analysis: Single-Stage vs. Multi-Stage Docker Build

The single-stage Docker image (`react-single`) had a disk usage of 818 MB, while the multi-stage image (`react-multistage`) used only 95.1 MB. This represents an approximate **88.37% reduction** in image size, saving 722.9 MB. The multi-stage approach achieves this by using Node.js only in the build stage and Nginx Alpine in the final runtime stage. Removing the Node.js environment and development dependencies reduces the final image's attack surface and improves security. The smaller image also requires less storage and can be transferred and deployed faster. Separating build and runtime stages makes the deployment more efficient, while Docker layer caching can help speed up subsequent builds when dependencies remain unchanged.

### Optional Optimization: Docker Health Check

I enhanced my multi-stage Docker deployment by adding a Docker HEALTHCHECK instruction to the Nginx runtime stage. The health check uses `wget` to send an HTTP request to the local Nginx server every 30 seconds. Docker allows a 5-second startup period, a 3-second timeout, and marks the container unhealthy after three consecutive failures. After rebuilding the image and recreating the container, I verified that its status was **healthy**, with a failing streak of zero and an exit code of zero. This optimization improves container monitoring by allowing Docker to distinguish between a running container and an application that is actually responding to requests.

---

# Task 6 — Explore Additional Production Optimizations (Optional)

## Goal

Optionally configure an Nginx health check, cache headers, parameterized ports via environment variables, or a lighter runtime image, and compare results.

![task 6](screenshots/Screenshot%20with%20analysis%20submission%20doc.png)

---

# LinkedIn Post (Optional)

## Goal

Create a LinkedIn post describing what you built, what a multi-stage Docker build is, the image size reduction achieved, and key learnings.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/eHnpaxU7`

---

#### Screenshot — Published LinkedIn post

![linkedIn](screenshots/Screenshot%20with%20publisheld%20linkedIn.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- Do not expose sensitive information

---

# Completion Checklist

- [✅] Task 1: `.dockerignore` created (Screenshot 1)
- [✅] Task 2: Single-stage image built and verified (Screenshots 2–3)
- [✅] Task 3: Multi-stage image built and verified (Screenshots 4–5)
- [✅] Task 4: Image sizes compared (Screenshot 6)
- [✅] Task 5: Analysis written (Screenshot 7 & Notes)
- [✅] Task 6: Optional production optimizations explored
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
