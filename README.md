# static-code-analysis

Automated static code review using **SonarCloud**, triggered via GitHub Actions on every pull request and push.
Part of a bachelor thesis comparing LLM, static, and hybrid code review quality.

## How it works

```
Pull Request / Push
       │
       ▼
GitHub Actions workflow (.github/workflows/sonarcloud.yml)
       │
       └─ Runs SonarCloud static analysis on the repository
              │
              ├─ Posts inline review comments on the pull request  (PR events only)
              └─ Shows findings directly in the SonarCloud dashboard
```

## Setup

### 1. Connect the repository to SonarCloud

1. Go to [sonarcloud.io](https://sonarcloud.io) and log in with your GitHub account.
2. Click **+** → **Analyze new project** and select this repository.
3. Choose **GitHub Actions** as the analysis method.
4. Note down the **Organisation key** and **Project key** shown on screen.

### 2. Add the SonarCloud token as a repository secret

1. Go to **Settings → Secrets and variables → Actions**.
2. Click **New repository secret**.
3. Name: `SONAR_TOKEN`  
   Value: the SonarCloud token generated in step 1.

### 3. Update `sonar-project.properties`

Edit `sonar-project.properties` and set your `sonar.organization` and
`sonar.projectKey` to the values from step 1.

### 4. Required permissions

The workflow requests these permissions automatically:

| Permission | Reason |
|---|---|
| `contents: read` | Check out the repository |
| `pull-requests: write` | Post review comments on the pull request |

Make sure **Actions → General → Workflow permissions** in your repository settings
is set to *Read and write permissions* (or at minimum allows the write scope above).

## Repository structure

```
.
├── .github/
│   └── workflows/
│       └── sonarcloud.yml            # GitHub Actions workflow
├── sonar-project.properties          # SonarCloud project configuration
└── README.md
```