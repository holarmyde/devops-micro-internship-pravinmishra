# Assignment 8 — Kubernetes Services (NodePort)

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

Create a healthy NGINX Deployment, expose it through a NodePort Service, test access using a node IP and NodePort, and observe how the Service behaves when a backend Pod is replaced.

---

# Task 1 — Set Up Your Lab Directory

## Goal

Create and enter a dedicated directory for the NodePort Service lab.

### Evidence

#### Screenshot 1 — Terminal showing that you are working inside `~/k8s-labs/services/nodeport`

![task 1](screenshots/Screenshot%20wit%20Terminal%20showin%20dt%20u%20are%20workin%20inside_k8s_5.png)

### Notes

**1. What is the difference between ClusterIP and NodePort?**

ClusterIP exposes a Service internally within the Kubernetes cluster, while NodePort exposes a Service through a port on each Kubernetes node, allowing external access when networking permits.

**2. What does a NodePort Service allow users to do?**

It allows users to access applications running inside Kubernetes using a node's IP address and an assigned port.

---

# Task 2 — Create a Healthy NGINX Deployment

## Goal

Create a healthy NGINX Deployment that can act as the backend for the NodePort Service.

### Evidence

#### Screenshot 2 — Completed `00-nginx-deploy.yaml` manifest

![task 2](screenshots/Screenshot%20with%20Completed%2000-nginx-deploy.yaml%20manifest_7.png)

---

#### Screenshot 3 — Output showing successful rollout and two ready NGINX Pods

![task 2](screenshots/Screenshot%20with%20Output%20showing%20successful%20rollout%20and%202_R.png)

### Notes

**1. Which label will the NodePort Service use to find the NGINX Pods?**

app: nginx

**2. Why must the Pods be ready before they can receive Service traffic?**

Readiness ensures that Pods are healthy and ready to handle incoming requests before Kubernetes includes them as usable Service endpoints.

**3. What port does the NGINX container listen on?**

Port 80.

---

# Task 3 — Create the NodePort Service

## Goal

Create a NodePort Service that exposes the NGINX application through a port on every cluster node.

### Evidence

#### Screenshot 4 — Completed `01-nginx-svc-nodeport.yaml` manifest

![task 3](screenshots/Screenshot%20wit%20Completed%2001-nginx-svc-nodeport.yaml%20manifest_4.png)

---

#### Screenshot 5 — Output showing the NodePort Service and its endpoints or EndpointSlices

![task 5](screenshots/Screenshot%20wit%20Output%20showin%20d%20NodePort%20Service%20n%20its%20endpts.png)

### Notes

**1. What is the purpose of `type: NodePort`?**

It exposes a Kubernetes Service on a port across the cluster nodes, allowing external clients to access applications using a node IP address and the assigned NodePort.

**2. What is the difference between `port`, `targetPort`, and `nodePort`?**

port: The port exposed by the Kubernetes Service.
targetPort: The port on which the application container receives traffic.
nodePort: The port opened on each Kubernetes node for external access.

**3. What NodePort was assigned or selected in your environment?**

aremufemmy@RCCGDONNYC:~/k8s-labs/services/nodeport$ kubectl get endpoints nginx-svc-nodeport -o wide
Warning: v1 Endpoints is deprecated in v1.33+; use discovery.k8s.io/v1 EndpointSlice
NAME                 ENDPOINTS                       AGE
nginx-svc-nodeport   10.244.0.50:80,10.244.0.51:80   81s
Kubernetes automatically assigned NodePort 32305.

**4. Why do Service endpoints need ready Pods?**

Ready Pods are healthy and able to handle incoming requests. Kubernetes uses readiness status to determine which Pods should receive Service traffic.

---

# Task 4 — Find a Node IP and Test the NodePort

## Goal

Find a reachable node IP and access the NodePort Service.

### Evidence

#### Screenshot 6 — Output of `kubectl get nodes -o wide`

![task 4](screenshots/Screenshot%20with%20Output%20of%20kubectl%20get%20nodes%20-o%20wide_3.png)

---

#### Screenshot 7 — Output showing the `tester` Pod running

![task 4](screenshots/Screenshot%20with%20Output%20showing%20the%20tester%20Pod%20running_11.png)

---

#### Screenshot 8 — Successful NodePort response from the laptop or the `tester` Pod

If neither route is supported by your environment, include the failed request output and ready endpoint evidence, and explain the limitation in your Notes.

![task 4](screenshots/Screenshot%20with%20Successful%20NodePort%20response%20frm%20d.png)

### Notes

**1. Which node IP and NodePort did you test?**

Node IP and NodePort tested:172.24.0.2:32305.

**2. Was your laptop able to reach the NodePort directly? Explain your result.**

We have confirmed access from the tester Pod inside the cluster. Laptop access has not yet been tested.

**3. Why can external NodePort access behave differently in Minikube, kind, Docker Desktop, and cloud clusters?**

Kubernetes environments such as Minikube, kind, Docker Desktop, and cloud clusters handle node networking, port mapping, NAT, and firewalls differently. A NodePort may work inside the cluster without being directly reachable from the host machine.

