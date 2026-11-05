# Assignment 5 — Kubernetes Health Probes (Readiness)

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will configure and test a Kubernetes readiness probe. You will create a baseline NGINX Deployment, add an HTTP readiness probe, intentionally break it, observe a stalled rollout, and restore the working configuration.

You will also demonstrate how readiness probes protect rolling updates.

---

# Task 0 — Prepare the Lab Environment

## Goal

Confirm that you are connected to your learning cluster and remove the previous assignment’s HPA and NGINX Deployment before starting this lab.

No screenshot or written notes are required for this pre-check.

---

# Task 1 — Set Up Your Lab Directory

## Goal

Create and enter a dedicated directory for the readiness-probe lab.

### Evidence

#### Screenshot 1 — Terminal showing that you are working inside `~/k8s-labs/health-probes-readiness`

![task 1](screenshots/Screenshot%20wirh%20Terminal%20showing%20that%20you%20are%20working%20inside-k8.png)

---

### Notes

**1. What is the purpose of a readiness probe?**

A readiness probe checks whether a container is ready to accept application traffic. Kubernetes uses the result to determine whether a Pod should be considered Ready.

**2. Why is a Pod being Running not always enough to confirm it can serve traffic?**

Running only indicates that the container has started. The application may still be initializing, loading configuration, or waiting for dependencies. A readiness probe helps confirm that it is actually ready to serve requests.

---

# Task 2 — Create a Baseline Deployment Without Probes

## Goal

Create a baseline NGINX Deployment with two replicas and no readiness probe.

### Evidence

#### Screenshot 2 — Completed `00-nginx-deploy-baseline.yaml` manifest

![task 2](screenshots/Screenshot%20with%20Completed%2000-nginx-deploy-baseline.yaml%20manifest.png)

---

#### Screenshot 3 — Output showing a successful rollout and two running NGINX Pods

![task 2](screenshots/Screenshot%20with%20Output%20showin%20a%20sucessful%20rollout%20n%202%20runnin%20NGINX%20Pod.png)

---

#### Screenshot 4 — Relevant output from `kubectl describe deployment nginx-deployment`

![task 2](screenshots/Screenshot%20with%20Relevant%20output%20from%20kubectl%20describe.png)

---

### Notes

**1. How many replicas does this Deployment maintain?**

Two replicas.

**2. What does `maxUnavailable: 0` mean during a rolling update?**

It means Kubernetes should not reduce the number of available replicas below the desired count during a rolling update. New Pods must become available before old Pods are removed.

**3. Is a readiness probe configured in this baseline Deployment?**

No. The baseline Deployment does not contain a readiness probe.

---

# Task 3 — Add a Readiness Probe

## Goal

Add an HTTP readiness probe so Kubernetes checks whether each NGINX container is ready to accept requests.

### Evidence

#### Screenshot 5 — Completed `01-nginx-deploy-readiness.yaml` manifest showing the readiness probe

![task 3](screenshots/Screenshot%20with%20Completed%2001-nginx-deploy-readiness%20probe.png)

---

#### Screenshot 6 — Output of the successful rollout and ready Pods

![task 3](screenshots/Screenshot%20with%20Output%20of%20the%20successful%20rollout%20and%20ready%20Pods.png)

---

#### Screenshot 7 — Pod description showing readiness conditions and readiness probe details

A![task 3](screenshots/Screenshot%20with%20Pod%20description%20showing%20readiness%20conditions.png)

---

### Notes

**1. What path and port does the readiness probe check?**

It checks the root path / on port 80 using an HTTP GET request.

**2. What does `initialDelaySeconds: 2` do?**

It tells Kubernetes to wait two seconds after the container starts before performing the first readiness check.

**3. What does `periodSeconds: 5` do?**

It instructs Kubernetes to perform the readiness check every five seconds.

**4. What must happen before Kubernetes considers this Pod ready?**

The HTTP readiness probe must successfully receive a response from the configured endpoint.

---

# Task 4 — Break and Restore Readiness

## Goal

Intentionally fail the readiness probe, observe the NotReady behavior, and then restore the working probe configuration.

### Evidence

#### Screenshot 8 — `02-nginx-readiness-broken.yaml` showing `/does-not-exist`

![task 4](screenshots/Screenshot%20wit%2002-nginx-readiness-broken.yaml%20showin_des-nt-exist.png)

---

#### Screenshot 9 — Output showing the rollout timeout or incomplete rollout

![task 4](screenshots/Screenshot%20with%20Output%20showing%20the%20rollout%20timeout%20or%20incomplete%20rollout.png)

---

#### Screenshot 10 — Pod details showing `Ready: False` or readiness-probe failure events

![task 4](screenshots/Screensht%20wit%20Pod%20details%20showin%20Rdy%20False%20or%20readiness-probe%20failure%20events.png)

---

#### Screenshot 11 — Output showing successful rollout and ready Pods after the fix

![task 4](screenshots/Screenshot%20wit%20Output%20showin%20successful%20rollout%20n%20ready%20Pods%20afta%20d%20fix.png)

---

### Notes

**1. Why did the readiness probe fail?**

The readiness probe failed because it was configured to access /does-not-exist, which returned HTTP 404 instead of a successful response.

**2. What happened to the Pod readiness status?**

