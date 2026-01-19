# API Tests: Katalon TestOps Integration

This project is a CI/CD automation example focused on interacting with the **Katalon TestOps API**. It uses the [Insomnia CLI (Inso)](https://developer.konghq.com) and [GitHub Actions](https://docs.github.com) to run automated API regression tests.

For the sake of simplicity and demonstration, this implementation specifically focuses on using **GET requests** to retrieve data from various TestOps endpoints (e.g., fetching teams, projects, test results).

The workflow checks out the repository, installs the `inso` CLI, injects necessary secrets from the GitHub repository settings, and executes the requests defined in the `TestOps_API_Collection.yaml` file.

## Getting Started

To use this project in your GitHub repository, you will need to configure two main things:

1.  GitHub Repository Secrets
2.  The GitHub Actions Workflow File

### 1. Configure GitHub Secrets

This project requires sensitive information (like your TestOps base URL, API keys, and specific resource IDs) to be stored securely in your GitHub repository's settings. These are used by the CI pipeline at runtime.

Navigate to your repository's **Settings** > **Secrets and variables** > **Actions** > **New repository secret** and add the following secrets:

| Secret Name        | Description                                       |
| ------------------ | ------------------------------------------------- |
| `BASE_URL`         | The base URL of the Katalon TestOps API (e.g., `https://testops.katalon.io/api/v1`) |
| `USERNAME`         | Your Katalon API Key (used as a Basic Auth username) |
| `PASSWORD`         | A placeholder for basic auth (can be anything if using API key as username) |
| `TEAM_ID`          | Target Team ID for API endpoints                  |
| `PROJECT_ID`       | Target Project ID for API endpoints               |
| `TEST_SUITE_ID`    | Target Test Suite ID                              |
| `EXECUTION_ID`     | Target Execution/Test Run ID                      |
| *... (and any others you use)* | |

### 2. The Workflow File (`.github/workflows/ci.yaml`)

The automation logic is defined in the `ci.yaml` file. The provided configuration performs the following steps on every `push` or `pull_request` to the `main` branch:

*   **Checkout Code:** Clones the repository.
*   **Setup Inso CLI:** installs the required version of the Insomnia CLI.
*   **Run API Collection:** Executes the **GET requests** defined in `TestOps_API_Collection.yaml`, injecting the secrets defined above into the `MY_ENVIRONMENT` in the Insomnia context.

You can view the full working configuration in the provided [`ci.yaml`](.github/workflows/ci.yaml) file in this repo.

## Exported Insomnia Data

The `TestOps_API_Collection.yaml` file contains the exported request definitions and environment structure from the Insomnia application. These requests utilize templating (e.g., `{{ _.BASE_URL }}`) which are resolved by the secrets at runtime.

NOTE: This file should only contain structure and placeholder variables (e.g., `""`), never actual secrets. The secrets are injected securely via GitHub Actions at runtime.