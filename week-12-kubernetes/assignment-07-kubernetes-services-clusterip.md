# Assignment 7 — Kubernetes Services (ClusterIP)

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will create and test a Kubernetes ClusterIP Service. You will connect it to a healthy NGINX Deployment, access it using DNS and its ClusterIP, break and restore its selector, and observe how readiness affects Service routing.

---

# Task 0 — Prepare the Lab Environment

## Goal

Confirm your learning-cluster context and remove previous lab resources where applicable before starting this assignment.

No screenshot or written notes are required for this pre-check.

---

# Task 1 — Set Up Your Lab Directory

## Goal

Create and enter a dedicated directory for the ClusterIP Service lab.

### Evidence

#### Screenshot 1 — Terminal showing that you are working inside `~/k8s-labs/services/clusterip`

![task 1](screenshots/Screenshot%20with%20Terminal%20showin%20that%20you%20are%20workin_%20inside_k8s.png)

---

### Notes

**1. Why is it not reliable to communicate with an application using a Pod IP address?**

Pod IP addresses are temporary and can change when Pods are deleted, recreated, or rescheduled. Therefore, applications should not depend directly on individual Pod IP addresses.

**2. What Kubernetes resource provides a stable internal address for a group of Pods?**

A Kubernetes Service, specifically a ClusterIP Service, provides a stable internal IP address and DNS name for accessing a group of Pods.

---

# Task 2 — Create a Healthy NGINX Deployment

## Goal

Create an NGINX Deployment with labels, readiness probes, and liveness probes that can act as the backend for a Service.

### Evidence

#### Screenshot 2 — Completed `00-nginx-deploy.yaml` manifest

![task 2](screenshots/Screenshot%20with%20Completed%2000-nginx-deploy.yaml%20manifest_G.png)

---

#### Screenshot 3 — Output showing successful rollout and two ready NGINX Pods

![task 2](screenshots/Screenshot%20with%20Output%20showing%20successful%20rollout%20n%202.png)

---

### Notes

**1. Which label will the Service use to find these Pods?**

The Service uses the label app: nginx to identify and select the NGINX Pods. This matches the labels defined in the Deployment's Pod template.

**2. Why is readiness important before connecting Pods to a Service?**

Readiness ensures that a Pod is healthy and ready to receive traffic before the Service sends requests to it. If a Pod fails its readiness probe, Kubernetes removes it from the Service's available endpoints until it becomes ready again. This helps prevent users from receiving errors from unhealthy or unready Pods.

**3. What port does the NGINX container listen on?**

The NGINX container listens on port 80 (TCP). The Service also exposes port 80 and forwards traffic to the container's target port 80.

---

# Task 3 — Create a ClusterIP Service

## Goal

Create a stable internal Service that routes traffic to the NGINX Pods.

### Evidence

#### Screenshot 4 — Completed `01-nginx-svc-clusterip.yaml` manifest

![task 3](screenshots/Screenshot%20with%20Completed%2001-nginx-svc-clusterip_yaml%20manifest.png )

---

#### Screenshot 5 — Output showing the ClusterIP Service, endpoints, or EndpointSlices

![task 3](screenshots/Screenshot%20with%20Output%20showing%20the%20ClusterIP%20Service%20endpoints_.png)

---

### Notes

**1. What is the purpose of a ClusterIP Service?**

A ClusterIP Service provides a stable internal IP address and DNS name for accessing a group of Pods within a Kubernetes cluster. It allows applications running inside the cluster to communicate with other services without exposing them directly to the internet.

**2. What is the difference between `port` and `targetPort`?**

- port is the port exposed by the Kubernetes Service that other applications use to access it.
- targetPort is the port on the destination Pod or container where the Service forwards the traffic.
In this assignment, both are set to port 80.

**3. Why must the Service selector match the Pod labels?**

The Service selector must match the Pod labels so Kubernetes can identify which Pods should receive traffic. If the labels do not match, the Service will have no endpoints and requests will fail because it cannot find any destination Pods.

**4. What information do EndpointSlices contain?**

