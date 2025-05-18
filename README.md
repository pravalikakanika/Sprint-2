# What is Static Code Analysis?
Static Code Analysis involves examining the code without executing it. It helps identify bugs, vulnerabilities, code smells, and compliance issues early in the development lifecycle.


# Why Use Static Code Analysis in CI?

- **Early bug detection before runtime**  
  Identify potential issues before code is executed, reducing the likelihood of runtime errors.

- **Consistent code quality across teams**  
  Enforces uniform coding practices, making codebases easier to maintain and scale.

- **Automated enforcement of code standards**  
  Ensures adherence to guidelines like PEP8 and checks for issues such as high cyclomatic complexity.

- **Faster code reviews**  
  Automates routine checks, allowing reviewers to focus on logic and architecture.

- **Improved security**  
  Detects common vulnerabilities and insecure coding patterns early in the development cycle.

# Tools for Static Code Analysis in Python

| Tool       | Type               | Key Features                                      | Integrates with CI | Maintained |
|------------|--------------------|---------------------------------------------------|---------------------|------------|
| **Flake8** | Linter             | PEP8 enforcement, plugin support                 | ✅ Yes              | ✅ Yes     |
| **Pylint** | Linter & Analyzer  | Code quality scores, bug detection               | ✅ Yes              | ✅ Yes     |
| **Mypy**   | Type Checker       | Static type checking using Python type hints     | ✅ Yes              | ✅ Yes     |
| **Bandit** | Security Scanner   | Finds common Python security issues              | ✅ Yes              | ✅ Yes     |
| **Black**  | Formatter          | Opinionated code formatting                      | ✅ Yes              | ✅ Yes     |
| **SonarQube** | Full Analyzer   | Deep static analysis with dashboard              | ✅ Yes              | ✅ Yes     |


# Tool Comparison

| Feature              | Flake8 | Pylint | Mypy  | Bandit | Black | SonarQube |
|----------------------|--------|--------|-------|--------|-------|-----------|
| **Linting**          | ✅     | ✅     | ❌    | ❌     | ❌    | ✅        |
| **Type Checking**    | ❌     | Partial| ✅    | ❌     | ❌    | ✅        |
| **Code Formatting**  | ❌     | ❌     | ❌    | ❌     | ✅    | ❌        |
| **Security Analysis**| ❌     | ❌     | ❌    | ✅     | ❌    | ✅        |
| **Custom Rules Support** | ✅  | ✅     | ✅    | ✅     | ❌    | ✅        |
| **Report Dashboard** | ❌     | ❌     | ❌    | ❌     | ❌    | ✅        |

# Advantages of Using CI & Static Analysis

- **Improved Code Health**  
  Enforces style guides and checks type consistency.

- **Automated QA**  
  Reduces manual review time and effort.

- **Security**  
  Detects injection flaws, hardcoded credentials, and more.

- **Efficiency**  
  Shortens the feedback loop for developers.

- **Scalability**  
  Maintains quality as team size and codebase grow.

  # Best Practices

- Enforce formatting using **Black** with pre-commit hooks.
- Combine **Flake8** and **Pylint** for style and logical issues.
- Use **Bandit** to check for security vulnerabilities.
- Use **Mypy** for type safety.
- Run these tools in every PR via **GitHub Actions**.
- Add badges for code quality, test coverage, and CI status.

  # Conclusion

After evaluating the tools based on speed, ease of integration, coverage, and maintainability, the recommended toolset is:

- **Black** – for formatting
- **Flake8** – for linting
- **Bandit** – for security analysis
- **Mypy** – for type checking
- **Pytest** – for testing
- **GitHub Actions** – for CI

  #  Contact Information


| Name       | Email Address                |
|------------|------------------------------|
| Pravalika  | kanikarapu.pravalika.snaatak@mygurukulam.co|



##  Reference

| **Link**                                                                 | **Description**                                      |
|--------------------------------------------------------------------------|------------------------------------------------------|
| [Golang installation](https://go.dev/doc/install) | Installation of GoLang  |
| [Docker setup](https://docs.docker.com/) | Documentation for Docker setup  |



# Step-by-Step PoC for Python CI Checks & Static Code Analysis

## Step 1: Create Your Python Project Structure
Example structure:
```
attendance-api/
├── attendance/               # Your Python package folder
│   ├── __init__.py
│   ├── main.py
│   └── utils.py
├── tests/
│   ├── test_main.py
│   └── test_utils.py
├── requirements.txt
├── requirements-dev.txt      # dev dependencies (linters, test tools)
└── .github/
    └── workflows/
        └── python-ci.yml    # GitHub Actions workflow file
```

## Step 2: Prepare requirements-dev.txt
Add dev dependencies for CI checks:
```
flake8
pylint
black
bandit
mypy
pytest
pytest-cov
```
## Step 3: Write Your GitHub Actions Workflow
Create .github/workflows/python-ci.yml with the following:
```yaml
name: Python CI Checks

on:
  push:
    branches:
      - main
      - develop
  pull_request:
    branches:
      - main
      - develop

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Python 3.10
      uses: actions/setup-python@v2
      with:
        python-version: 3.10

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements-dev.txt

    - name: Run Flake8 (Linting)
      run: flake8 .

    - name: Run Pylint (Static Analysis)
      run: pylint attendance/

    - name: Check code formatting with Black
      run: black --check .

    - name: Run Bandit (Security Scan)
      run: bandit -r attendance/

    - name: Run Mypy (Type Checking)
      run: mypy attendance/

    - name: Run Pytest (Unit Tests with Coverage)
      run: pytest --cov=attendance tests/
```

## Step 4: Replace attendance/
In the workflow, replace attendance/ with your actual Python package folder name if different (like notification/).

## Step 5: Commit and Push
```
git add .
git commit -m "Add Python CI checks workflow"
git push origin develop
```
## Step 6: Verify on GitHub

1. Go to your GitHub repository.
2. Open the **Actions** tab.
3. Find the workflow run triggered by your pushed commit or pull request (PR).
4. Review the results for:
   - Linting
   - Formatting
   - Security checks
   - Type checking
   - Tests



