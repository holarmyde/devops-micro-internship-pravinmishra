# Assignment 3 — Docker Networking

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will explore Docker networking by using default bridge, custom bridge, multiple bridge, and host network modes. You will verify container communication, service discovery, network isolation, public access, and host networking on a Linux VM or EC2 instance.

---

# Task 1 — Deploy a Standalone Application Using the Default Bridge Network

## Goal

Deploy an Nginx web server using Docker’s default bridge network and access it through the VM public IP address.

### Evidence

#### Screenshot 1 — Available Docker Networks

Add a screenshot of the terminal showing:

```bash
docker network ls
```

The output must include the default `bridge`, `host`, and `none` networks.

![task 1](screenshots/Screenshot%20with%20Output%20of%20docker%20network%20ls.png)

---

#### Screenshot 2 — Nginx Image Pull

Add a screenshot of the terminal showing successful completion of:

```bash
docker pull nginx:alpine
```

![task 1](screenshots/Screenshot%20with%20Output%20of%20docker%20images.png)

---

#### Screenshot 3 — Running `myweb` Container

Add a screenshot of the terminal showing:

```bash
docker ps
```

The output must show the running `myweb` container with:

```text
0.0.0.0:80->80/tcp
```

![task 1](screenshots/Screenshot%20with%20Output%20of%20docker%20search%20nginx.png)

---

#### Screenshot 4 — Nginx Welcome Page

Add a browser screenshot showing the Nginx Welcome Page at:

```text
http://<YOUR-VM-PUBLIC-IP>
```

Ensure that the VM public IP is visible in the address bar. Add your full name as a clear caption directly below the screenshot.

![task 1](screenshots/Screenshot%20with%20Successful%20docker%20pull%20nginx.png)

---

<<<<<<< HEAD
#### Screenshot 5 — Output of `docker ps` showing the running `myweb` container

![task 1](screenshots/Screenshot%20with%20Output%20of%20docker%20ps%20.png)

---

#### Screenshot 6 — Browser displaying the Nginx Welcome Page using the Public IP address

![task 1](screenshots/Screenshot%20with%20%20Browser%20displaying%20the%20Nginx.png)

---

# Task 2 — Connect Multiple Containers Using a Custom Bridge Network
=======
# Task 2 — Connect Containers Using a Custom Bridge Network
>>>>>>> upstream/main

## Goal

Create a custom bridge network and verify that containers can communicate using container names rather than IP addresses.

### Evidence

#### Screenshot 5 — Custom Bridge Network Created

Add a screenshot of the terminal showing `mynetwork` in:

```bash
docker network ls
```

![task 2](screenshots/Screenshot%20with%20Output%20of%20docker%20netwk.png)

---

#### Screenshot 6 — Running `web` and `client` Containers

Add a screenshot of the terminal showing:

```bash
docker ps
```

The output must show both `web` and `client` containers running without published host ports.

![task 2](screenshots/Screenshot%20with%20Output%20of%20docker%20netwrk%20ls.png)

---

#### Screenshot 7 — Service Discovery by Container Name

Add a screenshot of the terminal showing successful output from:

```bash
docker exec client wget -qO- http://web
```

The output must display the Nginx Welcome Page HTML.

![task 2](screenshots/Screenshot%20with%20Frontend%20Dockerfile.png)

---

#### Screenshot 8 — Custom Network Inspection

Add a screenshot of the terminal showing:

```bash
docker network inspect mynetwork
```

The output must show both `web` and `client` connected to `mynetwork`.

![task 2](screenshots/Screenshot%20with%20Frontend%20Dockerfile.png)

---

<<<<<<< HEAD
#### Screenshot 5 — Output of `docker ps` showing the frontend container

![task 2](screenshots/Screenshot%20with%20%20Output%20of%20docker%20ps%20showing%20both.png)

---

#### Screenshot 6 — Output of `docker ps` showing both frontend and backend containers

![task 2](screenshots/Screenshot%20with%20%20Output%20of%20docker%20ps%20showing%20both.png)

---

#### Screenshot 7 — Output of `docker network inspect mynetwork`

![task 2](screenshots/Screenshot%20with%20Output%20of%20docker%20ps%20.png)

---

#### Screenshot 8 — Successful `curl http://<Public-IP>` showing "Hello from Frontend"

