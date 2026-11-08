# Assignment 5 — Deploy EpicBook Web App on Azure VM with Azure Database for MySQL

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will deploy the EpicBook web application on Azure using an Ubuntu Virtual Machine to host the frontend and backend, and Azure Database for MySQL Flexible Server (private access) to store user and product data. You will build the network, provision the resources, deploy the application, and prove that the complete user flow works through the VM's public IP.

---

# Task 1 — Create Network Infrastructure

## Goal

Create a Resource Group and VNet (`10.0.0.0/16`) with `public-subnet` (`10.0.1.0/24`) for the VM and a delegated `private-db-subnet` (`10.0.2.0/24`) for MySQL. Configure and associate the public and private NSGs with the required HTTP, SSH, and MySQL rules.

## Evidence

#### Screenshot 1 — Virtual Network configuration showing the `10.0.0.0/16` address space and both subnets

![task 1](screenshots/Screenshot%20with%20Virtual%20Network%20overview%20showing%20the%20address%20space.png)

---

#### Screenshot 2 — Public and private NSG inbound rules showing HTTP 80, SSH 22, and the MySQL 3306 rule with source `10.0.1.0/24`

![task 1](screenshots/Screenshot%20with%20Public%20and%20private%20NSG%20inbound%20rules%20showing%20ports%2080.png)

---

<<<<<<< HEAD
#### Screenshot 3 — Public IP and Network Interface association for the Virtual Machine

![task 1](screenshots/Screenshot%20with%20Public%20IP%20and%20Network%20Interface%20association%20for%20the%20Virtual%20Machine.png)

---

# Task 2 — Provision Azure Virtual Machine
=======
# Task 2 — Create and Prepare the Azure Virtual Machine
>>>>>>> upstream/main

## Goal

Deploy a supported Ubuntu LTS VM in `public-subnet`, allowing Azure to create its NIC and Public IP. Connect using SSH and install Node.js, npm, Nginx, Git, MySQL Client, and curl.

## Evidence

#### Screenshot 3 — VM Network Interface configuration showing its association with the public subnet and Public IP

![task 2](screenshots/Screenshot%20with%20Virtual%20Machine%20overview%20showing%20Ubuntu.png)

---

#### Screenshot 4 — Virtual Machine overview showing the Ubuntu VM, VM size, Public IP address, and subnet

![task 2](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20software%20installation.png)

---

#### Screenshot 5 — Terminal showing successful installation or version checks for Node.js, npm, Nginx, Git, and MySQL Client

![task 2](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20software%20installation.png)

---

# Task 3 — Configure Azure Database for MySQL and Prepare the EpicBook Database

## Goal

Create Azure Database for MySQL Flexible Server using Private access (VNet Integration) in the delegated database subnet. Clone the EpicBook repository to the VM, create `bookstore`, import the schema and seed data, and verify the database contents.

## Evidence

#### Screenshot 6 — MySQL Flexible Server overview showing the server running and configured with private access

![task 3](screenshots/Screenshot%20with%20Terminal%20showing%20the%20EpicBook%20repository%20cloned%20.png)
---

#### Screenshot 7 — MySQL networking configuration showing the server connected to the private database subnet

![task 3](screenshots/Screenshot%20with%20Nginx%20configuration%20or%20service%20status.png)

---

#### Screenshot 8 — VM terminal showing the `bookstore` database tables and seeded author and book records

![task 3](screenshots/Screenshot%20with%20Backend%20process%20or%20listening-port%20evidence.png)

---

# Task 4 — Deploy and Configure the EpicBook Application

## Goal

Install the EpicBook dependencies, configure `config/config.json` with the Azure MySQL connection details, and configure Nginx to proxy HTTP traffic from port 80 to the Node.js application on port 8080.

## Evidence

#### Screenshot 9 — Terminal showing the EpicBook repository, successful dependency installation, and the `node_modules` folder

![task 4](screenshots/Screenshot%20with%20Networking%20configuration%20showing%20the%20private%20subnet.png)

---

#### Screenshot 10 — EpicBook Nginx configuration showing `proxy_pass http://127.0.0.1:8080`, or successful `sudo nginx -t` output and active Nginx service status

![task 4](screenshots/Screenshot%20with%20MySQL%20Flexible%20Server%20overview%20showing%20Private%20access%20.png)

