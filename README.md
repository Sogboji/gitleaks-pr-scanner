# Gitleaks PR Secret Scanner

This repository automatically scans pull requests for secrets and sensitive data using [Gitleaks](https://github.com/gitleaks/gitleaks) via GitHub Actions.

## Overview

Every time a pull request is opened or updated against the `main` branch, a GitHub Actions workflow will run a Gitleaks scan to ensure that no secrets (like API keys, tokens, passwords) are committed to the repository.

---

## Project Step-by-step Instruction

### 1.  Install Gitleaks via Homebrew (Recommended for Mac)
#### Open Terminal in VSCode or use Mac Terminal:

brew install gitleaks

#### Verify the installation:
gitleaks version

**You should see something like:**
Gitleaks version: v8.x.x

### 2. Create the GitHub Repository (manually)
#### Go to GitHub and:

Click "New Repository"

Choose a name for your repo like gitleaks-pr-scanner

Initialize with a README (optional)

Clone it to your local machine: 
git clone https://github.com/your-username/gitleaks-pr-scanner.git
cd gitleaks-pr-scanner

### 3. Add the GitHub Actions Workflow

Create a workflow directory if it doesn't already exist:

```bash
mkdir -p .github/workflows
```

Then create a file named `.github/workflows/gitleaks.yml` and add the following:

```yaml
name: Gitleaks PR Scan

on:
  pull_request:
    branches: [ main ]

jobs:
  gitleaks-scan:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run Gitleaks scan
        uses: zricethezav/gitleaks-action@v2
        with:
          config-path: .github/gitleaks.toml
```

This configuration ensures that a scan runs on every pull request targeting the `main` branch.

---

### 4. (Optional) Add a Custom Gitleaks Config

If you'd like to use custom rules or exclusions, create a `.github/gitleaks.toml` file.

```bash
touch .github/gitleaks.toml
```

You can find the default config file [here](https://github.com/gitleaks/gitleaks/blob/master/config/gitleaks.toml) and customize it as needed.

Make sure to reference it in the workflow using the `config-path` as shown above.

---

### 5. Commit and Push Your Changes

```bash
git add .
git commit -m "Add Gitleaks GitHub Actions workflow"
git push origin main
```

---

## 🧪 How to Test It

### Step 1: Create a new test branch

```bash
git checkout -b test-leak
```

### Step 2: Add a file containing a fake secret

```bash
echo "AWS_SECRET_ACCESS_KEY=FAKEKEY123456" > secret.txt
git add secret.txt
git commit -m "Test Gitleaks with fake secret"
git push origin test-leak
```

### Step 3: Open a Pull Request

Open a PR to `main` on GitHub. Gitleaks will run in CI and detect the fake secret.

>  The workflow will fail and prevent the PR from being merged until resolved.

---

##  Security Best Practices

- No API keys, tokens, or credentials are stored in code.
- Gitleaks is run entirely inside GitHub Actions, with no need for local setup or third-party CI tools.
- All scans are automatic and reproducible per pull request.

---

##  Tools Used

- [Gitleaks](https://github.com/gitleaks/gitleaks) — A fast and flexible tool for detecting secrets.
- [GitHub Actions](https://github.com/features/actions) — CI/CD pipeline for automation.

---

##  Project Structure

```
gitleaks-pr-scanner/
├── .github/
│   ├── workflows/
│   │   └── gitleaks.yml       # GitHub Actions Workflow
│   └── gitleaks.toml          # Optional custom rules for Gitleaks
├── README.md
└── secret.txt                 # (Used for testing only)
```

---

##  Maintainer

This project is maintained by [@Sogboji](https://github.com/Sogboji).  
Feel free to fork, modify, or contribute via pull requests.

---

## Stay Secure. Shift Left.

Scan early. Scan often. Catch secrets before they leak 
```
