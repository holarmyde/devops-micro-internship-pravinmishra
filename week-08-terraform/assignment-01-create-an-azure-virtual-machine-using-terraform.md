# Assignment 1 — Create an Azure Virtual Machine using Terraform

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will use Terraform to provision a complete Azure Virtual Machine environment: a resource group, virtual network, subnet, public IP, network interface, and an Ubuntu 18.04 Linux VM. You will initialize, plan, and apply the configuration, verify the running VM via Azure CLI, and destroy the resources after testing.

---

# Task 1 — Create a New Terraform Project and Define the Infrastructure

## Goal

Create a `terraform-azure-vm` project and define the resource group, virtual network, subnet, public IP, network interface, and Ubuntu 18.04 VM (with username/password authentication and a public IP output) in `main.tf`.

### Evidence

#### Screenshot 1 — VS Code showing `main.tf` and the required Azure resources

![task 1](screenshots/Screenshot%20with%20VS%20Code%20showing%20main_tf.png)

---

#### Screenshot 2 — `main.tf` showing the public IP output and VM authentication configuration, with the password hidden or redacted

![task 1](screenshots/Screenshot%20with%20main_tf%20showing%20the%20public%20IP%20output%20and%20VM%20authentication%20.png)

---

# Task 2 — Initialize Terraform

## Goal

Run `terraform init` and confirm the working directory initializes successfully.

### Evidence

#### Screenshot 3 — Terminal showing successful `terraform init` output

![task 2](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20terraform%20init%20output.png)

---

# Task 3 — Plan and Apply the Configuration

## Goal

Review `terraform plan`, run `terraform apply`, and record the VM's public IP from the Terraform output.

### Evidence

#### Screenshot 4 — Terraform plan summary showing the proposed resources

![task 3](screenshots/Screenshot%20with%20Terraform%20plan%20summary%20showing%20the%20proposed%20resources.png)

---

#### Screenshot 5 — Terraform apply output showing successful completion

![task 3](screenshots/Screenshot%20with%20Terraform%20output%20showing%20the%20public%20IP%20of%20the%20VM.png)

---

#### Screenshot 6 — Terraform output showing the public IP of the VM

![task 3](screenshots/Screenshot%20with%20Terraform%20output%20showing%20the%20public%20IP%20of%20the%20VM_final.png)

---

# Task 4 — Verify the Deployment

## Goal

Use Azure CLI to confirm the VM was created and is running.

### Evidence

#### Screenshot 7 — Azure CLI output showing the VM name and running status

![task 4](screenshots/Screenshot%20with%20Azure%20CLI%20output%20showing%20the%20VM%20name%20and%20running%20status.png)

---

# Task 5 — Destroy the Resources

## Goal

Run `terraform destroy` to clean up the Azure resources after testing.

### Evidence

#### Screenshot 8 — Terminal showing successful `terraform destroy` completion

![task 5](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20terraform%20destroy%20completion.png)

---

### Notes

Write a short paragraph explaining what you learned or any issues you encountered.

I learned that Azure VM availability depends on the specific subscription and region. In East US 2, the original Standard_D2s_v5 and Standard_B2s VM sizes were unavailable due to subscription/capacity restrictions. The SKU list showed that some newer D-series sizes, such as Standard_D2s_v7, are available, but availability does not necessarily mean they are free-tier eligible. This highlighted the need to check the specific free-tier B-series sizes before choosing a VM size for the Terraform deployment.

---

# Submission Instructions

- Add all required screenshots in your submission
- Include the VM public IP from the Terraform output
- Do not expose Azure credentials, subscription details, or passwords

---

# Completion Checklist

- [✅] Task 1: `terraform-azure-vm` project created with all required resources defined (Screenshots 1–2)
- [✅] Task 2: `terraform init` completed successfully (Screenshot 3)
- [✅] Task 3: Plan reviewed and `terraform apply` completed, public IP recorded (Screenshots 4–6)
- [✅] Task 4: VM verified as running via Azure CLI (Screenshot 7)
- [✅] Task 5: `terraform destroy` completed successfully (Screenshot 8)
- [✅] Learning/issues paragraph written (Notes)
- [✅] No sensitive information exposed

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

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