EndpointSlices contain information about the network endpoints associated with a Service, including:
- Pod IP addresses.
- Port numbers used to reach the Pods.
- Address type, such as IPv4 or IPv6.
- Endpoint readiness and serving conditions.
- Whether an endpoint is terminating.
Kubernetes uses this information to determine which Pods are available to receive Service traffic.

---

# Task 4 — Access the Service from Inside the Cluster

## Goal

Create a client Pod and access the NGINX Service using DNS and its ClusterIP.

### Evidence

#### Screenshot 6 — Output showing the `tester` Pod running

![task 4](screenshots/Screenshot%20with%20Output%20showing%20the%20tester%20Pod%20running.png)

---

#### Screenshot 7 — Successful Service request using DNS name and ClusterIP

![task 4](screenshots/Screenshot%20with%20Successful%20Service%20request%20using%20DNS%20name%20and%20ClusterIP.png)

---

#### Screenshot 8 — Successful DNS lookup for the Service

![task 4](screenshots/Screenshot%20with%20Successful%20DNS%20lookup%20for%20the%20Service.png)

---

### Notes

**1. Why should an application use `nginx-svc` instead of a Pod IP address?**

An application should use nginx-svc because a Kubernetes Service provides a stable name and IP address for accessing Pods. Pod IP addresses can change when Pods are deleted, recreated, or restarted. Using the Service name makes communication more reliable and easier to manage.

**2. What does the DNS name `nginx-svc.default.svc.cluster.local` represent?**

It is the fully qualified domain name (FQDN) of the Kubernetes Service named nginx-svc in the default namespace. It allows applications inside the cluster to locate and communicate with the Service through Kubernetes DNS.

**3. Why can the client access the Service even if the backend Pod IPs change?**

The client accesses the Service through its stable DNS name or ClusterIP rather than connecting directly to individual Pod IPs. Kubernetes automatically updates the Service's EndpointSlices when Pods are replaced or their IP addresses change, allowing traffic to be routed to the current healthy backend Pods.

---

# Task 5 — Break and Restore the Label-Selector Contract

## Goal

Break the Service selector, observe that usable endpoints disappear, and then restore the correct selector.

### Evidence

#### Screenshot 9 — `02-nginx-svc-selector-broken.yaml` showing the incorrect selector

![task 5](screenshots/Screenshot%20wit%2002-nginx-svc-selector-brkn_yaml%20showin%20d%20incorrect.png)

---

#### Screenshot 10 — Empty endpoints or EndpointSlice output after applying the broken selector

![task 5](screenshots/Screenshot%20with%20Empty%20endpoints%20or%20EndpointSlice%20output.png)

---

#### Screenshot 11 — Failed request output from the `tester` Pod

![task 5](screenshots/Screenshot%20with%20Failed%20request%20output%20from%20the%20tester%20Pod.png)

---

#### Screenshot 12 — Restored endpoints and successful Service request

![task 5](screenshots/Screenshot%20with%20Restored%20endpoints%20and%20successful%20Service%20request.png)

---

### Notes

**1. Why did the Service endpoints become empty?**

The Service endpoints became empty because I changed the Service selector from app: nginx to app: does-not-match. This selector no longer matched the labels on the NGINX Pods, so Kubernetes could not find any Pods to associate with the Service.

**2. What caused the request from the client Pod to fail?**

The request failed because the Service had no available endpoints to forward traffic to. Since no Pods matched the incorrect selector, the client Pod could not reach the NGINX application through nginx-svc.

**3. How did you restore connectivity to the NGINX Pods?**

I restored connectivity by copying the backup Service manifest back to 01-nginx-service.yaml, ensuring the selector was set to app: nginx, and applying the corrected configuration using: kubectl apply -f 01-nginx-service.yaml

Kubernetes then identified the matching NGINX Pods, restored their IP addresses as Service endpoints, and the client Pod successfully accessed the NGINX welcome page again.

---

# Task 6 — Observe How Readiness Affects Service Routing

## Goal

Observe how a new Pod that fails readiness is excluded from ready Service backends while the existing healthy Pod continues serving traffic.

### Evidence

#### Screenshot 13 — Output showing the Deployment scaled to one ready Pod

![task 6](screenshots/Screenshot%20Output%20showing%20the%20Deployment%20scaled%20to%20one%20ready%20Pod.png)