---

# Task 5 — Optional: Test the Label-Selector Contract

## Goal

Break the Service selector, observe that usable endpoints disappear, and restore the correct selector.

Complete the Evidence and Notes below only if you attempted this optional task.

### Evidence

#### Screenshot 9 — `02-nginx-svc-selector-broken.yaml` showing the incorrect selector

![task 5](screenshots/Screensht%20wit%2002-nginx-svc-selector-brkn_yaml%20showin%20d%20incorrect%20selector.png)

---

#### Screenshot 10 — Empty endpoints or EndpointSlice output after applying the broken selector

1[task](screenshots/Screenshot%20with%20Empty%20endpoints%20or%20EndpointSlice%20output%20after%20applying.png)

---

#### Screenshot 11 — Restored endpoints or EndpointSlices after applying the original Service manifest

![task 5](screenshots/Screenshot%20Restored%20endpoints%20or%20EndpointSlices%20afta%20applying%20the%20original%20S.png)

### Notes

**1. Why did the Service endpoints become empty?**

The Service selector was changed to app: does-not-match, but the NGINX Pods were labelled app: nginx. Kubernetes could not find matching Pods, so the Service had no backend endpoints.

**2. Why is NodePort only useful when matching ready Pods exist?**

NodePort exposes a Service through a port on the Kubernetes nodes, but the Service still needs matching, ready backend Pods to receive and process requests.

**3. How did you restore the backend connectivity?**

Restoring the selector to app: nginx allowed Kubernetes to identify the two ready NGINX Pods and add them back to the EndpointSlice. Requests to NodePort 32305 then succeeded again.

---

# Task 6 — Observe Resilience During Pod Churn

## Goal

Delete one NGINX Pod and observe its replacement while the Service continues routing traffic to available Pods.

### Evidence

#### Screenshot 12 — Output of the Pod deletion command

![task 6](screenshots/Screenshot%20with%20Output%20of%20the%20Pod%20deletion%20command.png)

---

#### Screenshot 13 — Output showing the replacement Pod and two running, ready Pods

![task 6](screenshots/Screenshot%20with%20Output%20showing%20the%20replacement%20Pod%20and%202.png)

### Notes

**1. What happened after you deleted one Pod?**

After I deleted one NGINX Pod, Kubernetes automatically detected that the number of available Pods was below the desired replica count. It created a replacement Pod, which became 1/1 Running.

**2. Which Kubernetes resource created the replacement Pod?**

The ReplicaSet created the replacement Pod. The Deployment manages the ReplicaSet and ensures that the desired number of replicas is maintained.

**3. Why can the NodePort Service continue working during Pod churn?**

The NodePort Service could continue routing traffic to the remaining ready NGINX Pod while the deleted Pod was being replaced. Once the replacement Pod became ready, Kubernetes added it to the Service endpoints.

---

# Task 7 — Share Your NodePort Learning on LinkedIn

## Goal

Share what you learned about NodePort Services, backend Pods, and network access in your Kubernetes environment.

### Evidence

#### Screenshot 14 — Published LinkedIn post about your NodePort Service lab

![linkedIn](screenshots/Screenshot%20with%20Published%20LinkedIn%20post%20Kubernates%205.png)

#### Linkedin post link

`https://lnkd.in/p/ex8mkzHS`

---

# Submission Instructions

- Include the completed YAML manifests created during the lab.
- Add all required screenshots: Screenshots 1–8 and 12–14.
- Include Screenshots 9–11 only if you attempted optional Task 5.
- Complete the Notes for Tasks 1–4 and Task 6.
- Complete Task 5 Notes only if you attempted that task.
- Use clear, readable screenshots showing the relevant commands and outputs.
- Ensure your full name is visible in the required screenshots.
- If NodePort access is restricted by your environment, include the failed request output, ready endpoint evidence, and an explanation.
- Do not expose passwords, access tokens, private keys, kubeconfig contents, or account IDs.

---

# Completion Checklist

- [✅] Created and entered `~/k8s-labs/services/nodeport` (Screenshot 1)
- [✅] Created and applied the NGINX Deployment manifest (Screenshot 2)
- [✅] Confirmed successful rollout and two ready NGINX Pods (Screenshot 3)
- [✅] Created the NodePort Service manifest (Screenshot 4)
- [✅] Verified the Service and its endpoints or EndpointSlices (Screenshot 5)
- [✅] Recorded the assigned or selected NodePort
- [✅] Identified a node IP (Screenshot 6)
- [✅] Confirmed that the `tester` Pod is running (Screenshot 7)
- [✅] Tested NodePort access, or documented the environment limitation with evidence (Screenshot 8)
- [✅] Optional: Tested and restored the Service selector (Screenshots 9–11)
- [✅] Deleted one NGINX Pod (Screenshot 12)
- [✅] Confirmed the replacement Pod and two running, ready Pods (Screenshot 13)
- [✅] Completed all required Notes
- [✅] Published the LinkedIn post (Screenshot 14)
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
