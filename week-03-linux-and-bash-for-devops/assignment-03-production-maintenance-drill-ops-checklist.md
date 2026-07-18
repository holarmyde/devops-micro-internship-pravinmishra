# Assignment 3 — Production Maintenance Drill (OPS Checklist)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will treat your already deployed React application (on Ubuntu VM with Nginx) as a live production system. You will perform structured operational checks covering network validation, service health, log analysis, resource monitoring, configuration verification, and incident simulation with recovery — mirroring real on-call DevOps responsibilities.

---

# Task 1 — Server Access & Networking Validation

## Goal

Verify that the deployed React application is reachable from the browser and confirm basic network connectivity of the Ubuntu VM.

### Evidence

#### Screenshot 1 — Browser showing the React app with your Full Name visible on the UI

Add your screenshot here.

--- ![task 1](screenshots/Screenshot%20with%20assign%203%20no%201.png)

#### Screenshot 2 — Output of `ip a`

Add your screenshot here.

--- ![task 1](screenshots/Screenshot%20with%20output%20of%20ip%20a.png)

#### Screenshot 3 — Output of `sudo ss -tulpen`

Add your screenshot here.

--- ![task 1](screenshots/Screenshot%20sudo_ss_tulpen%20assign%203%20no%203.png)

#### Screenshot 4 — Output of `sudo ufw status`

Add your screenshot here.

--- ![task 1](screenshots/Screenshot%20with%20sudo%20ufw%20status%20assign%203%20no%204.png)

### Notes

Answer the following in your own words:

**1. What proves Nginx is listening on 0.0.0.0:80?**

Write your answer here.

--- you must verify that the nginx process is successfully bound to TCP port 80 across all IPv4 interfaces. 

**2. What proves SSH is active on port 22?**

Write your answer here.

--- you must ensure that the service process is bound to the port and also that the network firewall allows inbound connections to it

**3. Did you find any unexpected open ports? Explain briefly.**

Write your answer here.

---  No I didnt find any unexpected ports. The ports available is still the SSH which is on port 22 and Nginx which is on port 80. There were some listening services were  systemd-resolved (DNS resolution), chronyd (time sync) also with addresses 127.0.0.53, 127.0.0.54.So the web server and SSH, are externally exposed.


# Task 2 — Service Health & Systemd Validation (Nginx)

## Goal

Verify that Nginx is properly installed, running, enabled at boot, and safely configured.

### Evidence

#### Screenshot 1 — Output of `systemctl status nginx --no-pager`

Add your screenshot here.

--- ![task 2](screenshots/Screenshot%20with%20no%203%20task%202%20screenshot%201.png)

#### Screenshot 2 — Output of `sudo nginx -t`

Add your screenshot here.

--- ![task 2](screenshots/Screenshot%20with%20assign%203%20task%202%20screenshot%203.png)


#### Screenshot 3 — Output of `sudo ss -lptn '( sport = :80 )'`

Add your screenshot here.

--- ![task 2](screenshots/Screenshot%20assign%203%20task%202%20scrn%203.png)

### Notes

Answer the following in your own words:

**1. What happens if Nginx fails to restart in production?**

Write your answer here.

--- The server would instantly drops all incoming HTTP/HTTPS traffic, triggering widespread 504 errors for users. The upstream applications will become completely unreachable until the service is restored.Because the master process terminates when a restart fails, the service stops entirely since Nginx is the only process serving HTTP traffic on port 80.. 

**2. What's your basic rollback plan?**

Write your answer here.

---   run sudo nginx -t first  Before making any configuration change in oreder to validate the config syntax — this catches most errors before they ever reach a restart. If a restart is attempted and fails, the first step is to check systemctl status nginx --no-pager and sudo journalctl -u nginx --no-pager -n 50 to see the exact error.
 However having a backup copy of the working config before making changes is the most secured safeguard, it allows an immediate track back or rollback .



# Task 3 — Logs & Request Trace

## Goal

Verify real traffic flow and analyze logs to understand system behavior and errors.

### Evidence

#### Screenshot 1 — Output of `sudo tail -n 30 /var/log/nginx/access.log`

Add your screenshot here.

--- ![task 3](screenshots/Screenshot%20with%20access_log.png)

#### Screenshot 2 — Output of `sudo tail -n 30 /var/log/nginx/error.log`

Add your screenshot here.

---![task 3](screenshots/Screenshot%20with%20error_log.png)

#### Screenshot 3 — Output of `sudo journalctl -u nginx --no-pager -n 50`

Add your screenshot here.

--- ![task 3](screenshots/Screenshot%20no%20pager%20nginx.png)

### Notes

Answer the following in your own words:

**1. Were there any errors in the logs?**

- If yes, mention 1–2 example error lines from the logs and explain what each one means in simple terms.
- If no, explain what it means if the error log is empty or shows no recent errors during your check.

Write your answer here.