The affected Pod remained Running but its readiness status changed to False, showing 0/1 Ready.

**3. Why did the rollout fail to complete?**

The new Pods could not pass their readiness checks. Kubernetes therefore could not consider them available to replace the existing healthy Pods.

**4. How did you restore the Deployment successfully?**

I restored the correct readiness probe path from /does-not-exist to / and reapplied the working Deployment configuration. The Pods passed their readiness checks and the rollout completed successfully.

---

# Task 5 — See How Readiness Protects Rolling Updates

## Goal

Observe how readiness prevents a bad new Pod from replacing healthy Pods during a rolling update.

### Evidence

#### Screenshot 12 — Successful update command and rollout status

![task 5](screenshots/Screenshot%20with%20Successful%20update%20command%20and%20rollout%20status.png)

---

#### Screenshot 13 — ReplicaSets and Pods after the successful rolling update

![task 5](screenshots/Screenshot%20with%20ReplicaSets%20and%20Pods%20after%20the%20successful%20rolling%20update.png)

---

#### Screenshot 14 — Completed `04-readiness-broken-patch.yaml`

![task 5](screenshots/Screenshot%20with%20Completed%2004-readiness-broken-patch.yaml.png)

---

#### Screenshot 15 — Output showing the expected stalled rollout or timeout

![task 5](screenshots/Screenshot%20with%20Output%20showing%20the%20expected%20stalled%20rollout%20or%20timeout.png)

---

#### Screenshot 16 — Deployment or Pod output showing the effect of the broken readiness probe

![task 5](screenshots/Screenshot%20wit%20Deploymnt%20or%20Pod%20output%20showin%20the%20effect%20of%20d%20broken%20readiness.png)

---

#### Screenshot 17 — Successful rollout status after restoring the fixed configuration

![task 5](screenshots/Screenshot%20with%20Successful%20rollout%20status%20after%20restoring%20the%20fixed%20configuration.png)

---

### Notes

**1. What happened during the successful rolling update?**

The Deployment image was updated from nginx:1.27-alpine to nginx:1.28-alpine. Kubernetes created a new ReplicaSet and gradually replaced the old Pods. The readiness probe confirmed that the new Pods were ready before the rollout completed.

**2. Why did Kubernetes create a new ReplicaSet?**

The image was changed to nginx:1.29-alpine, but the readiness probe used the invalid path /does-not-exist. The new Pod started but remained NotReady because the HTTP request returned status code 404. The rollout timed out after 60 seconds.

**3. Why were the new Pods unable to replace the existing healthy Pods after readiness was broken?**

The Deployment used maxUnavailable: 0, which prevented Kubernetes from reducing the number of available replicas below two during the rolling update. The existing healthy Pods were preserved while the replacement Pod remained NotReady.

**4. How does `maxUnavailable: 0` protect availability during this rollout?**

Kubernetes used the readiness probe to identify that the new Pod was not ready to serve traffic. It kept the existing healthy Pods available rather than completing an unsafe rollout.

---

# Task 6 — Share Your Readiness-Probe Learning on LinkedIn

## Goal

Share what you learned about Kubernetes readiness probes and how they protect rolling updates.

### Evidence

#### Screenshot 18 — Published LinkedIn post

![link](screenshots/Screenshot%20with%20Published%20LinkedIn%20post%20Kubernates%202.png)

#### Linkedin post link

`https://lnkd.in/p/ekMBb8Sb`

---

# Submission Instructions

- Add all required screenshots numbered 1–18.
- Ensure your full name is visible in required screenshots.
- Use clear, readable screenshots showing the relevant manifest or output.
- Include the completed baseline and readiness-probe manifests in your repository.
- Answer all Notes questions for Tasks 1–5 in your own words.
- Include evidence of the intentional readiness failure and successful recovery.
- Do not expose tokens, passwords, kubeconfig files, account IDs, or other sensitive information.

---

# Completion Checklist

- [✅] Learning cluster confirmed and previous assignment resources removed where applicable
- [✅] Readiness-probe lab directory created (Screenshot 1)
- [✅] Baseline Deployment created successfully (Screenshot 2)
- [✅] Two baseline Pods running and successful rollout verified (Screenshot 3)
- [✅] Baseline Deployment details reviewed (Screenshot 4)
- [✅] Readiness probe added successfully (Screenshot 5)
- [✅] Successful readiness rollout verified (Screenshot 6)
- [✅] Pod readiness conditions and probe details inspected (Screenshot 7)
- [✅] Broken readiness configuration created (Screenshot 8)
- [✅] Stalled rollout or timeout observed (Screenshot 9)
- [✅] NotReady status or readiness-probe failure events captured (Screenshot 10)
- [✅] Working readiness configuration restored (Screenshot 11)
- [✅] Successful image update completed (Screenshot 12)
- [✅] ReplicaSets and Pods inspected after the update (Screenshot 13)
- [✅] Broken-readiness patch created (Screenshot 14)
- [✅] Stalled rollout observed after the patch (Screenshot 15)
- [✅] Effect of broken readiness captured (Screenshot 16)
- [✅] Final recovery completed (Screenshot 17)
- [✅] LinkedIn post published (Screenshot 18)
- [✅] Notes for Tasks 1–5 completed
- [✅] Required manifests included
- [✅] All screenshots are clear and readable
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
