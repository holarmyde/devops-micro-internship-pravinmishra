# Assignment 1 — Create an Azure Virtual Machine using Terraform

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will use Terraform to provision a complete Azure Virtual Machine environment, including a resource group, virtual network, subnet, public IP, network interface, and a Linux-based virtual machine. You will set up and verify the required local tools, define the infrastructure in Terraform, initialize the project, review and apply the plan, verify the running VM through Azure CLI, capture the public IP output, and destroy the resources after testing.

---

# Task 0 — Set Up and Verify the Terraform and Azure CLI Environment

## Goal

Prepare your local environment for Terraform deployment by installing Terraform, Azure CLI, and the HashiCorp Terraform extension in VS Code, signing in to your Azure account, and confirming that all required tools are working correctly.

### Evidence

#### Screenshot 1 — Terminal showing successful `terraform version` output

Add your screenshot here.

---

#### Screenshot 2 — Terminal showing successful `az version` output

Add your screenshot here.

---

#### Screenshot 3 — VS Code Extensions panel showing the HashiCorp Terraform extension installed and enabled

Add your screenshot here.

---

# Task 1 — Create a New Terraform Project and Define the Infrastructure

## Goal

Create a new Terraform project and define the complete Azure Virtual Machine environment in `main.tf` by using the official Terraform Registry documentation.

### Evidence

#### Screenshot 4 — VS Code showing the AzureRM provider configuration and resource group configuration in `main.tf`

![task 1](screenshots/Screenshot%20with%20VS%20Code%20showing%20main_tf.png)

---

#### Screenshot 5 — VS Code showing the Linux virtual machine configuration and public IP `output` block in `main.tf`. Ensure that the VM password is hidden or redacted

![task 1](screenshots/Screenshot%20with%20main_tf%20showing%20the%20public%20IP%20output%20and%20VM%20authentication%20.png)

---

# Task 2 — Initialize Terraform

## Goal

Initialize the Terraform working directory and download the required provider components.

### Evidence

#### Screenshot 6 — Terminal showing the successful `terraform init` output

![task 2](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20terraform%20init%20output.png)

---

# Task 3 — Plan and Apply the Configuration

## Goal

Review the Terraform execution plan and provision the Azure resources.

### Evidence

#### Screenshot 7 — Terraform plan summary showing the proposed resources

![task 3](screenshots/Screenshot%20with%20Terraform%20plan%20summary%20showing%20the%20proposed%20resources.png)

---

#### Screenshot 8 — Terraform apply output showing successful completion

![task 3](screenshots/Screenshot%20with%20Terraform%20output%20showing%20the%20public%20IP%20of%20the%20VM.png)

---

#### Screenshot 9 — Terraform output showing the public IP address of the VM

![task 3](screenshots/Screenshot%20with%20Terraform%20output%20showing%20the%20public%20IP%20of%20the%20VM_final.png)

### Question

VM Public IP Address: [Enter the public IP shown by terraform output]

---

# Task 4 — Verify the Deployment

## Goal

Confirm through Azure CLI that the virtual machine was created successfully and is currently running.

### Evidence

#### Screenshot 10 — Azure CLI output showing the deployed VM name and `VM running` status

![task 4](screenshots/Screenshot%20with%20Azure%20CLI%20output%20showing%20the%20VM%20name%20and%20running%20status.png)

---

# Task 5 — Destroy the Resources

## Goal

Remove all Azure resources created by Terraform after completing the deployment and verification.

### Evidence

#### Screenshot 11 — Terminal showing successful `terraform destroy` completion

![task 5](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20terraform%20destroy%20completion.png)

---


### Notes

I learned that Azure VM availability depends on the specific subscription and region. In East US 2, the original Standard_D2s_v5 and Standard_B2s VM sizes were unavailable due to subscription/capacity restrictions. The SKU list showed that some newer D-series sizes, such as Standard_D2s_v7, are available, but availability does not necessarily mean they are free-tier eligible. This highlighted the need to check the specific free-tier B-series sizes before choosing a VM size for the Terraform deployment.



---


# Submission Instructions

- Complete all tasks in sequence and include all required screenshots specified in Tasks 0–5.
- Do not expose passwords, keys, account IDs, or other sensitive information in screenshots.

---

# Completion Checklist

<<<<<<< HEAD
- [✅] Task 1: `terraform-azure-vm` project created with all required resources defined (Screenshots 1–2)
- [✅] Task 2: `terraform init` completed successfully (Screenshot 3)
- [✅] Task 3: Plan reviewed and `terraform apply` completed, public IP recorded (Screenshots 4–6)
- [✅] Task 4: VM verified as running via Azure CLI (Screenshot 7)
- [✅] Task 5: `terraform destroy` completed successfully (Screenshot 8)
- [✅] Learning/issues paragraph written (Notes)
- [✅] No sensitive information exposed

- Installed Terraform and verified it using `terraform version`
- Installed Azure CLI and verified it using `az version`
- Signed in to Azure using `az login`
- Confirmed the correct Azure subscription
- Installed and enabled the HashiCorp Terraform extension in VS Code
- Created the `terraform-azure-vm` project directory and `main.tf`
- Added the Terraform and AzureRM provider configuration
- Defined the resource group, virtual network, subnet, public IP, and network interface
- Defined the Linux virtual machine with username and password-based authentication
- Added the Terraform output for the VM public IP address
- Completed `terraform init` successfully
- Reviewed the Terraform execution plan using `terraform plan`
- Completed `terraform apply` successfully
- Captured and recorded the VM public IP using `terraform output`
- Verified that the VM is running using Azure CLI
- Completed `terraform destroy` successfully
- Captured all required screenshots
- Checked that no passwords, keys, account IDs, or other sensitive information are visible in the screenshots
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