# Assignment 09 — Kubernetes Services: LoadBalancer on AKS

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

Deploy a healthy NGINX application to Azure Kubernetes Service (AKS), expose it publicly using a LoadBalancer Service, observe readiness behavior, and scale the application behind the same public endpoint.

---

# Task 1 — Create or Connect to an AKS Cluster

## Goal

Create a small AKS cluster for this lab, or connect to an existing approved AKS lab cluster.

### Evidence

#### Screenshot 1 — Output of `kubectl get nodes` showing ready AKS node(s)

![task 1](screenshots/Screenshot%20with%20%20Output%20of%20kubectl%20get%20nodes%20showin%20ready%20AKS%20node(s).png)

### Notes

**1. Why does this lab use a cloud-capable Kubernetes environment such as AKS for the LoadBalancer Service?**

This lab uses AKS because a LoadBalancer Service requires integration with a cloud provider to provision an external load balancer and public IP address. AKS provides Azure networking integration that allows the application to be accessed securely from the internet.

**2. What Azure resources can Kubernetes provision when a LoadBalancer Service is created?**

Kubernetes can provision an Azure Load Balancer, a public IP address, and the necessary load-balancing rules and backend configurations. These resources allow incoming internet traffic to be forwarded to healthy application Pods running inside the AKS cluster.

**3. Why is it important to delete unused AKS resources after the lab?**

Deleting unused AKS resources is important because they can continue generating costs, including charges for compute nodes, networking resources, and public IP addresses. Cleaning up resources also improves cloud resource management and prevents unnecessary consumption of Azure resources.

---

# Task 2 — Set Up Your Lab Directory

## Goal

Create and enter a dedicated directory for the LoadBalancer Service manifests.

### Evidence

#### Screenshot 2 — Terminal showing the output of `pwd` confirming that you are working inside `~/k8s-labs/services/loadbalancer`

![task 1](screenshots/Screenshot%20with%20%20Output%20of%20kubectl%20get%20nodes%20showin%20ready%20AKS%20node(s).png)

### Notes

**1. Why is it useful to keep Kubernetes manifests organized in dedicated directories?**

Keeping Kubernetes manifests in dedicated directories makes project files easier to organize, manage, locate, and maintain. It also reduces confusion between different Kubernetes resources and supports better version control and collaboration.
**2. What are the names of the main Deployment and LoadBalancer Service YAML files you will create in this assignment?**

The main YAML files created for this assignment are:

Deployment manifest: 00-nginx-deploy.yaml

LoadBalancer Service manifest: 01-nginx-svc-loadbalancer.yaml

The readiness patch file is 02-readiness-broken.yaml.

---

# Task 3 — Deploy a Healthy NGINX Application

## Goal

Create a healthy NGINX Deployment with readiness and liveness probes.

### Evidence

#### Screenshot 3 — Completed `00-nginx-deploy.yaml` manifest

![task 3](screenshots/Screenshot%20with%20%20Completed%2000-nginx-deploy.yaml%20manifest_U.png)

---

#### Screenshot 4 — Output showing successful rollout and two ready NGINX Pods

![task 3](screenshots/Screenshot%20with%20Output%20showing%20successful%20rollout%20and%202_R.png)

### Notes

**1. Why must Pods be ready before they should receive public traffic?**

Pods must be ready before receiving public traffic to ensure that the application is running correctly and can successfully handle incoming requests. Routing traffic to unready Pods could result in failed requests, errors, or an unreliable user experience.

**2. What is the role of the readiness probe in this Deployment?**

The readiness probe checks whether the NGINX application is ready to receive traffic by sending an HTTP request to the root path / on port 80. If the probe succeeds, the Pod is considered ready and can receive traffic through the Kubernetes Service. If it fails, the Pod is excluded from ready traffic endpoints.

**3. What is the role of the liveness probe in this Deployment?**

The liveness probe checks whether the NGINX container is still functioning correctly. If the probe repeatedly fails, Kubernetes restarts the container to recover from the failure. This helps maintain application health and availability

---

# Task 4 — Create and Test the LoadBalancer Service

## Goal

