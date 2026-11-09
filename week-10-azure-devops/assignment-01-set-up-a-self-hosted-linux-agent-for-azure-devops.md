# Assignment 1 — Configure a Self-Hosted Azure DevOps Agent on Ubuntu

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will provision an Ubuntu VM in AWS or Azure and configure it as a self-hosted Azure Pipelines agent. You will create an agent pool, register the agent using a Personal Access Token (PAT), run it as a Linux system service, and verify it by executing a test pipeline on the VM.

---

# Task 0 — Create or Access Azure DevOps

## Goal

Sign in to Azure DevOps and create or access an organization and project for the assignment.

No submission screenshot is required for this task.

---

# Task 1 — Create a Personal Access Token (PAT)

## Goal

Create and securely store the PAT required to register the self-hosted agent.

No submission screenshot is required for this task.

> Do not include the PAT in this document or in any screenshot.

---

# Task 2 — Create a Self-Hosted Agent Pool

## Goal

Create the Azure DevOps agent pool that will contain the Linux agent.

<<<<<<< HEAD
### Evidence

#### Screenshot 1 — Azure DevOps Agent Pools page showing the newly created pool

![task 2](screenshots/Screenshot%20with%20Azure%20DevOps%20Agent%20Pools%20page%20showing%20the%20newly%20created%20pool.png)
=======
![task 2](screenshots/Screenshot%20with%20Azure%20DevOps%20Agent%20Pools%20page%20showing%20the%20newly%20created%20pool.png)

>>>>>>> upstream/main
---

# Task 3 — Provision and Connect to the Ubuntu VM

## Goal

Provision an Ubuntu VM in AWS or Azure and verify its operating system, architecture, and outbound connection to Azure DevOps.

## Evidence

### Screenshot 1 — Ubuntu VM Running

Add a screenshot from AWS or Azure showing:

* Ubuntu VM name
* VM status as **Running**
* Public IP address

![task 3](screenshots/Screenshot%20with%20cloud%20console%20and%20its%20public%20IP%20or%20DNS%20name.png)

---

### Screenshot 2 — Ubuntu, Architecture, and HTTPS Verification

Add an SSH terminal screenshot showing the output of:

* `cat /etc/os-release`
* `uname -m`
* `curl -I https://dev.azure.com`

The screenshot must confirm a supported Ubuntu version, `x86_64` architecture, and a successful HTTP response from Azure DevOps.

![task 3](screenshots/Screenshot%20with%20Terminal%20showing%20a%20successful%20SSH%20login%20and%20Ubuntu%20version%20details.png)

---

# Task 4 — Install and Configure the Azure Pipelines Agent

## Goal

Download, configure, and register the Linux Azure Pipelines agent, and run it as a system service.

## Evidence

### Screenshot 3 — Agent Configuration and Service Status

Add a terminal screenshot showing:

* Successful agent configuration
* Agent service installation
* Agent service start
* `sudo ./svc.sh status` reporting that the service is running

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20successful%20agent%20configuration%20without%20exposing%20the%20PAT.png)

<<<<<<< HEAD
---

#### Screenshot 5 — Terminal showing the agent service running successfully

![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20the%20agent%20service%20running%20successfully.png)
=======
![task 4](screenshots/Screenshot%20with%20Terminal%20showing%20the%20agent%20service%20running%20successfully.png)
>>>>>>> upstream/main

---

# Task 5 — Verify That the Agent Is Online

## Goal

Confirm that the agent service is running and the agent appears online in Azure DevOps.

## Evidence

### Screenshot 4 — Agent Online in Azure DevOps

Add a screenshot of the Azure DevOps Agent Pool **Agents** page showing:

* Selected agent pool
* Selected agent name
* Agent status as **Online**
* Agent enabled and available

![task 5](screenshots/Screenshot%20with%20Pipeline%20online.png)

---

# Task 6 — Create and Run a Test Pipeline

## Goal

Create an Azure DevOps YAML pipeline and verify that its commands execute on the self-hosted Ubuntu VM.

## Evidence

### Screenshot 5 — Azure Pipelines YAML

Add a screenshot of `azure-pipelines.yml` open in the Azure Repos editor showing:

* `trigger: none`
* Selected self-hosted agent pool
* Bash verification step
* Your Full Name
* Linux verification commands

![task 6](screenshots/Screenshot%20with%20verify%20test.png)

---

### Screenshot 6 — Successful Test Pipeline

Add a screenshot of the successful Azure DevOps pipeline run showing:

<<<<<<< HEAD
Cloud Platform and Azure DevOps Details
Cloud platform: AWS EC2
Azure DevOps organization: bebotchild
Azure DevOps project: SelfHostedAgentProject
Agent pool: Default
Self-hosted agent: SelfHostedAgentProject
Issue Faced and Resolution

One issue I faced was getting SSH access to the Ubuntu EC2 instance. The correct AWS private key initially returned Permission denied (publickey) because the corresponding public key was not present in the Ubuntu user's authorized_keys file. I used AWS EC2 Instance Connect to temporarily access the VM, added the correct public key to ~/.ssh/authorized_keys, corrected the SSH file permissions, and then successfully connected using the original key.

