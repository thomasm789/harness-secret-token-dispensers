# GitHub App PAT Dispenser (Harness Secret Manager)

## Overview
The **GitHub App PAT Dispenser** is a Harness **Custom Secret Manager** script that generates a **Personal Access Token (PAT)** for a **GitHub App installation**. This allows pipelines to authenticate securely with **GitHub repositories and APIs** without using long-lived credentials.

## Features
- Retrieves a **JWT token** for a GitHub App using its **private key**.
- Exchanges the JWT for an **installation token**, scoped to repositories.
- Supports **secure, short-lived access** for GitHub API calls.
- Avoids storing **static PATs** in Harness secrets.

## Prerequisites
- A **GitHub App** must be created and installed in your organization or repositories.
- The **GitHub App must have permissions** for API access.
- Harness should have a **Custom Secret Manager** configured.

## Inputs (Environment Variables)
| Variable Name            | Type   | Description |
|--------------------------|--------|-------------|
| `installation_id`        | String | The **installation ID** of the GitHub App. |
| `app_id`                | String | The **App ID** assigned to the GitHub App. |
| `github_app_private_key` | String | The **private key** associated with the GitHub App. |

## Outputs
| Variable Name | Description |
|--------------|-------------|
| `PAT`        | The generated **GitHub Personal Access Token (PAT)**, valid for API calls. |

## Usage in Harness
1. **Configure a Custom Secret Manager** in Harness.
2. **Use this script** as an inline script within the Secret Manager.
3. **Set input values** in the secret manager settings.
4. **Reference the output variable** `PAT` in your pipeline.

## Example GitHub API Usage
Once the **PAT** is generated, you can use it for GitHub API calls:
```bash
curl -H "Authorization: token $PAT" https://api.github.com/user
