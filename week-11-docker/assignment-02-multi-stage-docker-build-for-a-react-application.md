# Assignment 2 — Multi-Stage Docker Build for a React Application

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will build both a single-stage and an optimized multi-stage Docker image for a React application, compare the resulting image sizes, and deploy the optimized version using a production-ready Nginx runtime container.

Complete this assignment locally on your own computer where Docker is installed and running.

---

# Task 1 — Prepare the Project

## Goal

Prepare the React application for Docker image creation.

### Evidence

#### Screenshot 1 — Contents of the `.dockerignore` File

Add a screenshot of the terminal showing:

```bash
cat .dockerignore
```

The file must exclude `node_modules`, `build`, and `.env`.

![task 1](screenshots/Screenshot%20with%20Contents%20of%20the%20dockerignore%20file.png)

---

# Task 2 — Create a Single-Stage Docker Image

## Goal

Create a baseline single-stage Docker image and run the application on port 3000.

### Evidence

#### Screenshot 2 — Contents of `Dockerfile.single`

<<<<<<< HEAD
![task 2](screenshots/Screenshot%20with%20Contents%20of%20Dockerfile.single.png)
=======
Add a screenshot showing the completed `Dockerfile.single`.

![task 2](screenshots/Screenshot%20with%20Contents%20of%20Dockerfile.single.png)
>>>>>>> upstream/main

---

#### Screenshot 3 — Single-Stage Application in Browser

Add a screenshot of the browser showing the application at:

```text
http://localhost:3000
```

Ensure that your full name is visible in the application.

![task 2](screenshots/Screenshot%20with%20Browser%20displaying%20the%20application.png)

---

# Task 3 — Create a Multi-Stage Docker Build

## Goal

Create an optimized multi-stage Docker image with separate builder and Nginx runtime stages, then run the application on port 80.

### Evidence

#### Screenshot 4 — Contents of the Multi-Stage Dockerfile

Add a screenshot showing the completed multi-stage `Dockerfile`.

![task 3](screenshots/Screenshot%20with%20Contents%20of%20the%20multi-stage%20Dockerfile.png)

---

#### Screenshot 5 — Multi-Stage Application in Browser

Add a screenshot of the browser showing the application at:

```text
http://localhost
```

Ensure that your full name is visible in the application.

![task 3](screenshots/Screenshot%20with%20Browser%20displaying%20the%20app.png)

---

# Task 4 — Compare Docker Image Sizes

## Goal

Compare the single-stage and multi-stage image sizes and calculate the percentage reduction.

### Evidence

#### Screenshot 6 — Docker Image Size Comparison

Add a screenshot of the terminal showing:

```bash
docker images
```

The output must include both:

```text
react-single:latest
react-multistage:latest
```

![task 4](screenshots/Screenshot%20with%20Docker%20image%20list%20showin%20both%20img.png )

---

### Percentage Reduction Calculation

Record the image sizes and calculate the reduction using the same unit for both images.

```text
Single-stage image size: Add size here

Multi-stage image size: Add size here

Percentage reduction =
((Single-stage image size − Multi-stage image size)
÷ Single-stage image size) × 100

Percentage reduction: Add result here
```

---

# Task 5 — Analyze the Optimization Results

## Goal

<<<<<<< HEAD
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
=======
Evaluate the advantages of using multi-stage Docker builds.

### Notes

Write a short analysis of 5–8 lines covering:

- The single-stage and multi-stage image sizes
- The percentage reduction in image size
- Security benefits of the smaller runtime image
- How a smaller runtime image reduces the attack surface
- How smaller images improve image pull and deployment speed
- One Docker build-caching optimization you used

Write your analysis here.
>>>>>>> upstream/main

---

# Task 6 — Explore Additional Production Optimizations (Optional)

## Goal

Explore one or more additional production optimization techniques.

<<<<<<< HEAD
![task 6](screenshots/Screenshot%20with%20analysis%20submission%20doc.png)
=======
### Optional Work

You may choose to:

- Configure an Nginx health check
- Configure cache headers for static assets
- Experiment with a lighter runtime image
- Compare the resulting image size with your original multi-stage image

Screenshots are optional.
>>>>>>> upstream/main

---

# LinkedIn Requirement

## Goal

Create a LinkedIn post describing what you built, what a multi-stage Docker build is, the image-size reduction achieved, and key learnings from the assignment.

### Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/eHnpaxU7`

---

#### LinkedIn Post Screenshot

<<<<<<< HEAD
![linkedIn](screenshots/Screenshot%20with%20publisheld%20linkedIn.png)
=======
Add a screenshot of the published LinkedIn post here.
>>>>>>> upstream/main

---

# Submission Instructions

- Complete all required tasks in sequence.
- Include Screenshots 1–6 exactly as specified.
- Include the percentage-reduction calculation and Task 5 analysis.
- Include the LinkedIn post URL and screenshot.
- Ensure that your full name is visible in all required screenshots.
- Do not expose passwords, keys, tokens, account IDs, or other sensitive information.

---

# Completion Checklist

<<<<<<< HEAD
- [✅] Task 1: `.dockerignore` created (Screenshot 1)
- [✅] Task 2: Single-stage image built and verified (Screenshots 2–3)
- [✅] Task 3: Multi-stage image built and verified (Screenshots 4–5)
- [✅] Task 4: Image sizes compared (Screenshot 6)
- [✅] Task 5: Analysis written (Screenshot 7 & Notes)
- [✅] Task 6: Optional production optimizations explored
- [✅] No sensitive information exposed
=======
- [ ] Assignment completed locally
- [ ] `.dockerignore` created and verified (Screenshot 1)
- [ ] `Dockerfile.single` created (Screenshot 2)
- [ ] Single-stage container verified in the browser (Screenshot 3)
- [ ] Multi-stage `Dockerfile` created (Screenshot 4)
- [ ] Multi-stage container verified in the browser (Screenshot 5)
- [ ] Both Docker image sizes captured (Screenshot 6)
- [ ] Percentage reduction calculated
- [ ] Optimization analysis completed
- [ ] LinkedIn post URL and screenshot included
- [ ] Full name visible in all required screenshots
- [ ] No sensitive information exposed
>>>>>>> upstream/main

---

## About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory), focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations through hands-on experience.

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
