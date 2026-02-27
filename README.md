
## Overview

The Migration Accelerator is an enterprise-grade web application designed to streamline and secure the migration of source code repositories from Team Foundation Server (TFS) to GitHub. It provides comprehensive assessment capabilities, automated secret detection, and detailed reporting to ensure safe and efficient migrations. 


## Key Features

**Infrastructure as Code (IaC) Automation**: Seamlessly create and manage Terraform-based infrastructure configurations for automated cloud deployments

**Secrets Management Integration**: Secure credential management through AWS Secrets Manager with automatic injection into application pipeline

**Repository Security Scanning**: Automated secret detection and vulnerability scanning using Gitleaks across GitHub repositories

**GitHub Integration**: Direct integration with GitHub API for repository analysis, configuration retrieval, and automated workflows

**Multi-Environment Support**: Segregate migration configurations across Development, Test, Staging, and Production environments

**Background Processing**: Asynchronous Terraform execution and scheduled updates through hosted background services

**Centralized Dashboard**: Web-based interface for viewing, managing, and orchestrating migration workflows

## Use Cases

**Accelerated Cloud Migrations**: Reduce migration timelines through automated infrastructure provisioning and configuration management

**Infrastructure Standardization**: Enforce consistent infrastructure patterns across teams and environments using Infrastructure as Code

**Secrets Governance**: Centralized credential management with audit trails and automated rotation capabilities

**Security Compliance**: Automated scanning for exposed secrets and compliance violations before deployment

**DevOps Automation**: Streamline deployment pipelines with integrated Terraform execution and GitHub workflows

**Multi-Cloud Strategy**: Support complex migrations across multiple cloud environments with consistent tooling

## Architecture

<img width="348" height="183" alt="image" src="https://github.com/user-attachments/assets/55f97dcb-ff2d-41ac-86e5-bddccebe2236" />





The platform consists of four core components:

### 1. User Interface
ASP.NET Core MVC application serving as the primary user interface for configuration management, testing, and migration orchestration. Built on .NET 8.0 with modern web standards.
The entry point of the application.

Purpose:

- Allows users to interact with the system

- Collects inputs (forms, clicks, filters)

- Sends HTTP requests to the backend

### 2. Service Layer (Services Directory)
Business logic implementation for GitHub integration, AWS Secrets management, Terraform execution, and security scanning using established design patterns.

### 3. Data Persistence (Data Directory & Models)
Entity Framework Core integration with SQL Server for storing migration configurations, audit logs, and system state across environment lifecycles.

### 4. Background Scheduler (TerraformBackgroundService)
Asynchronous processing engine for long-running Terraform executions and automated configuration synchronization across environments.

###  5. Controller

The request handler and traffic manager.

Purpose: Receives HTTP request from UI Validates input Calls Service Layer Returns appropriate response

## Technical Requirements

<img width="562" height="250" alt="image" src="https://github.com/user-attachments/assets/6ef5f14f-9c73-49e5-b13b-822e470a3786" />



### System Requirements
- **.NET Runtime**: .NET 8.0 or higher
- **GitLeaks**: GitLeaks 8.30.0 for Windows 
- **Operating System**: Windows/Linux with .NET support
- **Memory**: Minimum 2GB RAM recommended
- **Storage**: Sufficient space for Terraform state files and logs

### Cloud Requirements
- **AWS Account**: For Secrets Manager access
- **GitHub Account**: For repository integration
- **Terraform CLI**: Version 1.0+ for infrastructure execution

### Dependencies
- Entity Framework Core 8.0.0
- Microsoft.EntityFrameworkCore.SqlServer 8.0.0
- AWS SDK for .NET (AWSSDK.SecretsManager 3.7.1.16)
- SecretsScanner.Gitleaks 1.0.1
- Newtonsoft.Json 13.0.4
- System.IO.Compression.ZipFile 4.3.0

## 🔹 Required Tools & Accounts Overview

| Component        | Version / Requirement        | Why We Are Using It |
|-----------------|-----------------------------|---------------------|
| .NET Runtime    | .NET 8.0 or higher          | Provides the execution environment for building and running modern applications (MVC, ASP.NET Core, Web API, Blazor). |
| GitLeaks        | GitLeaks 8.30.0 (Windows)   | Used via a batch script to scan repositories for hardcoded secrets (API keys, connection strings, tokens)  |
| AWS Account     | With Secrets Manager access | Stores and manages application secrets securely. Prevents hardcoding sensitive data in source code and enables centralized, encrypted secret management. |
| Terraform CLI   | Version 1.0+                | Used to execute Terraform scripts to provision infrastructure and securely store secrets in AWS Secrets Manager. |



