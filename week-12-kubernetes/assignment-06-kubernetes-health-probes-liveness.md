# Assignment 6 — Kubernetes Health Probes (Liveness)

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will configure and test a Kubernetes liveness probe. You will create a baseline NGINX Deployment, add an HTTP liveness probe, intentionally make it fail, observe container restarts, and restore the working configuration.

---

# Task 0 — Prepare the Lab Environment

## Goal

Confirm that you are connected to your learning cluster and remove the previous HPA and NGINX Deployment before starting this lab.

No screenshot or written notes are required for this pre-check.

---

# Task 1 — Set Up Your Lab Directory

## Goal

Create and enter a dedicated directory for the liveness-probe lab.

### Evidence

#### Screenshot 1 — Terminal showing that you are working inside `~/k8s-labs/health-probes-liveness`

![task 1](screenshots/Screenshot%20with%20Terminal%20showing%20that%20you%20are%20working%20inside%20k8s.png)

---

### Notes

**1. What problem does a liveness probe solve?**

A liveness probe detects when a container is running but has become unhealthy or unresponsive. Kubernetes uses repeated probe failures to determine when the container should be restarted.

**2. What is the key difference between a readiness probe and a liveness probe?**

A readiness probe determines whether a Pod is ready to receive traffic, while a liveness probe determines whether a container needs to be restarted.

---

# Task 2 — Create a Baseline Deployment Without Probes

## Goal

Create a baseline NGINX Deployment with two replicas and no health probes.

### Evidence

#### Screenshot 2 — Completed `00-nginx-deploy-baseline.yaml` manifest

![task 2](screenshots/Screenshot%20with%20Completed%2000-nginx-deploy-baseline.yaml%20manifest.png)

---

#### Screenshot 3 — Output showing a successful rollout and two running NGINX Pods

![task 2](screenshots/Screenshot%20with%20Output%20showin%20a%20successful%20rollout%20and%202%20runnin%20NGINX%20Pods.png)

---

#### Screenshot 4 — Relevant output from `kubectl describe deployment nginx-deployment`

![task 2](screenshots/Screenshot%20with%20Relevant%20output%20from%20kubectl%20describe__2.png)

---

### Notes

**1. How many replicas does this Deployment maintain?**

The Deployment maintains two NGINX replicas.

**2. Are any health probes configured in the baseline Deployment?**

No. Neither a liveness probe nor a readiness probe is configured.

**3. Why is a baseline useful before adding a liveness probe?**

A baseline provides a working reference without health probes. It allows us to compare normal container behaviour with behaviour after introducing liveness checks.

---

# Task 3 — Add a Liveness Probe

## Goal

Add an HTTP liveness probe so Kubernetes can periodically check whether the NGINX container is still responsive.

### Evidence

#### Screenshot 5 — Completed `01-nginx-deploy-liveness.yaml` manifest showing the liveness probe

![task 3](screenshots/Screenshot%20with%20completed%20yaml%20manifest%20showing%20the%20liveness%20probe_2.png)

---

#### Screenshot 6 — Output showing successful rollout and running Pods

![task 6](screenshots/Screenshot%20with%20Output%20showing%20successful%20rollout%20and%20running%20Pods.png)

---

#### Screenshot 7 — Pod description showing liveness-probe details

![task 7](screenshots/Screenshot%20with%20Pod%20description%20showing%20liveness-probe%20details.png)

---

### Notes

**1. What path and port does the liveness probe check?**

It checks the root path / on port 80 using an HTTP GET request.

**2. What does `initialDelaySeconds: 10` do?**

It delays the first liveness check for 10 seconds after the container starts.

**3. What does `failureThreshold: 3` mean?**

Kubernetes restarts the container after three consecutive unsuccessful liveness checks.

**4. Does a successful liveness probe control whether a Pod receives traffic? Explain briefly.**

No. A liveness probe determines whether a container should continue running or be restarted. A readiness probe determines whether a Pod is ready to receive traffic.

---

# Task 4 — Trigger and Observe a Liveness Restart

## Goal

Intentionally make the liveness probe fail and observe Kubernetes restart the unhealthy container.

### Evidence

#### Screenshot 8 — `02-nginx-liveness-broken.yaml` showing `/does-not-exist`

![task 4](screenshots/Screenshot%20with%20doesnt%20exist%20parole.png)

---

#### Screenshot 9 — Pod output showing an increased RESTARTS count

![task 4](screenshots/Screenshot%20with%20Pod%20output%20showing%20an%20increased%20RESTARTS%20count.png)

---

#### Screenshot 10 — Pod description showing liveness-probe failure events or restart details