I also encountered an authentication mistake while configuring the Azure DevOps agent, where a PAT was entered at the wrong prompt. I revoked that exposed PAT, created a replacement PAT, and completed the agent configuration successfully.
=======
* Overall status as **Succeeded**
* Expanded **Verify self-hosted Ubuntu agent** step
* `Submitted by: <your-full-name>`
* Agent name
* Machine name
* Output from `uname -a`
* Output from `whoami`
* Output from `df -h`
* Output from `pwd`

![task 6](screenshots/Screenshot%20with%20The%20pipeline%20run%20succeeding%20after%20your%20fix.png)

---

## Completed azure-pipelines.yml

Paste the contents of your completed `azure-pipelines.yml` file below.

```yaml

trigger:
  branches:
    include:
      - main

pr: none

variables:
  - group: EpicBook-Production-Variables

pool:
  vmImage: ubuntu-latest

stages:
  - stage: TerraformPlan
    displayName: Terraform validation and plan
    jobs:
      - job: ValidateAndPlan
        displayName: Validate EpicBook infrastructure
        steps:
          - checkout: self

          - task: AzureCLI@2
            displayName: Validate and create reviewed plan
            inputs:
              azureSubscription: sc-epicbook-azure
              scriptType: bash
              scriptLocation: inlineScript
              workingDirectory: $(Build.SourcesDirectory)/terraform
              inlineScript: |
                set -euo pipefail

                sudo apt-get update -qq
                sudo apt-get install -y -qq curl unzip jq

                VERSION="1.16.1"
                curl -fsSLo /tmp/terraform.zip \
                  "https://releases.hashicorp.com/terraform/${VERSION}/terraform_${VERSION}_linux_amd64.zip"
                unzip -o /tmp/terraform.zip -d /tmp/terraform-bin
                sudo install /tmp/terraform-bin/terraform /usr/local/bin/terraform

                terraform version
                terraform fmt -check -recursive
                terraform init -input=false -reconfigure
                terraform validate

                KEY_FILE="$(Agent.TempDirectory)/epicbook_id_ed25519.pub"
                printf '%s\n' "$EPICBOOK_SSH_PUBLIC_KEY" > "$KEY_FILE"
                chmod 600 "$KEY_FILE"
                export TF_VAR_ssh_public_key_path="$KEY_FILE"

                PLAN_FILE="$(Agent.TempDirectory)/epicbook.tfplan"
                PLAN_JSON="$(Agent.TempDirectory)/epicbook-plan.json"

                terraform plan \
                  -input=false \
                  -lock-timeout=60s \
                  -out="$PLAN_FILE"

                terraform show -json "$PLAN_FILE" > "$PLAN_JSON"

                # Block deletions and actual changes to security rules or associations.
                if ! jq -e '
                  [
                    .resource_changes[]?
                    | select(.change.actions != ["no-op"])
                    | select(
                        (.change.actions | index("delete")) != null
                        or
                        (
                          (
                            .type
                            | test(
                                "network_security_rule|network_interface_security_group_association|subnet_network_security_group_association"
                              )
                          )
                          and
                          (.change.before != .change.after)
                        )
                      )
                  ] | length == 0
                ' "$PLAN_JSON" >/dev/null; then
                  echo "SAFETY CHECK FAILED: review deletions or actual network security rule/association changes."
                  exit 1
                fi

                mkdir -p "$(Build.ArtifactStagingDirectory)"
                cp "$PLAN_FILE" "$(Build.ArtifactStagingDirectory)/epicbook.tfplan"
                echo "Plan passed the configured safety checks."

            env:
              EPICBOOK_SSH_PUBLIC_KEY: $(sshPublicKey)
              TF_VAR_admin_source_cidr: $(adminSourceCidr)
              TF_VAR_admin_source_cidrs: $(adminSourceCidrs)
              TF_VAR_mysql_admin_password: $(mysqlAdminPassword)

          - task: PublishPipelineArtifact@1
            displayName: Publish reviewed Terraform plan
            inputs:
              targetPath: $(Build.ArtifactStagingDirectory)/epicbook.tfplan
              artifact: epicbook-reviewed-plan

  - stage: Approval
    displayName: Manual plan approval
    dependsOn: TerraformPlan
    condition: succeeded()
    jobs:
      - job: ManualApproval
        displayName: Review before applying
        pool: server
        timeoutInMinutes: 1500
        steps:
          - task: ManualValidation@1
            timeoutInMinutes: 1440
            inputs:
              notifyUsers: ''
              instructions: |
                Review the TerraformPlan stage logs and the reviewed plan artifact.
                Confirm every proposed resource change is expected.
                Reject this run if any change is unclear or unsafe.
                Approve only after the plan has been reviewed.
              onTimeout: reject

  - stage: TerraformApply
    displayName: Apply approved Terraform plan
    dependsOn: Approval
    condition: succeeded()
    jobs:
      - job: ApplyReviewedPlan
        displayName: Apply exact saved plan
        steps:
          - checkout: self

          - task: DownloadPipelineArtifact@2
            displayName: Download reviewed plan
            inputs:
              artifact: epicbook-reviewed-plan
              path: $(Pipeline.Workspace)/epicbook-reviewed-plan

          - task: AzureCLI@2
            displayName: Initialize and apply reviewed plan
            inputs:
              azureSubscription: sc-epicbook-azure
              scriptType: bash
              scriptLocation: inlineScript
              workingDirectory: $(Build.SourcesDirectory)/terraform
              inlineScript: |
                set -euo pipefail

                sudo apt-get update -qq
                sudo apt-get install -y -qq curl unzip

                VERSION="1.16.1"
                curl -fsSLo /tmp/terraform.zip \
                  "https://releases.hashicorp.com/terraform/${VERSION}/terraform_${VERSION}_linux_amd64.zip"
                unzip -o /tmp/terraform.zip -d /tmp/terraform-bin
                sudo install /tmp/terraform-bin/terraform /usr/local/bin/terraform

                terraform init -input=false -reconfigure

                PLAN_FILE="$(Pipeline.Workspace)/epicbook-reviewed-plan/epicbook.tfplan"
                test -f "$PLAN_FILE" || {
                  echo "ERROR: Reviewed plan artifact is missing."
                  exit 1
                }

                terraform apply -input=false "$PLAN_FILE"
                terraform output

            env:
              EPICBOOK_SSH_PUBLIC_KEY: $(sshPublicKey)
              TF_VAR_admin_source_cidr: $(adminSourceCidr)
              TF_VAR_admin_source_cidrs: $(adminSourceCidrs)
              TF_VAR_mysql_admin_password: $(mysqlAdminPassword)
```

