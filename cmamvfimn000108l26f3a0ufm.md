---
title: "AWS CodePipeline with Terraform and Terraform Cloud(TFE) Backend"
seoTitle: "terraform aws codepipeline"
seoDescription: "AWS CodePipeline with Terraform and Terraform Cloud(TFE) Backend"
datePublished: Tue May 13 2025 18:54:41 GMT+0000 (Coordinated Universal Time)
cuid: cmamvfimn000108l26f3a0ufm
slug: aws-codepipeline-with-terraform-and-terraform-cloudtfe-backend
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1747162406436/897fef16-ea1d-4098-bc1e-68dfb5efc21b.png
tags: aws, terraform, codepipeline, terraform-state, terraform-cloud

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1747461924333/28c0323c-f2f5-493e-a008-77e6b13fea28.png align="center")

### 🚀 Setting Up a Real-Time AWS CodePipeline with Terraform and Terraform Cloud (TFE) Backend

In this guide, we’ll walk through setting up a real-time **AWS CodePipeline** that leverages **Terraform** to provision AWS cloud infrastructure such as EC2 instances, using **Terraform Cloud (TFE)** as the remote backend.

---

### 🔧 Prerequisites & Setup Steps

#### 1\. **Create IAM User in AWS**

* Log in to your AWS account.
    
* Create an IAM user with programmatic access and attach necessary permissions (EC2, SSM, CodeBuild, CodePipeline, etc.).
    
* Generate and save the **Access Key ID** and **Secret Access Key**.
    

#### 2\. **Configure Terraform Cloud (TFE)**

* Log in to [Terraform Cloud](https://app.terraform.io).
    
* Create a **Project**, then a **Workspace** under it.
    
* Choose **CLI-Driven Workflow**.
    
* Under **Workspace → Variables**, add the following:
    
    * `AWS_ACCESS_KEY_ID`
        
    * `AWS_SECRET_ACCESS_KEY`
        
    * `AWS_DEFAULT_REGION`
        

#### 3\. **Generate Terraform API Token**

* Click on your user profile icon in Terraform Cloud.
    
* Go to **User Settings → Tokens** and generate a new API token.
    
* Note it down securely.
    

#### 4\. **Store Secrets in AWS SSM Parameter Store**

* Save the following values as **secure string parameters** in AWS Systems Manager (SSM):
    
    * `AWS_ACCESS_KEY_ID`
        
    * `AWS_SECRET_ACCESS_KEY`
        
    * `TFE_API_TOKEN`
        

---

### ⚙️ CodeBuild Setup

#### 5\. **Create AWS CodeBuild Project**

* Set the **Source Provider** as **GitHub**.
    
* Enter your GitHub repository details.
    
* Choose **buildspec** as the build configuration, and provide the file name (e.g., `buildspec.yaml`).
    
* Assign or create a **CodeBuild service role** with the required permissions (SSM, EC2, etc.).
    

---

### 🔄 CodePipeline Setup

#### 6\. **Create AWS CodePipeline**

* Go to the AWS CodePipeline console and create a new **custom pipeline**.
    
* Provide a name and select an appropriate **pipeline service role**.
    
* In the **Source** stage:
    
    * Select **GitHub App** as the provider.
        
    * Enter the repository name and branch (e.g., `main`).
        
* In the **Build** stage:
    
    * Choose **AWS CodeBuild** as the action provider.
        
    * Select the CodeBuild project.
        
    * Set the **input artifacts** to the source output.
        

---

### 📦 GitHub Repository Structure

Your GitHub repo (e.g., [https://github.com/shivaram235vemula/terrafrom\_latest](https://github.com/shivaram235vemula/terrafrom_latest)) should include:

| File Name | Purpose |
| --- | --- |
| `main.tf` | Terraform config for provisioning AWS resources (e.g., EC2). |
| `provider.tf` | Defines AWS provider and configures **Terraform Cloud backend**. |
| `buildspec.yaml` | Commands for CodeBuild (e.g., `terraform init`, `apply`, etc.). |

#### 9\. **Trigger the Pipeline & Verify Deployment**

* Commit and push your Terraform code changes to the GitHub repository.
    
* This triggers the **CodePipeline** automatically.
    
* The pipeline fetches the latest code, runs the **Terraform commands** through CodeBuild, and provisions the defined AWS resources (e.g., EC2 instance).
    
* Navigate to your **Terraform Cloud Workspace**, and under the **"States"** tab, you'll find the generated **Terraform state file**, confirming that the infrastructure has been provisioned successfully.