---

#### Screenshot 14 — Completed `03-readiness-broken.yaml` patch file

![task 6](screenshots/Screenshot%20with%20%20Completed%2003-readiness-broken_yaml%20patch%20file.png)

---

#### Screenshot 15 — Output showing the expected incomplete rollout or NotReady Pod

![task 6](screenshots/Screenshot%20with%20Output%20showing%20the%20expected%20incomplete%20rollout.png)

---

#### Screenshot 16 — EndpointSlice output showing the healthy Pod with `ready: true` and the new NotReady Pod with `ready: false`

![task 6](screenshots/Screenshot%20with%20EndpointSlice%20output%20showing%20the%20healthy%20Pod%20_1.png)

---

#### Screenshot 17 — Service EndpointSlice output showing ready backends after restoring the Deployment

![task 6](screenshots/Screenshot%20with%20Service%20EndpointSlice%20output%20showing%20ready%20backends.png)

---

### Notes

**1. Why was the new NotReady Pod excluded from ready Service backends?**

The new Pod was excluded because its readiness probe was configured with an incorrect HTTP path (/does-not-exist). The probe failed, so Kubernetes marked the Pod as NotReady (0/1 Running). As a result, its IP address was excluded from the ready Service endpoints.

**2. What is the relationship between readiness probes and Service routing?**

Readiness probes determine whether a Pod is ready to receive application traffic. Kubernetes uses the readiness status to manage the Service's endpoints. When a Pod passes its readiness probe, it becomes eligible to receive Service traffic. When it fails, it is removed from the ready endpoints until it becomes ready again.

**3. Why does this behavior protect users from unhealthy application instances?**

This behavior prevents Kubernetes from routing user requests to Pods that are not ready to serve them. It helps reduce failed requests, errors, and service interruptions by directing traffic only to healthy, ready application instances.

---

# Task 7 — Share Your ClusterIP Service Learning on LinkedIn

## Goal

Share what you learned about ClusterIP Services, Kubernetes DNS, selectors, and readiness-aware Service routing.

### Evidence

#### Screenshot 18 — Published LinkedIn post

![linkedIn](screenshots/Screenshot%20with%20Learning%20on%20LinkedIn%20__1.png)

#### Linkedin post link

`https://lnkd.in/p/e7d8kxsX`

---

# Submission Instructions

- Add all required screenshots numbered 1–18.
- Ensure your full name is visible in required screenshots.
- Use clear, readable screenshots showing the relevant manifest or output.
- Include the completed NGINX Deployment and ClusterIP Service manifests in your repository.
- Answer all Notes questions for Tasks 1–6 in your own words.
- Include evidence of the intentional failures and successful recovery.
- Do not expose tokens, passwords, kubeconfig files, account IDs, or other sensitive information.

---

# Completion Checklist

- [✅] Learning-cluster context confirmed and previous lab resources removed where applicable
- [✅] ClusterIP lab directory created (Screenshot 1)
- [✅] Healthy NGINX Deployment manifest completed (Screenshot 2)
- [✅] Successful rollout and two ready NGINX Pods verified (Screenshot 3)
- [✅] ClusterIP Service manifest completed (Screenshot 4)
- [✅] Service and backend endpoints or EndpointSlices verified (Screenshot 5)
- [✅] Client Pod created and running (Screenshot 6)
- [✅] Service accessed using DNS name and ClusterIP (Screenshot 7)
- [✅] Service DNS resolution verified (Screenshot 8)
- [✅] Broken selector manifest created (Screenshot 9)
- [✅] Empty endpoints observed (Screenshot 10)
- [✅] Failed Service request captured (Screenshot 11)
- [✅] Original selector restored and connectivity verified (Screenshot 12)
- [✅] Deployment scaled to one ready Pod (Screenshot 13)
- [✅] Broken readiness patch created (Screenshot 14)
- [✅] Incomplete rollout or NotReady Pod observed (Screenshot 15)
- [✅] Readiness effect on Service backends verified (Screenshot 16)
- [✅] Deployment restored and ready Service backends verified (Screenshot 17)
- [✅] LinkedIn post published (Screenshot 18)
- [✅] Notes for Tasks 1–6 completed
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