![task 2](screenshots/Screenshot%20with%20Successful%20curl%20http.png)

---

#### Screenshot 9 — Successful `curl backend` output from the frontend container showing the Nginx Welcome Page

![task 2](screenshots/Screenshot%20with%20%20Output%20of%20docker%20ps%20showing%20both.png)

---

# Task 3 — Deploy a Multi-Tier Application Using Multiple Docker Networks
=======
# Task 3 — Demonstrate Multi-Network Isolation
>>>>>>> upstream/main

## Goal

Deploy frontend, backend, and database containers across two separate Docker networks. Verify allowed communication and confirm that the frontend cannot directly reach the database.

### Evidence

#### Screenshot 9 — Two Docker Networks

Add a screenshot of the terminal showing:

```bash
docker network ls
```

The output must include both `frontend-network` and `backend-network`.

![task 3](screenshots/Screenshot%20with%20docker%20network%20inspect%20backend-network.png)

---

#### Screenshot 10 — Running Multi-Network Containers

Add a screenshot of the terminal showing:

```bash
docker ps
```

The output must show:

- `frontend` with the published port mapping `0.0.0.0:80->80/tcp`
- `backend` without a published host port
- `db` without a published host port

![task 3](screenshots/Screenshot%20with%20docker%20network%20create%20frontend-network.png)

---

#### Screenshot 11 — Frontend Network Inspection

Add a screenshot of the terminal showing:

```bash
docker network inspect frontend-network
```

The output must show `frontend` and `backend`.

![task 3](screenshots/Screenshot%20with%20Project%20Structure.png)

---

#### Screenshot 12 — Backend Network Inspection

Add a screenshot of the terminal showing:

```bash
docker network inspect backend-network
```

The output must show `backend` and `db`.

![task 3](screenshots/Screenshot%20with%20Database%20dockerfile.png)

---

#### Screenshot 13 — Frontend-to-Backend Communication

Add a screenshot of the terminal showing successful output from:

```bash
docker exec frontend wget -qO- http://backend
```

The output must display the Nginx Welcome Page HTML.

![task 3](screenshots/Screenshot%20with%20backend%20Dockerfile.png)

---

#### Screenshot 14 — Backend-to-Database Communication

Add a screenshot of the terminal showing a successful connection to `db` on port `27017` from the `backend` container.

![task 3](screenshots/Screenshot%20with%20Project%20Structure.png)

---

#### Screenshot 15 — Frontend-to-Database Isolation

Add a screenshot of the terminal showing that the `frontend` container cannot reach `db` on port `27017`.

The output must include:

```text
Expected result: frontend cannot reach db
```

![task 3](screenshots/Screenshot%20with%20%20Terminal%20showing%20docker%20images.png)

---

#### Screenshot 16 — Public Frontend Access

<<<<<<< HEAD
![task 3](screenshots/Screenshot%20with%20%20Output%20of%20docker%20ps%20showing%20both.png)
=======
Add a browser screenshot showing the Nginx Welcome Page from the `frontend` container at:
>>>>>>> upstream/main

```text
http://<YOUR-VM-PUBLIC-IP>
```

<<<<<<< HEAD
#### Screenshot 9 — Output of `docker network inspect backend-network`

![task 3](screenshots/Screenshot%20with%20docker%20network%20inspect%20backend-network.png)

---

#### Screenshot 10 — Output of `docker network inspect frontend-network`

![task 3](screenshots/Screenshot%20with%20docker%20network%20create%20frontend-network.png)

---

#### Screenshot 11 — Browser showing the frontend application

![task 2](screenshots/Screenshot%20wit%20Browser%20displayin%20d%20deployed%20website.png)

---

#### Screenshot 12 — Successful `curl api` from the frontend container

![task 3](screenshots/Screenshot%20with%20Successful%20curl%20http.png)

---

#### Screenshot 13 — MongoDB connection using `mongosh`

![task 3](screenshots/Screenshot%20with%20MongoDB%20connection%20using%20mongosh.png)

---

#### Screenshot 14 — Successful document insertion

![task 3](screenshots/Screenshot%20with%20Successful%20document%20insertion.png)

---

#### Screenshot 15 — Successful retrieval of the inserted document
=======
Ensure that the VM public IP is visible in the address bar. Add your full name as a clear caption directly below the screenshot.
>>>>>>> upstream/main

