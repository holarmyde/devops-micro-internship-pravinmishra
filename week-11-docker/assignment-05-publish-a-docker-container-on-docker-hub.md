# Assignment 5 — Publish a Docker Container on Docker Hub

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will publish a Dockerized React application to Docker Hub, remove the local image tags, pull the image again from Docker Hub, and run it to verify that it can be downloaded and deployed from a container registry.

---

# Task 1 — Publish a Docker Image to Docker Hub

## Goal

Tag a locally built React image, publish it to Docker Hub, remove the local copy, pull it again from Docker Hub, and run it successfully.

### Evidence

#### Screenshot 1 — Public Docker Hub Repository

Add a screenshot of Docker Hub showing your newly created public repository:

```text
my-react-app
```

![task 1](screenshots/Screenshot%20with%20Docker%20Hub%20repository%20my-react-app.png)

---

#### Screenshot 2 — Successful Docker Login

Add a screenshot of the terminal showing:

```text
Login Succeeded
```

Ensure that your full name is visible and that no password, Personal Access Token, or device code is exposed.

![task 1](screenshots/Screenshot%20with%20Successful%20docker%20login.png)

---

#### Screenshot 3 — Correctly Tagged Image

Add a screenshot of the terminal showing:

```bash
docker image ls <YOUR_DOCKERHUB_USERNAME>/my-react-app
```

The output must show the `latest` tag.

![task 1](screenshots/Screenshot%20with%20Successful%20docker%20tag_1.png)

---

#### Screenshot 4 — Successful Docker Push

Add a screenshot of the terminal showing successful completion of:

```bash
docker push <YOUR_DOCKERHUB_USERNAME>/my-react-app:latest
```

The output must include a pushed status or image digest.

![task 1](screenshots/Screenshot%20with%20Successful%20docker%20push.png)

---

#### Screenshot 5 — Published `latest` Tag in Docker Hub

Add a screenshot of your Docker Hub repository showing the uploaded `latest` image tag.

![task 1](screenshots/Screenshot%20with%20Docker%20Hub%20repository%20showing%20the%20uploaded%20image.png)

---

#### Screenshot 6 — Local Image Removed and Pulled Again

Add a screenshot of the terminal showing:

- The targeted local image tags removed
- Successful `docker pull` output
- `docker image ls` showing the pulled image

![task 1](screenshots/Screenshot%20with%20Successful%20docker%20pull%20_%202.png)

---

#### Screenshot 7 — Running Pulled Image

Add a screenshot of the terminal showing:

```bash
docker ps
```

The output must show the running `react-container` with:

```text
0.0.0.0:80->80/tcp
```

![task 1](screenshots/Screenshot%20with%20Output%20of%20docker%20ps_%202.png)

---

#### Screenshot 8 — React Application in Browser

Add a browser screenshot showing the React application at:

```text
http://<YOUR-VM-PUBLIC-IP>
```

Ensure that the VM public IP is visible in the address bar. Add your full name as a clear caption directly below the screenshot.

![task 1](screenshots/Screenshot%20with%20Browser%20displaying%20the%20application.png)

---

# Docker Hub Repository URL

**Repository URL:** `https://hub.docker.com/repository/docker/aremufemmy/my-react-app/tags`

---

# Registry and Image Tagging Notes

Write a short explanation covering:

- Why image tagging is required before pushing to Docker Hub
- Why a container registry is useful in DevOps workflows
- Why production deployments should use versioned image tags instead of relying only on `latest`

1. Why image tagging is required before pushing to Docker Hub: Image tagging assigns an identifiable name and version to a Docker image, making it easier to organize, reference, and push images to the correct Docker Hub repository.

2. Why a container registry is useful in DevOps workflows: A container registry provides a central location to store, manage, and distribute Docker images. It allows development and operations teams to share images and deploy consistent application versions across different environments.

3. Why production deployments should use versioned image tags instead of relying only on latest: Versioned tags, such as v1.0, v1.1, or v2.0, make deployments predictable and traceable. They allow teams to identify exactly which image is running, roll back to a previous version when necessary, and avoid accidentally deploying an unexpected update.

---

# LinkedIn Requirement

## Goal

Create a LinkedIn post about publishing a Docker container image to Docker Hub.

Include:

- Assignment title: **Publish a Docker Container Image to Docker Hub**
- Your Docker Hub repository URL
- What you published
- How you verified the remote image by pulling and running it
- Key learning outcomes

### Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/evNeYdem`

---

#### LinkedIn Post Screenshot

![LinkedIn](screenshots/Screenshot%20with%20published%20LinkedIn%20-4.png)

---

# Submission Instructions

- Complete all steps in sequence.
- Include Screenshots 1–8 exactly as specified.
- Include your Docker Hub repository URL.
- Include the Registry and Image Tagging Notes.
- Include the LinkedIn post URL and screenshot.
- Ensure that your full name is visible in all terminal screenshots.
- Add your full name as a clear caption below the browser screenshot.
- Do not expose passwords, Personal Access Tokens, device codes, credentials, or other sensitive information.

---

# Completion Checklist

- [✅ ] Public `my-react-app` repository created
- [✅ ] Docker login completed successfully
- [✅ ] `react-multistage:latest` tagged correctly
- [✅ ] Image pushed to Docker Hub
- [✅ ] `latest` tag verified in Docker Hub
- [✅ ] Targeted local image tags removed
- [✅ ] Image pulled again from Docker Hub
- [✅ ] Pulled image runs successfully
- [✅ ] React application is accessible through the VM public IP
- [✅ ] Docker Hub repository URL included
- [✅ ] Registry and image-tagging notes completed
- [✅ ] LinkedIn post URL and screenshot included
- [✅ ] All required screenshots included
- [✅ ] Full name visible in terminal screenshots
- [✅ ] Browser screenshot has a full-name caption
- [✅ ] No passwords, tokens, or credentials exposed
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
