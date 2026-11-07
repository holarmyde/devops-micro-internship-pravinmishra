# Assignment 04 — Deploy Mini Finance on Azure Using Terraform and Ansible

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will provision Azure infrastructure using Terraform and deploy the Mini Finance website using an Ansible multi-play playbook.

Terraform will create the Azure Virtual Machine and networking resources. Ansible will install Nginx, clone the Mini Finance repository, deploy the website, and verify the deployment.

---

# Task 1 — Create the Project Structure

## Goal

Create separate directories and files for the Terraform infrastructure and Ansible configuration.

### Evidence

<<<<<<< HEAD
#### Screenshot 1 — Terminal or editor showing the complete `mini-finance` project tree

![task 1](screenshots/Screenshot%20with%20Terminal%20or%20editor%20showing%20the%20complete%20mini-finance%20project%20tree.png)

---

# Task 2 — Terraform — Azure VM + NSG (Ports 22/80)

## Goal

Provision an Ubuntu 22.04 Standard_B1s VM with a public IP, SSH key authentication, and an NSG allowing SSH (22) and HTTP (80), and output the public IP.

### Evidence

#### Screenshot 2 — Terminal showing the end of a successful `terraform apply`

![task 1](screenshots/Screenshot%20with%20Terminal%20showing%20the%20end%20of%20a%20successful%20terraform%20apply.png)

---

#### Screenshot 3 — Terminal showing `terraform output public_ip`

![task 2](screenshots/Screenshot%20with%20Terminal%20showing%20terraform%20output%20public_ip.png)

---

#### Screenshot 4 — Terraform code or Azure Portal showing NSG inbound rules for ports 22 and 80

![task 2](screenshots/Screenshot%20with%20Azure%20Portal%20showing%20NSG%20rules%20for%20ports%2022%20and%2080.png)

---

# Task 3 — Configure Passwordless SSH

## Goal

Connect to the VM with SSH using the injected key and run `hostname` remotely without a password prompt.

### Evidence

#### Screenshot 5 — Terminal showing the successful passwordless SSH hostname check

![task 3](screenshots/Screenshot%20with%20Terminal%20showing%20the%20successful%20passwordless%20SSH%20hostname%20.png)

---

# Task 4 — Ansible — Multi-Play: Install → Deploy → Verify

## Goal

Create `ansible/inventory.ini` and a three-play `site.yml` that installs Nginx and Git, clones and deploys the Mini Finance repository to `/var/www/html/` with a reload handler, and verifies HTTP 200 from `localhost`.

### Evidence

#### Screenshot 6 — Editor showing `inventory.ini` and the three plays in `site.yml`

![task 4](screenshots/Screenshot%20with%20Editor%20showing%20inventory.ini%20and%20the%20three%20plays.png)

---

#### Screenshot 7 — Terminal showing `ansible-playbook -i inventory.ini site.yml` with HTTP 200, assertion OK, and no failures

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20ansible-playbook%20-i%20inventory.ini%20site_yml.png)

---

# Task 5 — Test End-to-End Functionality

## Goal

Confirm the Mini Finance site is publicly accessible and correctly served by Nginx.

### Evidence

#### Screenshot 8 — Browser showing the Mini Finance site loaded from `http://<public_ip>` with the URL visible
=======
#### Screenshot 1 — Terminal or VS Code showing the complete `mini-finance` project structure
>>>>>>> upstream/main

![task 5](screenshots/Screenshot%20with%20Browser%20showing%20the%20Mini%20Finance%20site%20loaded.png)

---

### Notes

<<<<<<< HEAD
Describe an issue you faced and how you fixed it, and what you learned.

One issue I faced during the Mini Finance deployment was that the Ansible playbook kept hanging when it tried to clone the GitHub repository. I first tested the connection from the Azure VM and confirmed that DNS and HTTPS access to GitHub were working. I then discovered that the repository URL provided in my playbook was not accessible, while the correct repository was https://github.com/pravinmishraaws/mini_finance. I updated the repository URL in site.yml and reran the playbook. The deployment then completed successfully, including synchronizing the application to /var/www/html/, reloading Nginx, and passing the HTTP 200 validation.

This taught me the importance of troubleshooting each layer separately instead of assuming that a deployment failure is caused by Ansible itself. I learned to verify DNS, network connectivity, Git access, and the repository URL independently before changing the playbook. I also learned that small configuration details, such as an incorrect repository URL, can stop an otherwise correctly configured automation workflow
=======
The project was organized into separate Terraform and Ansible directories to maintain a clear separation between infrastructure provisioning and server configuration. Terraform files were created to define the Azure resources, while Ansible files were created to manage the VM configuration, application deployment, and verification.

