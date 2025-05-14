

![image](https://github.com/user-attachments/assets/fabe8487-d70c-4dba-b9d6-ddcc1139b444)



|**Author**        | **created on**       | **Version** |**Last edited on**| **Review Level**   | **Reviewer**      |
|---------------|------------|---------|--------|--------|----------------------|
| Pravalika Kanikarapu  | May 12   | v1.0|   May 13  | Pre-Reviewer   | Priyanshu            |
| Pravalika Kanikarapu  |  |  |   | L0             | Priyanka     |
| Pravalika Kanikarapu  |      |      |         | L1             | Rishabh Sharma       |
| Pravalika Kanikarapu  |      |      |         | L2             | piyush Upadhyay      |

# Table of Contents

- [What is DAST?](#what-is-dast)
- [Why Use DAST in React CI/CD?](#why-use-dast-in-react-cicd)
- [DAST Workflow in React CI/CD](#dast-workflow-in-react-cicd)
- [Tools Overview](#tools-overview)
- [Tool Comparison Table](#tool-comparison-table)
- [Advantages of DAST for React CI](#advantages-of-dast-for-react-ci)
- [Proof of Concept (PoC)](#proof-of-concept-poc)
  - [Pre-requisites](#pre-requisites)
  - [Steps to Set Up PoC](#steps-to-set-up-poc)
  - [Explanation of Workflow Steps](#explanation-of-workflow-steps)
  - [Sample Output](#sample-output)
- [Best Practices](#best-practices)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [References](#references)



# What is DAST?

DAST is a black-box testing approach that interacts with your app like an external attacker would—scanning it for vulnerabilities such as:

- Cross-Site Scripting (XSS)
- Broken authentication
- API security flaws
- Security misconfigurations

It does not require access to source code and works on a deployed/staging instance.

# Why Use DAST in React CI/CD?

| Reason                       | Benefit                                                                 |
|-----------------------------|-------------------------------------------------------------------------|
| Production-like Testing      | Tests the real deployed app, not code or build artifacts               |
| API Interaction Coverage     | Captures issues in frontend-backend communication                      |
| Realistic Vulnerability Detection | Uncovers exploitable flaws missed by SAST/SCA                  |
| Continuous Security          | Automates scanning at each deployment stage                            |
| Compliance & Audit           | Helps meet security standards (OWASP Top 10, SOC 2, etc.)              |

# DAST Workflow in React CI/CD

```plaintext
[ Developer Commits Code ]
            ↓
[ Build and Unit Tests ]
            ↓
[ Deploy React App to Staging ]
            ↓
[ Run DAST Tool (e.g., OWASP ZAP) ]
            ↓
[ Generate Security Report ]
            ↓
[ If Critical Issue → Block Deployment ]
            ↓
[ Developer Fixes Issues ]
            ↓
[ Redeploy → Re-test ]
            ↓
[ Deploy to Production ]
```
![image](https://github.com/user-attachments/assets/fab8623e-65b9-4f20-9587-c19984b0fddf)



1. Developer commits code to Git
2. CI builds the React app
3. Unit/Integration tests run
4. App is deployed to a staging environment
5. DAST tool (e.g., OWASP ZAP) scans the staging URL
6. Vulnerability report is generated
7. CI pipeline checks for critical issues
8. If clean → deploy to production

# Tools Overview

| Tool         | Open Source | CI/CD Support | API Coverage | Auth Scanning | Active Maintenance |
|--------------|-------------|----------------|--------------|---------------|--------------------|
| OWASP ZAP    | ✅           | ✅              | ✅            | ✅             | ✅                  |
| Burp Suite   | ❌ (Pro)     | Limited        | ✅            | ✅             | ✅                  |
| Arachni      | ✅ (inactive)| ❌              | Limited      | ❌             | ❌                  |
| w3af         | ✅           | ❌              | Limited      | ❌             | Low                |

# Tool Comparison Table

| Feature               | OWASP ZAP   | Burp Suite (Pro) | Arachni   | w3af     |
|-----------------------|-------------|------------------|-----------|----------|
| Open Source           | ✅           | ❌               | ✅         | ✅        |
| React-Friendly        | ✅           | ✅               | ✅         | ❌        |
| Maintained            | ✅           | ✅               | ❌         | ⚠️       |
| CLI/CI Integration    | ✅           | ❌ (manual)      | ❌         | ❌        |
| Auth/API Testing      | ✅           | ✅               | ⚠️         | ❌        |

# Advantages of DAST for React CI

- **Works post-build, before production**: DAST tests the application after it has been built, allowing you to identify vulnerabilities before deployment to production.
- **Finds real-world exploit paths**: DAST simulates real-world attacks, helping uncover vulnerabilities that can be exploited in a live environment.
- **No access to source code needed**: Since DAST is a black-box testing approach, it doesn’t require access to the source code, making it suitable for testing deployed applications.
- **Protects dynamic content and APIs**: DAST can detect vulnerabilities in dynamic content and APIs, which are often difficult to test with static analysis alone.
- **Fits easily into CI workflows**: DAST integrates well with popular CI tools such as GitHub Actions, Jenkins, and GitLab, enabling automated security scans as part of your development pipeline.


# Proof of Concept (PoC)

**Scenario**: GitHub Actions with OWASP ZAP DAST Scan

This PoC demonstrates how to integrate OWASP ZAP into a GitHub Actions CI pipeline for continuous DAST scanning of a React application deployed to a staging environment.

## Pre-requisites

- **React Application**: A deployed React application on a staging server  
  _Example_: `https://staging.example-react.com`

- **GitHub Actions**: GitHub Actions must be set up and enabled for your React project repository.

- **OWASP ZAP Docker Image**: The OWASP ZAP GitHub Action uses the official OWASP ZAP Docker image, which is automatically pulled and run as part of the scan process.


## Steps to Set Up PoC

Create a GitHub Actions workflow file: `.github/workflows/security.yml`

```yaml
name: React CI with DAST (OWASP ZAP)

on:
  push:
    branches:
      - main
      - staging

jobs:
  security-scan:
    runs-on: ubuntu-latest

    steps:
    # Step 1: Checkout Code
    - name: Checkout code
      uses: actions/checkout@v2

    # Step 2: Install Dependencies and Build React App (Optional)
    - name: Install Dependencies and Build
      run: |
        npm install
        npm run build

    # Step 3: Deploy React App to Staging
    - name: Deploy to Staging
      run: ./deploy-staging.sh  # Replace with your staging deployment script

    # Step 4: Run OWASP ZAP DAST Scan
    - name: Run OWASP ZAP DAST Scan
      uses: zaproxy/action-full-scan@v0.4.0
      with:
        target: 'https://staging.example-react.com'  # Replace with your staging URL
        cmd_options: '-config scanner.attackStrength=HIGH'  # Set scan strength

    # Step 5: Upload ZAP Report
    - name: Upload ZAP Report
      uses: actions/upload-artifact@v2
      with:
        name: zap-security-report
        path: zap-report.html  # Adjust based on your configuration

    # Step 6: Check for Critical Issues
    - name: Check for Critical Issues
      run: |
        if grep -q "High" zap-report.html; then
          echo "Critical vulnerabilities found!";
          exit 1;
        fi
 ```
## Explanation of Workflow Steps

- **Checkout Code**: The workflow first checks out the latest code from the repository using the GitHub Actions `checkout` action.

- **Install Dependencies and Build**: Installs all required dependencies and builds the React application. This step is optional but useful if your deployment requires a build artifact.

- **Deploy to Staging**: The app is deployed to a staging environment. Replace `./deploy-staging.sh` with your own deployment script suited for your infrastructure.

- **Run OWASP ZAP DAST Scan**: OWASP ZAP performs a dynamic security scan against the deployed staging URL. The scan strength is configured as `HIGH` to increase vulnerability detection.

- **Upload ZAP Report**: After the scan completes, the resulting report (`zap-report.html`) is uploaded as an artifact, making it accessible for manual or automated review.

- **Check for Critical Issues**: This step parses the ZAP report for any "High" severity findings. If found, the pipeline is failed intentionally to block further deployment to production until issues are resolved.


## Sample Output

After running the GitHub Actions workflow, you will see the following output:

### ZAP Scan Output

- The OWASP ZAP scan will generate a detailed report highlighting vulnerabilities such as XSS, SQL Injection, and other security issues.
- The report will be available in both **HTML** and **JSON** formats for review and integration into reporting systems.

### Build Status

- If critical issues are detected, the build will **fail**, and the CI/CD pipeline will prevent deployment to production.
- The output will display a message like:

```plaintext
Critical vulnerabilities found in the scan report!
```

# Best Practices

| Best Practice                                                             | Description                                                                 |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| **Run scans on staging, not production**                                  | Always run DAST scans on your staging environment rather than production to avoid affecting real users and systems. |
| **Use environment-specific configs for auth/API keys**                    | Ensure that authentication and API keys are environment-specific to avoid exposing sensitive data in scans. |
| **Scan behind login flows (authenticated DAST)**                          | Perform authenticated DAST scans to test vulnerabilities that can only be accessed after logging in, simulating real attacker behavior. |
| **Automate high-severity issue reporting (e.g., Jira integration)**       | Automate the reporting of critical vulnerabilities to issue tracking systems like Jira, ensuring quick action and resolution. |
| **Combine with SAST and SCA tools for full security coverage**            | Use DAST in conjunction with Static Application Security Testing (SAST) and Software Composition Analysis (SCA) tools to provide comprehensive security coverage across the development pipeline. |


# Conclusion

After evaluating all tools and aligning with React’s deployment model, the recommended DAST tool is:

 **OWASP ZAP**

### Why?

- **100% open source and community-backed**: OWASP ZAP is fully open-source and supported by a large community, ensuring continuous updates and improvements.
- **Native CI/CD support (especially GitHub Actions)**: It integrates seamlessly with CI/CD workflows, particularly with tools like GitHub Actions, making it easy to automate security testing.
- **Excellent support for React APIs and login flows**: ZAP has robust support for modern web technologies, including React APIs and authentication flows, ensuring comprehensive vulnerability coverage.
- **Configurable for headless scanning and automation**: OWASP ZAP can be configured to run in headless mode, ideal for automated scans in CI/CD pipelines without requiring a UI.

This tool provides the best balance of automation, flexibility, and React compatibility for securing CI pipelines.

# Contact Information
| Name       | Email Address                |
|------------|------------------------------|
| Pravalika  | kanikarapu.pravalika.snaatak@mygurukulam.co|

# References
| **Link** | **Description** |
|------------------------------------------------------|------------------|
| [DAST in CI/CD](https://circleci.com/blog/dynamic-application-security-testing-dast/)| Documentation followed from this link      |
