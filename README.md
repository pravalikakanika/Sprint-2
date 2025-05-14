![image](https://github.com/user-attachments/assets/d65deb28-0a83-43ba-a33e-f5f15dd03475)



|**Author**        | **created on**       | **Version** |**Last edited on**| **Review Level**   | **Reviewer**      |
|---------------|------------|---------|--------|--------|----------------------|
| Pravalika Kanikarapu  | May 13  | v1.1| May 14     | Pre-Reviewer   | Priyanshu            |
| Pravalika Kanikarapu  |  |  |   | L0             | Priyanka     |
| Pravalika Kanikarapu  |      |      |         | L1             | Rishabh Sharma       |
| Pravalika Kanikarapu  |      |      |         | L2             | piyush Upadhyay      |


# Table of Contents

- [Introduction](#introduction)
- [What is Credential Scanning?](#what-is-credential-scanning)
- [Why Credential Scanning?](#why-credential-scanning)
- [Workflow Diagram](#workflow-diagram)
- [Tools for Credential Scanning](#tools-for-credential-scanning)
- [Comparison of Credential Scanning Tools](#comparison-of-credential-scanning-tools)
- [Advantages of Credential Scanning](#advantages-of-credential-scanning)
- [Proof of Concept (POC) – Credential Scanning with GitLeaks in CI](#proof-of-concept-poc--credential-scanning-with-gitleaks-in-ci)
- [Best Practices](#best-practices)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [References](#references)


# Introduction

This document provides a comprehensive guide to implementing credential scanning in CI/CD pipelines using GitLeaks and GitHub Actions. It is intended for developers, DevOps engineers, and security teams who want to prevent accidental exposure of secrets—such as API keys, tokens, and passwords—in code repositories.

The guide covers the importance of credential scanning, compares available tools, outlines a proof of concept with GitLeaks, and offers best practices to strengthen your security posture. 

# What is Credential Scanning?

Credential scanning is an automated technique used in Continuous Integration (CI) pipelines to detect and prevent sensitive information—such as API keys, passwords, tokens, and private keys—from being committed to version control systems. It works by parsing code, configuration files, environment variables, and other text-based files to identify patterns that resemble credentials or secrets.

This helps protect your systems from accidental exposure of sensitive data and supports secure development practices.

# Why Credential Scanning?

Credential scanning plays a critical role in securing your development lifecycle. Here's why it's important:

- **Prevent Data Breaches:** Secrets in code can be easily exploited by attackers if exposed, leading to potential security incidents.
- **Compliance:** Helps meet industry standards and regulations like ISO 27001, SOC 2, GDPR, and HIPAA by ensuring sensitive data isn't exposed.
- **Shift-left Security:** Enables early detection of security issues during development, reducing the cost and effort needed to fix them later.
- **Auditability:** Maintains a clear audit trail for any incidents related to secret leakage, supporting better incident response.
- **Cultural Change:** Promotes secure coding practices and raises awareness about the importance of not hardcoding secrets.

Incorporating credential scanning into your CI/CD pipeline strengthens your security posture and builds trust in your software.

# Workflow Diagram

![image](https://github.com/user-attachments/assets/170d2c0a-78dd-4235-ab61-407d68dddb41)

# Tools for Credential Scanning

Below is a list of popular tools used for scanning credentials and secrets in codebases:

| Tool                | Description                                         | Integration Support         | Licensing              |
|---------------------|-----------------------------------------------------|------------------------------|------------------------|
| **GitLeaks**        | Lightweight secret scanner for git repositories     | CI/CD, Git Hooks             | Open Source            |
| **TruffleHog**      | Searches for high-entropy strings and regex patterns| GitHub Actions, CLI          | Open Source            |
| **Detect Secrets**  | Pluggable and extensible secret scanner             | GitHub, GitLab               | Open Source            |
| **GitGuardian**     | SaaS platform with advanced secret detection        | GitHub, GitLab, CI           | Commercial + Free tier |
| **SpectralOps**     | Developer-first code scanner                        | CI/CD pipelines              | Commercial             |
| **AWS Secrets Detector** | AWS-specific secret detection tool             | AWS pipelines                | Free with AWS          |

Each of these tools can be integrated into your development workflow to catch sensitive information before it’s committed or deployed.

# Comparison of Credential Scanning Tools

The following table compares popular tools based on accuracy, features, and pricing:

| Feature / Tool      | GitLeaks | TruffleHog | GitGuardian | Detect Secrets | SpectralOps |
|---------------------|----------|------------|-------------|----------------|-------------|
| **Accuracy**        | High     | Medium     | Very High   | High           | Very High   |
| **Real-time Alerts**| No       | No         | Yes         | No             | Yes         |
| **SaaS Integration**| Limited  | Limited    | Full        | Limited        | Full        |
| **Entropy Scanning**| Yes      | Yes        | Yes         | Optional       | Yes         |
| **Custom Rules**    | Yes      | Yes        | Yes         | Yes            | Yes         |
| **Pricing**         | Free     | Free       | Free + Paid | Free           | Paid        |

This comparison can help you select the tool that best fits your development and security needs.

# Advantages of Credential Scanning

Implementing credential scanning in your development workflow offers several key benefits:

- **Early Secret Detection**  
  Prevents sensitive information such as API keys and passwords from being committed to public or shared repositories.

- **Automated Enforcement**  
  Integrates into CI/CD pipelines to continuously enforce security policies without manual intervention.

- **Audit Trails**  
  Provides logs and alerts that enable accountability and support incident investigations.

- **Improved Developer Awareness**  
  Encourages secure coding habits and raises awareness about the risks of hardcoding secrets.

- **Cross-Team Collaboration**  
  Brings together security, DevOps, and development teams to proactively address risks related to credential exposure.


# Proof of Concept (POC) – Credential Scanning with GitLeaks in CI
Here’s a simple and effective POC using GitLeaks in a GitHub Actions CI pipeline. This setup scans for secrets every time code is pushed or a pull request is created.

## Objective:
Automatically scan code for hardcoded credentials (e.g., API keys, tokens) during CI execution.

### Step 1: Set Up Your GitHub Repository

1. **Create or Use an Existing Repository**  
   Start by creating a new GitHub repository or use an existing one where you want to enable credential scanning.

2. **Add Sample Code**  
   Ensure the repository contains code or sample configuration files where secrets might accidentally be committed. This will help with testing and validation of the scanning process.

3. **Commit to a Branch**  
   Commit your changes to the `main` branch or any active development branch to trigger the CI/CD pipeline and secret scanning jobs (if already configured).

### Step 2: Add GitHub Actions Workflow

1. **Create the Workflow Directory**  
   In your GitHub repository, navigate to the root folder and create the following directory structure:
   ```bash
   .github/workflows/
   ```

   
2. **Create the Workflow File**  
Inside the `workflows` folder, create a new YAML file. For example:

```bash
secret-scan.yml
```
3. **Paste the following workflow content:**

```yaml
   name: Secret Scan with GitLeaks

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  gitleaks:
    name: Run GitLeaks Secret Scanner
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Run GitLeaks
        uses: gitleaks/gitleaks-action@v2
        with:
          config-path: .gitleaks.toml  # Optional – skip this line if not using a custom config

  ```

### Step 3: Test the Setup
1. Push a test secret (e.g., a fake AWS key) to trigger the scan:

```txt
AWS_SECRET_ACCESS_KEY = "AKIA1234567890FAKEKEY"
```
2. Commit and push your changes:
```bash
git add .
git commit -m "Add test secret"
git push origin main
```
3. Go to the Actions tab in your GitHub repo.

4.  Open the latest run of the Secret Scan with GitLeaks workflow.

### Step 4: Observe the Results in GitHub Actions

#### How to View the Results

1. Go to your repository on GitHub.  
2. Click the **"Actions"** tab.  
3. Select the workflow run named **“Secret Scan with GitLeaks”**.  
4. Click on the job step **“Run GitLeaks”**.  
5. View the logs/output of the GitLeaks run.

   #### ## If a Secret is Detected

- The workflow fails with a red ❌.
- In the logs, you’ll see details like:

```vbnet
Secret detected:
Rule: Generic API Key
File: src/config/settings.py
Line: 45
Secret: abc123apikeyexamplekey
```

- GitHub marks the job as failed, stopping further deployment if you have gates or protections in place.

 #### ## If No Secrets are Detected

- The workflow passes with a green ✔.
- In the logs, you’ll see something like:

```yaml
No secrets were detected. ✅
```

- The job continues, and other CI/CD steps (such as tests, builds, or deployments) proceed as normal.



# Best Practices

To maximize the effectiveness of credential scanning, follow these best practices:

- **Pre-commit Hooks**  
  Enforce secret scanning locally before code is pushed to a repository using tools like `pre-commit`.

- **CI/CD Integration**  
  Incorporate credential scanning into your CI/CD pipelines to ensure automated and consistent enforcement.

- **Developer Training**  
  Educate developers on secure coding practices and the risks of hardcoding sensitive information.

- **Store Secrets Securely**  
  Avoid storing secrets in code. Use secret management tools such as HashiCorp Vault, AWS Secrets Manager, or Azure Key Vault.

# Conclusion
Credential scanning is essential for preventing accidental exposure of secrets in code. By integrating automated scanning into CI pipelines, teams can detect issues early, reduce security risks, and maintain compliance. It’s a simple yet powerful step toward building a secure and responsible DevOps culture.


# Contact Information
| Name       | Email Address                |
|------------|------------------------------|
| Pravalika  | kanikarapu.pravalika.snaatak@mygurukulam.co|

## References
| Links | Description |
|-------|-------------|
| [Cred Scanning](https://www.indusface.com/blog/why-ci-cd-security-scanning-matters/) | Documentation followed from this link  |
| [Git Leaks](https://github.com/gitleaks/gitleaks) | Git leaks doc |