This structure makes the project easier to maintain, troubleshoot, and reuse.
>>>>>>> upstream/main

---

# Task 2 — Create the Azure Infrastructure Using Terraform

## Goal

Use Terraform to provision an Ubuntu Virtual Machine with the required Azure networking and security resources.

### Evidence

#### Screenshot 2 — Terraform code showing the `Allow-SSH` rule for port `22` and the `Allow-HTTP` rule for port `80`

![task 1](screenshots/Screenshot%20with%20Terminal%20showing%20the%20end%20of%20a%20successful%20terraform%20apply.png)

---

#### Screenshot 3 — Terraform code showing the association between `nsg-mini-finance` and `nic-mini-finance`

![task 2](screenshots/Screenshot%20with%20Terminal%20showing%20terraform%20output%20public_ip.png)

---

### Notes

Terraform was used to provision the Azure infrastructure required to host the Mini Finance website. The configuration included an Ubuntu Linux Virtual Machine, virtual network, subnet, Network Security Group, network interface, and public IP address.

The NSG was configured to allow SSH access through port 22 and public HTTP traffic through port 80. SSH key authentication was configured to provide secure access to the VM.

---

# Task 3 — Initialize and Apply the Terraform Configuration

## Goal

Format and validate the Terraform configuration, review the execution plan, and provision the Azure infrastructure.

### Evidence

#### Screenshot 4 — End of the `terraform apply` output showing `Apply complete!` with no errors

![task 2](screenshots/Screenshot%20with%20Azure%20Portal%20showing%20NSG%20rules%20for%20ports%2022%20and%2080.png)

---

#### Screenshot 5 — Output of `terraform output public_ip` showing the VM’s public IP address

![task 3](screenshots/Screenshot%20with%20Terminal%20showing%20the%20successful%20passwordless%20SSH%20hostname%20.png)

---

### Notes

I initialized Terraform using terraform init, formatted the configuration with terraform fmt, and validated the files using terraform validate.

I reviewed the execution plan with terraform plan before applying the configuration. Terraform successfully provisioned the Azure resources, and I retrieved the VM's public IP address using terraform output public_ip.

This helped me understand how Terraform manages infrastructure through declarative configuration files.

---

# Task 4 — Verify Passwordless SSH Access

## Goal

Confirm that the Ansible controller can connect to the Terraform-provisioned Azure VM using SSH key authentication.

### Evidence

#### Screenshot 6 — Passwordless SSH command and the returned `mini-finance` hostname

![task 4](screenshots/Screenshot%20with%20Editor%20showing%20inventory.ini%20and%20the%20three%20plays.png)

---

### Notes

I verified SSH connectivity between the Ansible controller and the Azure VM using SSH key authentication. The connection was successful without requiring a password prompt.

I also executed the hostname command remotely to confirm that I was connected to the correct VM. This verification ensured that the server was accessible before proceeding with Ansible configuration.

---

# Task 5 — Create the Ansible Inventory and Verify Connectivity

## Goal

Add the Terraform-provisioned Azure VM to the Ansible inventory and confirm that Ansible can connect to it.

### Evidence

#### Screenshot 7 — Ansible ping output showing `SUCCESS` and `pong` from the Azure VM

![task 5](screenshots/Screenshot%20with%20Terminal%20showing%20ansible-playbook%20-i%20inventory.ini%20site_yml.png)

---

### Configuration File

Copy and paste the complete contents of your `ansible/inventory.ini` file below:

```ini
I created an Ansible inventory file named inventory.ini and added the Azure VM under the web host group.

The inventory contained the VM's public IP address, SSH username, private key location, and Python interpreter configuration.

I tested connectivity using the Ansible ping module. The VM returned SUCCESS and pong, confirming that Ansible could communicate with the target server.
```

---

# Task 6 — Create the Multi-Play Ansible Playbook

## Goal

Create one Ansible playbook containing separate plays to install Nginx, deploy the Mini Finance website, and verify the deployment.

### Evidence

#### Screenshot 8 — `site.yml` showing Play 1 and the beginning of Play 2

Screenshot must show:

- Play 1 targeting the `web` group
- Installation of `nginx`, `git`, and `rsync`
- Nginx service configured as started and enabled
- Beginning of Play 2 with the Git repository URL and synchronization task

