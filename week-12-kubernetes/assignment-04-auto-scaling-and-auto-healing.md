# Assignment 4 — Auto-Scaling & Auto-Healing: Let Kubernetes React for You

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

Create Deployments, test Pod auto-healing, configure CPU requests and limits, verify Metrics Server, and create a Horizontal Pod Autoscaler.

Optionally generate CPU load to observe how the HPA responds.

---

# Task 0 — Check the Existing Cluster Resources

## Goal

Confirm that you are connected to your learning cluster and inspect existing Deployments and HPAs.

### Evidence

No screenshots required.

---

# Task 1 — Set Up Your Lab Directory

## Goal

Create and enter a dedicated directory for the auto-scaling lab.

### Evidence

#### Screenshot 1 — Terminal showing that you are working inside `~/k8s-labs/autoscaling`

![task 1](screenshots/Screenshot%20with%20Terminal%20showing%20that%20you%20are%20working%20inside%20.png)

---

### Notes

**1. What is the difference between auto-healing and auto-scaling?**

Auto-healing automatically replaces failed or deleted Pods to maintain the desired replica count. Auto-scaling automatically increases or decreases the number of Pods based on workload demand.

---

**2. Which Kubernetes feature will you use for CPU-based auto-scaling?**

The Horizontal Pod Autoscaler (HPA).

---

# Task 2 — Test Auto-Healing with a Deployment

## Goal

Create a Deployment with two replicas, delete one Pod, and observe Kubernetes create a replacement Pod.

### Evidence

#### Screenshot 2 — Completed `autohealing-deployment.yaml` manifest

![task 2](screenshots/Screenshot%20with%20Completed%20autohealing-deployment.yaml%20manifest.png)

---

#### Screenshot 3 — Output of `kubectl get pods` showing two running Pods before deletion

![task 2](screenshots/Screenshot%20with%20Output%20of%20kubectl%20get%20pods%20showing%20two%20running.png)

---

#### Screenshot 4 — Output of the `kubectl delete pod` command

![task 2](screenshots/Screenshot%20with%20Output%20of%20the%20kubectl%20delete%20pod%20command.png)

---

#### Screenshot 5 — Output of `kubectl get pods` showing the replacement Pod and two running Pods

![task 2](screenshots/Screenshot%20with%20Output%20of%20kubectl%20get%20pods%20showing%20d%20replacement.png)

---

### Notes

**1. What happened after you deleted one Pod?**

Kubernetes automatically detected that the number of running Pods had fallen below the desired replica count and created a replacement Pod.

---

**2. What Kubernetes resource maintains the desired replica count for this Deployment?**

The ReplicaSet maintains the desired number of Pods on behalf of the Deployment.

---

**3. What proves that auto-healing worked?**

The successful creation of a new Pod after deleting the original Pod, restoring the Deployment to two running Pods.

---

# Task 3 — Create a Deployment for HPA

## Goal

Create a Deployment with CPU resource requests and limits so that HPA can calculate CPU utilization.

### Evidence

#### Screenshot 6 — Completed `hpa-deployment.yaml` manifest showing CPU requests and limits

![task 3](screenshots/Screenshot%20with%20Completed%20hpa-deployment.yaml%20manifest.png)

---

#### Screenshot 7 — Output showing `nginx-deployment` and its running Pods

![task 3](screenshots/Screenshot%20with%20Output%20showing%20nginx-deployment%20n%20its%20runnin%20Pods.png)

---

### Notes

**1. What does `cpu: "100m"` mean?**

It means the container requests 100 millicores, equivalent to 0.1 CPU core.

---

**2. What is the difference between a CPU request and a CPU limit?**

A CPU request specifies the amount of CPU resources Kubernetes uses when scheduling a Pod. A CPU limit specifies the maximum CPU resources the container can use.

---

**3. Why does CPU-based HPA require CPU requests?**

HPA uses CPU requests as the baseline for calculating CPU utilization percentages. Without CPU requests, it cannot properly calculate the percentage required for CPU-based scaling.

---

# Task 4 — Enable and Verify Metrics Server

## Goal

Ensure the cluster can collect resource metrics for the HPA.

### Evidence

#### Screenshot 8 — Output showing Metrics Server running, or `kubectl top pods` displaying Pod metrics

![task 4](screenshots/Screenshot%20with%20Output%20showing%20Metrics%20Server%20running,%20or%20kubectl.png)

---

### Notes

**1. Why does HPA need Metrics Server?**

Metrics Server collects CPU and memory usage data from Kubernetes Pods and Nodes. HPA uses this information to make scaling decisions.

---

**2. What issue can occur if Metrics Server is not working?**

HPA may be unable to retrieve resource metrics, causing the CPU utilization target to display <unknown> and preventing proper autoscaling.

---

**3. What does `kubectl top pods` help you check?**

It displays the CPU and memory consumption of running Pods.

---

# Task 5 — Create a Horizontal Pod Autoscaler