---

#### Screenshot 11 — Terminal showing the EpicBook application running and listening on port 8080

![task 4](screenshots/Screenshot%20with%20MySQL%20Client%20output%20showing%20the%20EpicBook%20database.png)

---

# Task 5 — Test End-to-End Functionality

## Goal

Verify that EpicBook loads through the VM’s public IP, displays products, supports cart operations and checkout, and records the corresponding changes in Azure Database for MySQL.

## Evidence

#### Screenshot 12 — Browser showing EpicBook with the Virtual Machine public IP visible in the address bar

![task 5](screenshots/Screenshot%20with%20Browser%20showing%20the%20EpicBook%20application%20.png )

---

#### Screenshot 13 — Proof of a successful cart or order action together with the corresponding database verification

![task 5](screenshots/Screenshot%20with%20Proof%20of%20a%20successful%20database-backed%20action.png)
![task 5](screenshots/Screenshot%20with%20Proof%20of%20a%20successful%20database-backed%20Proof%20.png)

---

#### Public IP URL

Paste your working website URL here:

`http://52.167.1.94:8080/`

---

# Configure PM2 for Application Persistence

## Goal

Run EpicBook in the background using PM2, configure startup after a VM reboot, save the process list, and verify that the application remains accessible through Nginx.

## Evidence

#### Screenshot 14 — Terminal showing the `epicbook` process with status `online` in `pm2 list`

![task 5](screenshots/Screenshot%20with%20Terminal%20showing%20the%20EpicBook%20repository%20cloned%20and%20dependencies%20installed.png)

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post with an EpicBook application screenshot, your website URL, and a caption explaining what you deployed, what you learned, a challenge you resolved, and how you verified application actions in the database.

Include **#DMIByPravinMishra #Azure #DevOps #CloudComputing**. Add the credit note and tags for your track as specified in the assignment guidelines.

## Evidence

#### LinkedIn Post URL

Paste your published LinkedIn post URL here:

`https://lnkd.in/p/eE_QcX63`

---

#### Screenshot 15 — Published LinkedIn post showing your caption, website URL, and EpicBook application screenshot

![linkedIn](screenshots/Screenshot%20with%20epicbook%20LinkedIn%20post.png)

---

# Submission Instructions

- Include your full name and Screenshots **1–15** under the corresponding evidence headings.
- Include the working **Virtual Machine public IP URL** and published **LinkedIn post URL**.
- Add brief notes describing your network, VM, application, and database configuration.
- Include proof of product viewing, cart operations, and order placement.
- Ensure screenshots are readable and show your own environment.
- Do not expose database passwords, private keys, connection strings containing credentials, subscription IDs, or other sensitive information.
- Submit your work using the method specified in the assignment guidelines.

---

# Completion Checklist

<<<<<<< HEAD
- [✅] Task 1: Network foundation created with public/private subnets and NSGs (Screenshots 1–3)
- [✅] Task 2: VM provisioned and required software installed (Screenshots 4–5)
- [✅] Task 3: EpicBook frontend and backend deployed (Screenshots 6–8)
- [✅] Task 4: Private Azure Database for MySQL created and data imported (Screenshots 9–11)
- [✅] Task 5: End-to-end functionality validated (Screenshots 12–13, Public IP URL)
- [✅] No sensitive data exposed
=======
- [✅] Task 1: Resource Group, VNet, subnets, and NSGs created (Screenshots 1–2)
- [✅] Task 2: VM deployed with NIC and Public IP, and required software installed (Screenshots 3–5)
- [✅] Task 3: Private MySQL server configured, `bookstore` created, and schema and seed data imported (Screenshots 6–8)
- [✅] Task 4: EpicBook configured and running behind Nginx (Screenshots 9–11)
- [✅] Task 5: Product viewing, cart operations, checkout, and database changes verified (Screenshots 12–13)
- [✅] PM2 configured, startup command completed, and process list saved (Screenshot 14)
- [✅] Application remains accessible through Nginx while running under PM2
- [✅] LinkedIn post published with the required hashtags, credit note, and tags (Screenshot 15)
- [✅] Full name, working website URL, and LinkedIn post URL included
- [✅] Brief configuration notes included
- [✅] All screenshots readable and no sensitive information exposed
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