![task 6](screenshots/Screenshot%20with%20Editor%20showing%20the%20three%20plays%20in%20site.yml.png)

---

#### Screenshot 9 — `site.yml` showing the deployment destination, handler, and Play 3 verification

Screenshot must show:

- Website destination `/var/www/html/`
- Ownership set to `www-data:www-data`
- Nginx reload handler
- Play 3 targeting `localhost`
- The `uri` verification and `assert` condition

![task 6](screenshots/Screenshot%20with%20Editor%20showing%20ansible_site.yml%20with%20the%20three%20roles.png)

---

### Configuration File

Copy and paste the complete contents of your `ansible/site.yml` file below:

```yaml
I created a multi-play Ansible playbook named site.yml with three separate plays.
- Play 1 installed Nginx, Git, and rsync and ensured that Nginx was started and enabled.
- Play 2 cloned the Mini Finance GitHub repository, synchronized the website files to /var/www/html/, and configured a handler to reload Nginx.
- Play 3 verified the deployment using the Ansible uri module and an assertion checking the HTTP response status.
This approach separated installation, deployment, and verification into organized and reusable automation steps.
```

---

# Task 7 — Validate and Run the Ansible Playbook

## Goal

Validate the syntax of the multi-play Ansible playbook and run it to install Nginx, deploy the Mini Finance website, and verify the deployment.

### Evidence

#### Screenshot 10 — Successful playbook syntax check showing `playbook: site.yml`

![task 7](screenshots/Screenshot%20with%20Terminal%20showing%20ansible-playbook%20-i%20inventory.ini%20site_yml.png)

---

#### Screenshot 11 — Play 3 output showing the successful HTTP verification and assertion

![task 7](screenshots/Screenshot%20with%20%20Editor%20showing%20the%20copy%20task%20and%20HTTP%20200%20assertion.png)

---

#### Screenshot 12 — Final `PLAY RECAP` showing `failed=0` and `unreachable=0`

![task 7](screenshots/Screenshot%20with%20Terminal%20showing%20%20Ansible%20run%20and%20final%20recap%20with%20failed=0.png)

---

### Notes

I validated the playbook syntax using ansible-playbook --syntax-check before executing it.

The playbook was then executed against the Azure VM. Ansible successfully installed the required packages, deployed the Mini Finance website, and verified the HTTP response.

The final play recap showed no failed or unreachable hosts, confirming that the automated deployment completed successfully.

---

# Task 8 — Test the Mini Finance Website in a Browser

## Goal

Confirm that the Mini Finance website is publicly accessible through the Azure VM’s public IP address.

### Evidence

#### Screenshot 13 — Mini Finance website successfully loading in the browser, with the Azure VM’s public IP address visible in the address bar

![task 7](screenshots/Screenshot%20with%20Browser%20showing%20the%20Mini%20Finance%20site%20loaded.png)

---

### Website URL

Add your deployed website URL below:

```text
http://20.228.222.157
```

---

# Task 9 — Create the Project README

## Goal

Create a `README.md` file to document the Mini Finance infrastructure and deployment project.

### Evidence

#### Screenshot 14 — Completed `README.md` displayed in the VS Code Markdown preview or terminal

![task 9](screenshots/Screenshot%20with%20Terminal%20or%20editor%20showing%20the%20complete%20mini-finance%20project%20tree.png)

---

### README Content

Copy and paste the complete contents of your `README.md` file below:

```markdown
I created a README.md file documenting the purpose of the project, the Terraform infrastructure, the Ansible deployment process, the required tools, and the verification steps.

The README provides instructions for understanding and reproducing the deployment. It also documents the project structure and the troubleshooting issue encountered during the deployment.
```

---

# LinkedIn Post Required

## Evidence

#### Screenshot 15 — Published LinkedIn post showing the text and at least one deployment screenshot

![task 9](screenshots/Screenshot%20with%20linkedIn%20post_9.png)

---

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://lnkd.in/p/gzGZV9Tb`

---

### LinkedIn Submission Notes

<<<<<<< HEAD
![LinkedIn](screenshots/Screenshot%20with%20linkedIn%20post.png)
=======
**One challenge you faced and how you fixed it:**

One challenge I faced during this assignment was an issue with the Ansible playbook hanging while attempting to clone the Mini Finance GitHub repository.