## Installation

### 1  GitLeaks Integration Setup Guide

This application integrates with GitLeaks to scan repositories for sensitive data before migration.

 ### 1. Download GitLeaks

Visit the official GitLeaks releases page:
https://github.com/gitleaks/gitleaks/releases

Download:

gitleaks_8.30.0_windows_x64.zip


Extract the contents to:

C:\Tools\


Verify installation:

gitleaks version


### 2. 1️⃣.Clone Repository from GitHub

Repository URL:

https://github.com/rajkumarvaluemomentum/migration_accelerator


Clone using Git:

git clone https://github.com/rajkumarvaluemomentum/migration_accelerator.git
cd migration_accelerator

### 2️⃣ Directory Structure

Ensure the following structure exists on your system:

🔹 Tools Directory
- C:\Tools\
- │
- ├── gitleaks.exe
- ├── scan-repo.bat
- └── repositories\

### 🔹 Application Result Storage

- migration_accelerator
- └── wwwroot
    - └── scan-results


Make sure the scan-results folder exists inside wwwroot.

 ### 3️⃣ Batch File Configuration (scan-repo.bat)

Create a file named:

- C:\Tools\scan-repo.bat


Add the following content:

@echo off
set repoName=%1

echo Scanning repository: %repoName%

gitleaks detect ^
--source "C:\Tools\repositories\%repoName%" ^
--report-format json ^
--report-path "C:\Path\To\Your\Project\wwwroot\scan-results\%repoName%.json"

echo Scan completed.
pause


⚠ Important:
Update the --report-path value to match your actual project path.

Example:

- --report-path "C:\YourProjectPath\wwwroot\scan-results\%repoName%.json"

### 4️⃣ C# Configuration

Inside your service or controller, configure the paths like this:

// Path to GitLeaks batch script
- private readonly string _batFile = @"C:\Tools\scan-repo.bat";

// Folder where scan results are stored
- private readonly string _resultFolder = @"C:\Users\JadiRajKumar\OneDrive - ValueMomentum, Inc\Desktop\TFS_TO_GITGUB_MIGRATION_2026\migration_accelerator-master\migration_accelerator-master\migration_accelerator\wwwroot\scan-results";



### 📘 Application Walkthrough

This section provides a step-by-step guide to using the Migration Accelerator application.

### ▶ 1️⃣ Launch the Application

Open the solution file:

migration_accelerator.sln


Build and run the application (Press F5 or click Start in Visual Studio).

The home screen will appear as shown below:

<img width="958" height="380" alt="Home Screen" src="https://github.com/user-attachments/assets/c50e9113-135c-4c7f-9d94-a80d2a38e953" />

This is the main dashboard where you can initiate repository scanning.

###  2️⃣ Scan a Repository

- To scan a repository for sensitive data:

- Enter the Repository Path.

- Enter the Repository Name.

- Click the Scan Now button.

Example screen:

<img width="953" height="356" alt="Scan Screen" src="https://github.com/user-attachments/assets/bbc32007-219b-45bd-92a3-8ae25b2f9b52" />
What Happens Behind the Scenes?

The application triggers the configured scan-repo.bat.

GitLeaks scans the repository.

A JSON report is generated in wwwroot/scan-results.

The repository name is added to the dropdown list.

###  3️⃣ View Scan Results

After the scan completes:

Select the scanned repository from the dropdown list.

The application will automatically load detected secrets.

<img width="930" height="421" alt="Scan Results" src="https://github.com/user-attachments/assets/041777fe-a115-4197-8b1c-ae04440db374" />
Results Displayed:

Secret Type (Key)

Masked Secret Value

File Path

Position Details (if applicable)

Secrets are displayed as key-value pairs with masking applied for security.

###  Security Behavior

- Full secret values are never displayed.

- Secrets are masked before rendering.

- JSON reports are read and mapped to structured models.

- If no secrets are found, a success message is displayed.

###  End-to-End Flow
- User Inputs Repository Details
        ↓
- Scan Now Clicked
        ↓
- Batch Script Executes GitLeaks
        ↓
- JSON Report Generated
        ↓
- Application Loads Results
        ↓
- Secrets Displayed in UI

### Expected Outcome

After following the above steps, you will be able to:

- Identify hardcoded secrets

- Review sensitive configuration issues

- Prepare repositories for secure migration

- Ensure compliance before GitHub migration

<img width="930" height="346" alt="image" src="https://github.com/user-attachments/assets/21160c12-e559-4ef6-88b9-58e2b2123913" />





   