Create a public LoadBalancer Service and verify that the NGINX application is reachable from the internet.

### Evidence

#### Screenshot 5 — Completed `01-nginx-svc-loadbalancer.yaml` manifest

![task 4](screenshots/Screenshot%20with%20Completed%2001-nginx-svc-clusterip_yaml%20manifest.png)

---

#### Screenshot 6 — Output showing the Service with an assigned EXTERNAL-IP

![task 4](screenshots/Screenshot%20with%20Output%20of%20kubectl%20get%20nodes%20-o%20wide_3.png)

---

#### Screenshot 7 — Successful curl output from the public IP

![task 4](screenshots/Screenshot%20wit%20Output%20showin%20d%20NodePort%20Service%20n%20its%20endpts.png)

### Notes

**1. What does `type: LoadBalancer` instruct AKS to do?**

The type: LoadBalancer configuration instructs AKS to provision an external Azure Load Balancer and assign a public IP address to the Kubernetes Service. This makes the NGINX application accessible from outside the Kubernetes cluster.
**2. What public IP was assigned to your Service?**

The public IP address assigned to my NGINX LoadBalancer Service was:

[172.24.0.2]

**3. How does the public request eventually reach the NGINX Pods?**

A public request is sent to the external IP address of the Azure Load Balancer. Azure forwards the request to the AKS cluster, where the Kubernetes Service uses its selector to identify the NGINX Pods. The request is then routed to a healthy, ready NGINX Pod, which processes the request and returns the response.

**4. Why should you wait for EXTERNAL-IP before testing the Service?**

The EXTERNAL-IP may initially display <pending> while Azure provisions the required networking resources. Waiting until a public IP address is assigned ensures that the Service has an external endpoint that can be used to test public connectivity.

---

# Task 5 — Observe How Readiness Protects Public Traffic

## Goal

Intentionally break readiness and observe how Kubernetes excludes unready Pods from ready Service backends while existing healthy Pods continue serving public traffic.

### Evidence

#### Screenshot 8 — Completed `02-readiness-broken.yaml` patch file

![task 5](screenshots/Screenshot%20with%20Completed%20`02-readiness-broken.yaml`%20patch%20file.png)

---

#### Screenshot 9 — Output showing the expected incomplete rollout or readiness failure

![task 5](screenshots/Screenshot%20with%20Output%20showing%20the%20rollout%20timeout%20or%20incomplete%20rollout.png)

---

#### Screenshot 10 — EndpointSlice output showing ready healthy backends and the effect of the readiness failure

![task 5](screenshots/Screenshot%20wit%20Output%20showin%20d%20NodePort%20Service%20n%20its%20endpts.png)

---

#### Screenshot 11 — Successful public curl response after recovery

![task 5](screenshots/Screenshot%20with%20Pod%20output%20showing%20an%20increased%20RESTARTS%20count.png)

### Notes

**1. Why did the rollout not complete?**

The rollout did not complete because the readiness probe was intentionally configured to access the nonexistent path /does-not-exist. NGINX returned an unsuccessful HTTP response, preventing the new Pod from becoming ready. Kubernetes therefore could not complete the rolling update.

**2. Why can the public endpoint still work while new Pods fail readiness?**

The public endpoint can still work because the existing healthy NGINX Pods remain ready and continue receiving traffic. The Deployment uses maxUnavailable: 0, which prevents the existing available Pods from being removed during the incomplete rollout.
**3. What would happen if Kubernetes routed traffic to Pods before they were ready?**

If Kubernetes routed traffic to Pods before they were ready, users could experience failed HTTP requests, application errors, slow responses, or service interruptions. Readiness probes help prevent these problems by ensuring that only ready Pods receive Service traffic.

**4. How did you restore the Deployment successfully?**

I restored the Deployment by reapplying the original healthy manifest using:

kubectl apply -f 00-nginx-deploy.yaml

The original readiness probe was restored to use the / path. I then waited for the rollout to complete using:

kubectl rollout status deployment/nginx-deployment

After the recovery, I verified that the two NGINX Pods were running and ready and that the public endpoint was responding successfully.