I initially suspected a network connectivity issue, so I tested DNS resolution and HTTPS connectivity from the Azure VM. After confirming that the connection was working, I discovered that the repository URL configured in my playbook was incorrect.

I corrected the repository URL to https://github.com/pravinmishraaws/mini_finance, reran the Ansible playbook, and successfully completed the deployment.

This experience taught me the importance of troubleshooting systematically by checking network connectivity, repository URLs, and configuration details before making changes to automation scripts.

---

**One real-world example where you can use this learning:**

This learning can be applied in real-world DevOps environments where infrastructure and application deployments need to be automated consistently.

For example, when deploying a company website to multiple Azure Virtual Machines, Terraform can provision the required infrastructure, while Ansible can install Nginx, configure the servers, deploy the application, and verify that the website is accessible.

This approach reduces manual configuration, improves deployment consistency, minimizes human errors, and makes infrastructure easier to scale and maintain.

---

# Assignment Questions

Answer the following in your own words:

**1. What did you provision using Terraform in this assignment?**

I used Terraform to provision the Azure infrastructure required for the Mini Finance website. This included an Ubuntu 22.04 Virtual Machine, Virtual Network, Subnet, Network Interface, Public IP Address, and Network Security Group. The NSG allowed SSH access on port 22 and HTTP traffic on port 80. Terraform helped automate infrastructure creation and ensure consistent resource configuration.

---

**2. What did Ansible configure and deploy in this assignment?**

Ansible installed Nginx, Git, and rsync on the Azure Virtual Machine. It cloned the Mini Finance GitHub repository, synchronized the website files to /var/www/html/, configured the website ownership, ensured that Nginx was running, and verified the deployment using an HTTP request. Ansible automated the server configuration and application deployment process.

---

**3. Why is SSH access on port `22` restricted to your public IP address?**

SSH access on port 22 is restricted to my public IP address to reduce unauthorized access to the Azure Virtual Machine. Restricting SSH traffic prevents other internet users from directly attempting to connect to the server. This follows the principle of least privilege and improves the security of the infrastructure.

---

**4. Why is HTTP port `80` open to the internet?**

HTTP port 80 is open to the internet because the Mini Finance website needs to be publicly accessible through the VM's public IP address. Nginx listens on port 80 and serves the website to users who access it through their browsers. Allowing inbound traffic on this port enables public website access.

---

**5. What is the purpose of the Ansible inventory file?**

The Ansible inventory file identifies the servers that Ansible will manage. It contains information such as the host IP address, host group, SSH username, private key location, and Python interpreter. In this assignment, the inventory file allowed Ansible to connect to the Azure VM in the web group and execute configuration tasks remotely.

---

**6. Why does the playbook use separate plays for install, deploy, and verify?**

The playbook uses separate plays to organize the deployment workflow into clear stages. The installation play prepares the server by installing required packages. The deployment play clones and copies the website files to the Nginx web directory. The verification play checks whether the website is accessible and returns HTTP 200. This separation improves readability, troubleshooting, and maintainability.

---

**7. Why is `rsync` useful when deploying website files?**

Rsync is useful because it synchronizes files and directories between locations efficiently. It can transfer only changed files instead of copying everything repeatedly. This reduces unnecessary file transfers, improves deployment speed, and helps keep the Nginx web directory updated with the latest website files.

---

**8. What does the Ansible `uri` module verify in this assignment?**

The Ansible uri module sends an HTTP request to the deployed Mini Finance website and checks the response. In this assignment, it verifies that the website returns HTTP status code 200, which indicates that the request was successful. The result is then checked using an Ansible assertion to confirm that the deployment is working correctly.

---

**9. What issue did you face during this assignment, and how did you fix it?**

One issue I faced was that the Ansible playbook appeared to hang while cloning the Mini Finance GitHub repository. I tested DNS resolution and HTTPS connectivity from the Azure VM and confirmed that the network connection was working. I then discovered that the repository URL configured in the playbook was incorrect. I updated it to https://github.com/pravinmishraaws/mini_finance and reran the playbook. The deployment completed successfully, and the website passed the HTTP 200 verification.

---

**10. What did you learn from using Terraform and Ansible together?**

I learned that Terraform and Ansible complement each other in infrastructure automation. Terraform provisions the cloud infrastructure, including virtual machines and networking resources, while Ansible configures the servers and deploys applications. Using both tools together creates a more efficient, repeatable, and organized deployment workflow. I also learned the importance of SSH security, infrastructure validation, automation, and end-to-end application testing.