## Goal

Create an HPA that keeps at least two Pods running and can scale up to five Pods when average CPU utilization exceeds 50%.

### Evidence

#### Screenshot 9 — The imperative command output OR completed `hpa-nginx.yaml` manifest

![task 5](screenshots/Screenshot%20of%20creation%20of%20hpa%20command.png)

---

#### Screenshot 10 — Output of `kubectl get hpa` and the relevant `kubectl describe hpa` command

![task 5](screenshots/Screenshot%20wit%20d%20imperative%20command%20completed%20hpa-nginx.yaml%20manifest.png)

---

### Notes

**1. What does `minReplicas: 2` mean?**

It means the HPA will maintain at least two Pods for the Deployment.

---

**2. What does `maxReplicas: 5` mean?**

It means the HPA can increase the number of Pods up to a maximum of five.

---

**3. What does `averageUtilization: 50` mean?**

It means HPA targets average CPU utilization of 50% of the CPU requests configured for the Pods.

---

**4. Why should only one HPA manage a Deployment at a time?**

Multiple HPAs managing the same Deployment can conflict by trying to control its replica count independently, resulting in unpredictable scaling behavior.

---

# Task 6 — Optional: Generate CPU Load and Observe Scaling

## Goal

Generate CPU load in one NGINX Pod and observe how HPA responds.

### Evidence

If you skip this task, write “Not completed — optional” and leave the remaining Task 6 placeholders blank.

Task status: Add your status here.

#### Screenshot 11 — Terminal showing the CPU-load command running inside the selected Pod

![task 6](screenshots/Screenshot%20with%20Terminal%20showing%20the%20CPU-load%20command%20running.png)

---

#### Screenshot 12 — Output of `kubectl get hpa` and `kubectl get pods` while HPA evaluates or changes the replica count

![task 6](screenshots/Screenshot%20wit%20Output%20of%20kubectl%20get%20hpa%20n%20kubectl%20get%20pods.png)

---

### Notes

Complete these answers only if you perform the optional test.

**1. Did the HPA change the number of Pods? Explain what you observed.**

Yes. The HPA automatically increased the NGINX Deployment from 2 Pods to 4 Pods when CPU utilization reached 100%, exceeding the configured 50% target. After I stopped the CPU load, CPU utilization dropped to 0%, demonstrating that the HPA was responding to changes in workload.

---

**2. If scaling did not happen, what are two possible reasons?**

Not applicable — scaling occurred.

---

**3. Why is automatic scale-down useful after traffic decreases?**

Automatic scale-down is useful because it reduces unnecessary Pods when traffic or workload decreases. This helps conserve CPU, memory, and other cluster resources while reducing infrastructure costs and maintaining efficient resource utilization.

---

# LinkedIn Requirement

### Evidence

#### Screenshot 13 — Published LinkedIn post showing your name, the required explanation, and the attached Kubernetes screenshot

![LinkedIn](screenshots/Screenshot%20with%20kubernates%20linkedIn%20post.png)

#### Linkedin Post Link

`https://lnkd.in/p/eui8EuUc`

---

# Submission Instructions

- Include required Screenshots 1–10 and Screenshot 13.
- Include Screenshots 11–12 only if you complete Task 6.
- Full Name must be visible in required screenshots.
- Ensure screenshots are clear and readable.
- Include the completed `autohealing-deployment.yaml` and `hpa-deployment.yaml` files.
- Include `hpa-nginx.yaml` if you selected the declarative HPA method.
- Complete the Notes sections for Tasks 1–5 in your own words.
- Complete Task 6 notes only if you perform the optional test.
- Do not expose passwords, tokens, private keys, kubeconfig files, or account IDs.

---

# Completion Checklist

- [✅] Checked the active context and existing cluster resources
- [✅] Created the autoscaling working directory (Screenshot 1)
- [✅] Created `autohealing-deployment.yaml` (Screenshot 2)
- [✅] Confirmed two auto-healing Pods were running (Screenshot 3)
- [✅] Deleted one managed Pod (Screenshot 4)
- [✅] Confirmed that the Pod was automatically replaced (Screenshot 5)
- [✅] Created `hpa-deployment.yaml` with CPU requests and limits (Screenshot 6)
- [✅] Verified `nginx-deployment` and its running Pods (Screenshot 7)
- [✅] Verified Metrics Server and confirmed Pod metrics are available (Screenshot 8)
- [✅] Created one HPA using the selected method (Screenshot 9)
- [✅] Verified minimum replicas of 2, maximum replicas of 5, and a CPU target of 50% (Screenshot 10)
- [✅] Completed Notes for Tasks 1–5
- [✅] Completed Task 6 evidence and notes, or marked it “Not completed — optional”
- [✅] Stopped the CPU-load command and exited the container shell, if Task 6 was completed
- [✅] Published the LinkedIn post with the required explanation and attached screenshot (Screenshot 13)
- [✅] Included the required manifest files
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