---  The error log showed no output , and the journalctl entries showed clean Started, Stopped, Reloaded, and Deactivated successfully events Hence No errors were found in either the error log and the journalctl output.


**2. If there were no errors, what does that indicate about the system?**

Write your answer here.

--- This shows Nginx has not encountered any internal errors, misconfigurations during the period covered by these logs. This is a positive signal about current system health and it means that nothing went wrong during the window check. Its important to check these logs periodically to avoid issues.


**3. Based on the access logs, were your curl requests visible in the log entries? What does that prove about traffic flow?**

Write your answer here.

---Sure and yes the curl request was visible in access.log. his confirms the full traffic path is working end-to-end: It showed up as GET request from the server's own public IP with a 200 status and the user agent curl/8.18.0. 


# Task 4 — System Resource Health Check (Capacity Red Flags)

## Goal

Assess server capacity and detect potential performance or failure risks.

### Evidence

#### Screenshot 1 — Output of `uptime`

Add your screenshot here.

--- ![task 4](screenshots/Screenshot%20for%20uptime.png)

#### Screenshot 2 — Output of `free -h`

Add your screenshot here.

--- ![task 4](screenshots/Screenshot%20for%20free_h.png)

#### Screenshot 3 — Output of `df -h`

Add your screenshot here.

--- [task 4](screenshots/Screenshot%20for%20df_h.png)

#### Screenshot 4 — Output of `sudo du -sh /var/* | sort -h`

Add your screenshot here.

--- [task 4](screenshots/Screenshot%20for%20var%20sh%20sudo.png)

### Notes

Answer the following in your own words:

**1. Which resource looks most critical right now? (CPU/load, memory, or disk) Explain why.**

Write your answer here.

--- Disk is at a comfortable 65%CPU is idle, memory has healthy available headroom with zero swap pressure hence no resources is showing critical right now

**2. What happens if disk becomes 100% full in a production server?**

Write your answer here.

---  software Applications including build tools can crash if more space than necessary is needed to write temporary files. Even package managers also can fail. If a database were running locally, it could refuse writes. The Operating system could also act up

# Task 5 — Configuration & Deployment Verification

## Goal

Ensure the correct React build is deployed and Nginx is serving it properly.

### Evidence

#### Screenshot 1 — Output of `ls -lah /var/www/html | head -n 20`

Add your screenshot here.

--- [task 5](screenshots/Screenshot%20of%20var_www_html.png)
#### Screenshot 2 — Output of `grep -R "Deployed by" -n /var/www/html 2>/dev/null | head`

Add your screenshot here.

---[task 5](screenshots/Screenshot%20of%20var_www_html____head.png)

#### Screenshot 3 — Output of `grep -n "try_files" /etc/nginx/sites-available/default`

Add your screenshot here.

--- [task 5](screenshots/Screenshot%20with%20try_files%20config%20-%20Copy.png)

### Notes

Answer the following in your own words:

**1. How do you confirm that the correct version of the application is deployed?**

Write your answer here.

--- grep -n searches the Nginx site configuration file for any line containing the string try_files, and the -n flag prefixes the result with its line number 8 showing the file

grep -R "Deployed by" confirmed the specific text was compiled into the live Js set of codes
 and matched the original source code

 ls -lah /var/www/html confirmed the presence of a genuine Create React App production build — index.html, a static/ folder with compiled JS/CSS codes

# Task 6 — Nginx Configuration Failure Simulation

## Goal

Simulate a real-world Nginx misconfiguration and recover the service safely.

### Evidence

#### Screenshot 1 — Output of `sudo nginx -t` showing the syntax error (broken config)

Add your screenshot here.

--- [task 6](screenshots/Screenshot%20with%20config%20error%20on%20try%20files.png)

#### Screenshot 2 — Output of `sudo nginx -t` showing syntax ok (fixed config)

Add your screenshot here.

--- [task 6](screenshots/Screenshot%20with%20sudo%20nginx_t.png)

#### Screenshot 3 — Output of `curl -I http://<public-ip>` confirming recovery (200 OK)

Add your screenshot here.

--- ![task 6](screenshots/Screenshot%20with%20curl%20live%20http.png)

### Notes

Answer the following in your own words:

**1. What caused the configuration failure?**

Write your answer here.

--- What caused the syntax error was that there were  semicolons missing  in /etc/nginx/sites-available/default 

 one was  removed from the try_files $uri /index.html;

and a second one found missing from the error_page 404 /index.html hence   making   Nginx's parser unable to correctly interpret the  block of code


**2. How did you fix the issue?**

Write your answer here.

--- I had to open again the config file and restored both missing semicolons, then re-ran sudo nginx -t to confirm the syntax was valid before restarting the service.

That fixed the as  systemctl restart nginx run, followed by an external curl -I check to confirm the live application was working again


**3. How can you avoid this kind of issue in real production systems?**