---

# Required Files

Confirm that the following files are included in your assignment folder:

- [✅] `.gitignore`
- [✅] `README.md`
- [✅] `terraform/providers.tf`
- [✅] `terraform/main.tf`
- [✅] `terraform/variables.tf`
- [✅] `terraform/outputs.tf`
- [✅] `ansible/inventory.ini`
- [✅] `ansible/site.yml`
>>>>>>> upstream/main

---

# Submission Instructions

- Add all required screenshots in the correct order.
- Full Name must be visible in required screenshots.
- Add the Azure VM public IP address.
- Add the final Mini Finance website URL.
- Paste `inventory.ini`, `site.yml`, and `README.md` as editable text.
- Answer all assignment questions clearly in your own words.
- Add your LinkedIn post URL.
- Do not expose SSH private keys, passwords, Azure credentials, subscription IDs, Terraform state contents, or other sensitive information.

---

# Completion Checklist

<<<<<<< HEAD
- [✅] Task 1: `mini-finance` project structure created (Screenshot 1)
- [✅] Task 2: Azure VM and NSG provisioned with Terraform (Screenshots 2–4)
- [✅] Task 3: Passwordless SSH verified (Screenshot 5)
- [✅] Task 4: Ansible install/deploy/verify plays run successfully (Screenshots 6–7)
- [✅] Task 5: Site verified in the browser (Screenshot 8)
- [✅] Reflection notes written (Notes)
- [✅] LinkedIn post published and URL submitted
- [✅] No sensitive data exposed
=======
- [✅] Task 1: `mini-finance` project structure created
- [✅] Task 1: `.gitignore` created
- [✅] Task 2: Terraform Azure infrastructure code created
- [✅] Task 2: `Allow-SSH` rule configured for port `22`
- [✅] Task 2: `Allow-HTTP` rule configured for port `80`
- [✅] Task 2: NSG associated with the Network Interface
- [✅] Task 3: `terraform fmt` completed
- [✅] Task 3: `terraform init` completed
- [✅] Task 3: `terraform validate` completed successfully
- [✅] Task 3: `terraform apply` completed successfully
- [✅] Task 3: `terraform output public_ip` displayed the VM public IP
- [✅] Task 4: Passwordless SSH works from the Ansible controller
- [✅] Task 5: `inventory.ini` created
- [✅] Task 5: Ansible ping returns `SUCCESS` and `pong`
- [✅] Task 6: `site.yml` contains three separate plays
- [✅] Task 6: Play 1 installs Nginx, Git, and rsync
- [✅] Task 6: Play 2 clones and deploys the Mini Finance website
- [✅] Task 6: Play 3 verifies HTTP status code `200`
- [✅] Task 7: Playbook syntax check passes
- [✅] Task 7: Ansible playbook completes successfully
- [✅] Task 7: Final recap shows `failed=0` and `unreachable=0`
- [✅] Task 8: Mini Finance website loads in the browser
- [✅] Task 8: Azure VM public IP is visible in the browser screenshot
- [✅] Task 9: `README.md` completed
- [✅] Screenshots 1–15 are included
- [✅] `inventory.ini`, `site.yml`, and `README.md` are pasted as editable text
- [✅] Assignment questions are answered
- [✅] LinkedIn post published with Anyone visibility
- [✅] LinkedIn post URL added
- [✅] No sensitive information is exposed
>>>>>>> upstream/main

---

## About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra and The CloudAdvisory, focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## Resources

- DMI Official Website: [https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme](https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme)
- University: [https://university.pravinmishra.com?utm_source=github&utm_medium=readme](https://university.pravinmishra.com?utm_source=github&utm_medium=readme)
- Discord Community: [https://discord.pravinmishra.com?utm_source=github&utm_medium=readme](https://discord.pravinmishra.com?utm_source=github&utm_medium=readme)
- Blog: [https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme](https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme)
- YouTube Playlist: [https://www.youtube.com/playlist?list=PLFeSNDtI4Cho](https://www.youtube.com/playlist?list=PLFeSNDtI4Cho)
- Pravin Mishra LinkedIn: [https://www.linkedin.com/in/pravin-mishra-aws-trainer/](https://www.linkedin.com/in/pravin-mishra-aws-trainer/)
- CloudAdvisory LinkedIn: [https://www.linkedin.com/company/thecloudadvisory/](https://www.linkedin.com/company/thecloudadvisory/)

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*