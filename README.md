# AWS Multi-Account External IP Auditor

A specialized Python utility designed for security teams and network administrators. This tool scans multiple AWS accounts (via AWS Profiles) to discover every public-facing IP address and DNS endpoint in your infrastructure.

It utilizes a "Physics-based" scanning approach (enumerating Network Interfaces) to catch resources that standard API calls often miss, such as NAT Gateways, Load Balancers, and Fargate nodes.

## Table of Contents
- [About](#about)
- [Quick Install Guide](#quick-install-guide)
- [Configuration](#configuration)
- [Usage](#usage)
- [Security](#security)

## About

Most AWS inventory scripts only check for EC2 instances. This leaves massive blind spots.

This tool is different because it scans the **Network Interface (ENI)** layer. If a resource exists in a VPC and has a public IP, it *must* have an ENI. By scanning ENIs, this tool automatically detects:

* **Compute:** EC2, ECS, EKS, Fargate, Batch, Beanstalk.
* **Networking:** NAT Gateways, Classic ELB, ALB, NLB, Global Accelerators.
* **Databases:** Public RDS Instances.
* **Other Services:** Redshift Clusters, OpenSearch Domains, CloudFront Distributions, API Gateways, and Lightsail.

It automatically detects which Regions are enabled in your account to avoid API errors and processes every profile found in your credentials file.

## Quick Install Guide

### Prerequisites
* Python 3.8 or higher
* AWS Credentials configured locally

### Installation Steps

1.  **Clone the repository** (or download the script):
    ```bash
    git clone [https://github.com/YOUR_USERNAME/aws-external-ip-collector](https://github.com/YOUR_USERNAME/aws-external-ip-collector)
    cd aws-external-ip-collector
    ```

2.  **Set up a Virtual Environment** (Recommended to keep your system clean):
    * **Windows:**
        ```powershell
        python -m venv venv
        .\venv\Scripts\activate
        ```
    * **Mac/Linux:**
        ```bash
        python3 -m venv venv
        source venv/bin/activate
        ```

3.  **Install AWS SDK:**
    ```bash
    pip install boto3
    ```

## Configuration

This tool relies on the standard AWS Credentials file. It will automatically loop through **every** profile defined in this file.

### 1. Locate your credentials file
* **Windows:** `C:\Users\YOUR_NAME\.aws\credentials`
* **Mac/Linux:** `~/.aws/credentials`

### 2. Setup your profiles
Open the file in a text editor (Notepad, VS Code, etc.) and add your accounts:

```ini
[default]
aws_access_key_id = AKIA...
aws_secret_access_key = ...

[production]
aws_access_key_id = AKIA...
aws_secret_access_key = ...

[marketing]
aws_access_key_id = AKIA...
aws_secret_access_key = ...# aws-external-ip-collector