Write your answer here.

--- Use a staging environment to test config changes before they ever touch production.

Always run nginx -t after any config edit, before restarting or reloading.

 Nginx config files should always be  in version control (git), so a bad change can be instantly reverted to a known-good state 


# Task 7 — Web Application Failure Simulation

## Goal

Simulate missing deployment content and recover the application safely.

### Evidence

#### Screenshot 1 — Output of `curl -I http://<public-ip>` showing failure (non-200 response)

Add your screenshot here.

---  ![task 7](screenshots/Screenshot%20with%20internal%20error%20nginx%20curl%20http.png)

#### Screenshot 2 — Output of `curl -I http://<public-ip>` confirming recovery (200 OK)

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What caused the application to break in this scenario?**

Write your answer here

---  it returned a 500 Internal Server Error instead of serving the React application. The issue was that The web root directory   /var/www/html which is   the exact path  that   Nginx serves content from  was emptied of all deployed files.

**2. How did you fix the issue and restore the application?**

Write your answer here.

--- I removed the empty broken directory and moved the backup back into place at the correct path. Nginx was restarted to make it was delivered from the restored files. Recovery was confirmed externally via curl -I,  and that showed 200  which meant it was successful/OK 

**3. What steps would you take to prevent this kind of issue in real production systems?**

Write your answer here.

---



Have  backups you deploy , so every release can be instantly rolled back 

 monitoring after you deploy  helps to  verify the live site returns a healthy 200  success  response  immediately after every deploy


# Task 8 — Security & Reliability Review

## Goal

Review and reflect on the security and reliability practices applied during this assignment.

### Security & Reliability Notes

Answer the following in your own words:

**1. Why is SSH key-based authentication more secure than sharing passwords?**

Write your answer here.
 
--- SSH key-based authentication is more secure than sharing passwords because it relies on asymmetric cryptography, meaning the secret credential (the private key) never leaves your local device. Unlike passwords, which must be transmitted to or stored on the server to verify your identity, SSH keys use a challenge-response handshake that proves identity without exposing the secret

**2. Why should only required ports be open on a production server?**

Write your answer here.

--- Leaving only required ports open on a production server minimizes the attack surface, preventing unauthorized access to hidden backdoors or unpatched services. It helps limit opportunities for attackers to establish remote connections

**3. Why is it important for Nginx to be enabled on boot?**

Write your answer here.

--- Enabling Nginx on boot is important  helps  maintains 24/7 uptime for your applications without requiring manual intervention to restart services

  it ensures your web server, load balancer automatically resumes operations following a server restart or unexpected power outage. 
 


**4. What are the risks of sharing secrets, keys, or credentials publicly?**

Write your answer here.

---  Sharing secrets, keys, or credentials publicly hands unauthorized actors the master key to your digital infrastructure. Automated bots constantly scan platforms like GitHub for exposed data, granting cybercriminals the ability to deploy ransomware, steal sensitive user information, and hijack corporate identities in seconds

**5. Why should cloud resources be stopped or terminated when they are no longer needed?**

Write your answer here.

--- . It ensures your infrastructure remains lean, cost-efficient, and aligned with modern operational best practices. It also prevents"cloud sprawl", eliminate unexpected financial waste, and reduce exposure to security vulnerabilities

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

<<<<<<< HEAD
`_https://www.linkedin.com/feed/update/urn:li:activity:7484008587325952002/_`
=======
`https://www.linkedin.com/feed/update/urn:li:activity:7484008587325952002/`
>>>>>>> upstream/main

---

#### Screenshot — Published LinkedIn post

Add your screenshot here.

--- [linkedIn](screenshots/Screenshot%20with%20security.png)

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- Do not expose sensitive information (keys, passwords, account IDs)

---

# Completion Checklist

- [✅ ] Task 1: Screenshots (browser, ip a, ss -tulpen, ufw status) + Notes answered
- [✅ ] Task 2: Screenshots (nginx status, nginx -t, ss port 80) + Notes answered
- [✅ ] Task 3: Screenshots (access log, error log, journalctl) + Notes answered
- [✅ ] Task 4: Screenshots (uptime, free -h, df -h, du -sh) + Notes answered
- [✅ ] Task 5: Screenshots (ls html, grep deployed by, grep try_files) + Notes answered
- [✅ ] Task 6: Screenshots (nginx -t fail, nginx -t pass, curl recovery) + Notes answered
- [✅ ] Task 7: Screenshots (curl failure, curl recovery) + Notes answered
- [✅ ] Task 8: Security & Reliability Notes answered
- [✅ ] LinkedIn post published and URL submitted
- [✅ ] Full Name visible in all required screenshots
- [✅ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://pravinmishra.com/dmi  
- 🎓 DevOps for Beginners (Udemy): https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/  
- 🎓 Agentic AI DevOps with Claude Code: https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/  
- 🎓 DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm: https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*