![task 4](screenshots/Screenshot%20wit%20Pod%20description%20showin%20liveness-probe%20failure__1.png)

---

#### Screenshot 11 — Output showing stable running Pods after applying the fixed configuration

![task 4](screenshots/Screenshot%20wit%20Output%20showin%20stable%20runnin%20Pods%20afta%20applyin%20d%20fixed%20configuration.png)

---

### Notes

**1. Why did the liveness probe fail?**

The liveness probe failed because it was configured to access /does-not-exist, an invalid NGINX URL. NGINX returned HTTP 404 instead of a successful HTTP response.

**2. What happened after Kubernetes detected repeated liveness-probe failures?**

Kubernetes detected three consecutive failed liveness checks and automatically restarted the affected NGINX containers.

**3. What does the RESTARTS column prove?**

The RESTARTS column shows how many times a container has restarted inside a Pod. The increase from zero to one confirmed that Kubernetes restarted the unhealthy containers.

**4. How did you restore the Deployment to a stable state?**

I restored the original working liveness configuration using 03-nginx-liveness-fixed.yaml, changed the probe endpoint back to /, reapplied the manifest, and verified that both NGINX Pods were running successfully.

---

# Task 5 — Optional: Observe a Restart Loop

## Goal

Observe why overly aggressive or incorrectly configured liveness probes can create restart loops.

Complete this section only if you attempt Task 5. Otherwise, mark it as “Not attempted.”

**Task status:** Completed / Not attempted

### Evidence

#### Optional Screenshot — Pod output showing the RESTARTS count increasing during the restart-loop observation

![task 5](screenshots/Screenshot%20wit%20Output%20showin%20stable%20runnin%20Pods%20afta%20applyin%20d%20fixed%20configuration.png)

No specific restart count or CrashLoopBackOff status is required.

---

### Notes — Answer Only If Attempted

**1. What is a restart loop?**

A restart loop occurs when a container repeatedly starts, fails its health checks or exits, and is restarted by Kubernetes without becoming healthy.

**2. Why can an aggressive liveness probe be harmful?**

An overly aggressive liveness probe can restart healthy applications during temporary delays or slow startup periods, causing unnecessary downtime and repeated restarts.

**3. Why should liveness checks be lightweight and carefully tuned?**

Liveness checks should be lightweight to avoid consuming unnecessary resources and should have appropriate timing thresholds to prevent false failures and unnecessary container restarts.

---

# Task 6 — Share Your Liveness-Probe Learning on LinkedIn

## Goal

Share what you learned about Kubernetes liveness probes, container restarts, and the difference between readiness and liveness.

### Evidence

#### Screenshot 12 — Published LinkedIn post

![linkedIn](screenshots/Screenshot%20with%20Published%20LinkedIn%20post%20Kubernates%203.png )

#### Linkedin post URL

`https://lnkd.in/p/eqJbhyj4`

---

# Submission Instructions

- Add all required screenshots numbered 1–12.
- Include the optional Task 5 screenshot only if you attempt that task.
- Ensure your full name is visible in required screenshots.
- Use clear, readable screenshots showing the relevant manifest or output.
- Include the completed baseline and liveness-probe manifests in your repository.
- Answer all Notes questions for Tasks 1–4 in your own words.
- If you attempt Task 5, complete its Notes and restore the working configuration afterward.
- Do not expose tokens, passwords, kubeconfig files, account IDs, or other sensitive information.

---

# Completion Checklist

- [✅] Learning cluster confirmed and previous assignment resources removed where applicable
- [✅] Liveness-probe lab directory created (Screenshot 1)
- [✅] Baseline Deployment manifest completed (Screenshot 2)
- [✅] Successful rollout and two baseline Pods verified (Screenshot 3)
- [✅] Baseline Deployment details reviewed (Screenshot 4)
- [✅] Liveness probe added successfully (Screenshot 5)
- [✅] Successful liveness-probe rollout verified (Screenshot 6)
- [✅] Liveness-probe details inspected (Screenshot 7)
- [✅] Broken liveness configuration created (Screenshot 8)
- [✅] Increased container restart count observed (Screenshot 9)
- [✅] Liveness-probe failure events or restart details captured (Screenshot 10)
- [✅] Working liveness configuration restored and stable Pods verified (Screenshot 11)
- [✅] Optional Task 5 evidence and Notes completed, if attempted
- [✅] Working configuration restored after Task 5, if attempted
- [✅] LinkedIn post published (Screenshot 12)
- [✅] Notes for Tasks 1–4 completed
- [✅] Required manifests included
- [✅] All required screenshots are clear and readable
- [✅] No sensitive data exposed

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