> Do not include your PAT, SSH private key, password, or cloud credentials in the YAML file.

---

# Assignment Summary

Write a short summary of what you configured.

### Assignment Summary

In this assignment, I worked on configuring a self-hosted Azure DevOps agent using an Ubuntu Linux environment.

I learned how to create an Azure DevOps organization and project, generate a Personal Access Token, configure an agent pool, and register a Linux machine as a self-hosted agent.

I also explored SSH connectivity, Linux system administration, agent service management, and Azure DevOps pipeline configuration.

Finally, I created a YAML test pipeline to verify the Linux environment and demonstrate how Azure DevOps executes pipeline jobs using a self-hosted agent.

This assignment improved my practical understanding of CI/CD automation, Linux administration, cloud infrastructure, and DevOps engineering.

---

# LinkedIn Requirement (If Applicable)

## Screenshot 7 — LinkedInisor填Token Belle

Add a screenshot of your LinkedIn post showing:

* What you configured
* Why organizations use self-hosted agents
* Three to five lines explaining your experience
* A screenshot of the successful pipeline run with no secrets visible

![LinkedIn](screenshots/successful%20pipeline%20LinkedIn%20post.png)

**LinkedIn Post URL:** https://www.linkedin.com/posts/oluwafemi-aremu-a85ab4197_devops-azuredevops-linux-ugcPost-7525618422974644224-STf3/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAC5CRAUBiD9hmJ-VjKBPuiy5dtrdA6AJQDk
>>>>>>> upstream/main

---

# Submission Instructions

* Include the short assignment summary.
* Include Screenshots 1–6.
* Include the contents of your completed `azure-pipelines.yml` file.
* Include Screenshot 7 and the LinkedIn post URL if the LinkedIn requirement applies.
* Do not expose a PAT, SSH private key, password, account details, or another secret.

---

# Completion Checklist

<<<<<<< HEAD
- [✅] Task 1: PAT created with required scopes and stored securely
- [✅] Task 2: Self-hosted agent pool created (Screenshot 1)
- [✅] Task 3: Ubuntu VM provisioned and SSH verified (Screenshots 2–3)
- [✅] Task 4: Agent installed, registered, and running as a service (Screenshots 4–5)
- [✅] Task 5: Agent verified Online (Screenshot 6)
- [✅] Task 6: Test pipeline run successfully (Screenshot 7)
- [✅] Platform/org/pool details and issue notes written (Notes)
- [✅] No secrets exposed
=======
* Azure DevOps organization and project are ready
* A supported Ubuntu LTS VM is running and accessible through SSH
* SSH access is restricted to your public IP address
* Outbound HTTPS connectivity is working
* PAT was created with the required scopes and stored securely
* A self-hosted agent pool was created
* The same pool name was used during registration and in the pipeline YAML
* The agent service is running
* The agent appears **Online** in Azure DevOps
* The pipeline targets the selected agent pool
* The pipeline run completed with **Succeeded** status
* The pipeline output displays your Full Name
* Screenshots 1–6 are included and readable
* Screenshot 7 and the LinkedIn post URL are included if applicable
* The completed `azure-pipelines.yml` content is included
* No PAT, SSH private key, password, or other secret is visible
>>>>>>> upstream/main

---

*This submission is part of the DevOps Micro Internship (DMI) — Agentic AI Track.*