---

# Task 6 — Scale the Deployment Behind the Load Balancer

## Goal

Scale the NGINX Deployment and confirm that more ready Pods become available behind the same public endpoint.

### Evidence

#### Screenshot 12 — Two ready NGINX Pods verified initially; four ready Pods verified after scaling

![task 5](screenshots/Screenshot%20with%20Output%20showing%20successful%20rollout%20n%202.png)

---

#### Screenshot 13 — Successful public curl response after scaling

![task 6](screenshots/Screenshot%20wit%20Output%20showin%20d%20NodePort%20Service%20n%20its%20endpts.png)

### Notes

**1. What changed after scaling the Deployment from two to four replicas?**

After scaling the Deployment from two to four replicas, Kubernetes created two additional NGINX Pods. The Deployment now maintained four running and ready Pods, and the Service EndpointSlices included the additional ready Pod endpoints. This increased the application's capacity to handle incoming traffic.

**2. Did the public IP change after scaling? Explain why or why not.**

No, the public IP address did not change after scaling. The public IP belongs to the LoadBalancer Service, not to individual Pods. Scaling the Deployment changes the number of backend Pods without requiring the Service to create a new public IP address.

**3. Why does scaling behind a LoadBalancer Service not require users to change the URL or IP they use?**

Scaling behind a LoadBalancer Service does not require users to change the URL or IP address because the Service provides a stable public endpoint. Kubernetes automatically updates the Service endpoints to include the additional ready Pods, allowing traffic to be distributed among them while users continue accessing the application through the same public IP address.

---

# Task 7 — Share Your LoadBalancer Lab on LinkedIn

## Goal

Share what you learned about exposing an NGINX application publicly through a LoadBalancer Service on AKS.

### Evidence

#### Screenshot 14 — Published LinkedIn post about your LoadBalancer lab on AKS

![linkedIn](screenshots/Screenshot%20with%20kubernates%20linkedIn%20post_9.png)

#### LinkedIn post link

`https://lnkd.in/p/eKXvHkiY`

---

# Submission Instructions

- Include the completed YAML files:
  - `00-nginx-deploy.yaml`
  - `01-nginx-svc-loadbalancer.yaml`
  - `02-readiness-broken.yaml`
- Add all required screenshots, numbered 1–14, under their corresponding Evidence sections.
- Use clear, readable screenshots showing the relevant commands, configuration, and outputs.
- Ensure your full name is visible in the required screenshots.
- Complete the Notes for Tasks 1–6 in your own words.
- Capture all required evidence before cleaning up Azure resources.
- Plan or complete Azure cleanup according to your lab ownership and reuse arrangements.
- Do not expose passwords, tokens, private keys, cluster credentials, kubeconfig contents, subscription IDs, account IDs, or infrastructure details you are not permitted to share.

---

# Completion Checklist

- [✅] Created or connected to an approved AKS lab cluster
- [✅] Confirmed ready AKS node(s) (Screenshot 1)
- [✅] Created and entered the LoadBalancer lab directory (Screenshot 2)
- [✅] Created the NGINX Deployment manifest (Screenshot 3)
- [✅] Verified successful rollout and two ready NGINX Pods initially (Screenshot 4)
- [✅] Created the LoadBalancer Service manifest (Screenshot 5)
- [✅] Verified the assigned external IP (Screenshot 6)
- [✅] Verified successful public access using curl (Screenshot 7)
- [✅] Created the broken readiness patch (Screenshot 8)
- [✅] Observed the expected incomplete rollout or readiness failure (Screenshot 9)
- [✅] Inspected EndpointSlices during the readiness failure (Screenshot 10)
- [✅] Restored the healthy Deployment and verified public access (Screenshot 11)
- [✅] Scaled the Deployment to four ready Pods and verified updated Service backends (Screenshot 12)
- [✅] Verified public access after scaling (Screenshot 13)
- [✅] Completed the Notes for Tasks 1–6
- [✅] Published the LinkedIn post (Screenshot 14)
- [✅] Included all required YAML files and screenshots
- [✅] Planned or completed Azure cleanup
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