![task 3](screenshots/Screenshot%20Successful%20retrieval%20of%20the%20inserted%20document.png)

---

# Task 4 — Deploy an Application Using Docker Host Network Mode

## Goal

Run an Nginx container using Docker host network mode and compare it with bridge networking.

### Evidence

#### Screenshot 17 — Running Host-Networked Container

Add a screenshot of the terminal showing:

```bash
docker ps
```

The output must show the running `fastapp` container.

![task 4](screenshots/Screenshot%20with%20%20Output%20of%20docker%20run%20--network%20host.png)

---

#### Screenshot 18 — Host Network Mode Verification

Add a screenshot of the terminal showing output from:

```bash
docker inspect fastapp | grep '"NetworkMode"'
```

The output must confirm:

```text
"NetworkMode": "host"
```

![task 4](screenshots/Screenshot%20with%20Output%20of%20docker%20ps%20with%20fastapp.png)

---

#### Screenshot 19 — Host-Networked Nginx Page

Add a browser screenshot showing the Nginx Welcome Page at:

```text
http://<YOUR-VM-PUBLIC-IP>
```

Ensure that the VM public IP is visible in the address bar. Add your full name as a clear caption directly below the screenshot.

![task 4](screenshots/Screenshot%20with%20%20Browser%20displaying%20the%20Nginx.png)

---

#### Screenshot 20 — Host-Networked Container Cleanup

Add a screenshot of the terminal showing successful completion of:

```bash
docker stop fastapp
docker rm fastapp
```

![task 4](screenshots/Screenshot%20with%20%20Output%20of%20docker%20run%20--network%20host.png)

---

# Networking Notes

<<<<<<< HEAD
![task 4](screenshots/Screenshot%20with%20docker%20stop%20fastapp.png)
=======
Write a short note explaining:

- Default bridge networking
- Container-name communication on a custom bridge network
- Why the frontend could not access the database in Task 3
- The difference between bridge mode and host network mode

Write your note here.
>>>>>>> upstream/main

---

# LinkedIn Requirement

## Goal

Create a LinkedIn post about the Docker networking modes explored, one key lesson about container isolation, and the learning outcomes from this assignment.

### Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/eyXz_k_P`

---

#### LinkedIn Post Screenshot

<<<<<<< HEAD
![LinkedIn](screenshots/Screenshot%20with%20published%20LinkedIn%20-2.png)
=======
Add a screenshot of the published LinkedIn post here.
>>>>>>> upstream/main

---

# Submission Instructions

- Complete all tasks in sequence.
- Include Screenshots 1–20 exactly as specified.
- Include the Networking Notes section.
- Include the LinkedIn post URL and screenshot.
- Ensure that your full name is visible in all terminal screenshots.
- Add your full name as a caption below each browser screenshot that shows the standard Nginx page.
- Do not expose private keys, passwords, access keys, tokens, account IDs, or other sensitive information.

---

# Completion Checklist

<<<<<<< HEAD
- [✅] Task 1: Standalone app on default bridge network deployed and verified (Screenshots 1–6)
- [✅] Task 2: Custom bridge network with frontend/backend communication verified (Screenshots 1–9)
- [✅] Task 3: Multi-tier app across two networks deployed and verified end to end (Screenshots 1–15)
- [✅] Task 4: Host network mode deployment verified and cleaned up (Screenshots 1–5)
- [✅] No sensitive information exposed
=======
- [ ] Completed on a Linux VM or EC2 instance
- [ ] Docker Engine is running
- [ ] HTTP port 80 is allowed in the VM firewall or cloud security rules
- [ ] Default bridge networking verified
- [ ] Custom bridge network created
- [ ] Container-name communication verified
- [ ] `frontend-network` and `backend-network` created
- [ ] Frontend-to-backend communication verified
- [ ] Backend-to-database communication verified
- [ ] Frontend-to-database isolation verified
- [ ] Only the frontend published port 80 in Task 3
- [ ] Host network mode verified
- [ ] All required screenshots included
- [ ] Networking Notes completed
- [ ] LinkedIn post URL and screenshot included
- [ ] Full name visible in terminal screenshots
- [ ] Browser screenshots include full-name captions
- [ ] No sensitive information exposed
>>>>>>> upstream/main

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
