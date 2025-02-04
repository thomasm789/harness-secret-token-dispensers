# GitLab Personal Access Token (PAT) Dispenser (Harness Secret Manager)

## Overview
The **GitLab PAT Dispenser** is a Harness **Custom Secret Manager** script that generates a **short-lived Personal Access Token (PAT)** for programmatic access to **GitLab repositories and APIs**.

## Features
- Automates **GitLab OAuth authentication** to generate a **short-lived PAT**.
- Supports **API access, repository management, and CI/CD pipeline integration**.
- Avoids storing **static long-lived PATs** in Harness pipelines.
- Enhances **security and compliance** by using ephemeral tokens.

## Prerequisites
- A **GitLab instance** (GitLab.com or self-hosted GitLab).
- A **GitLab OAuth application** with required API permissions.
- Harness should have a **Custom Secret Manager** configured.

## Inputs (Environment Variables)
| Variable Name  | Type   | Description |
|---------------|--------|-------------|
| `gitlab_url`   | String | The **GitLab instance URL** (e.g., `https://gitlab.com`). |
| `app_id`       | String | The **GitLab OAuth Application ID**. |
| `client_secret`| String | The **GitLab OAuth Client Secret**. |
| `username`     | String | The **GitLab username** requesting a PAT. |
| `password`     | String | The **GitLab user password or access token**. |
| `scopes`       | String | The **OAuth scopes** required for the token (e.g., `read_api`, `write_repository`). |

## Outputs
| Variable Name | Description |
|--------------|-------------|
| `GITLAB_PAT`  | The generated **GitLab Personal Access Token (PAT)**. |

## Usage in Harness
1. **Configure a Custom Secret Manager** in Harness.
2. **Use this script** as an inline script within the Secret Manager.
3. **Set input values** in the secret manager settings.
4. **Reference the output variable** `GITLAB_PAT` in your pipeline.

## Example GitLab API Usage
Once the **GITLAB_PAT** is generated, you can use it for GitLab API calls:
```bash
curl -X GET \
  -H "Authorization: Bearer $GITLAB_PAT" \
  -H "Accept: application/json" \
  "$gitlab_url/api/v4/projects"
```

## Security Considerations
- The **GITLAB_PAT is short-lived** and should not be stored persistently.
- Use **GitLab OAuth best practices** to minimize exposure risks.
- Ensure that only **necessary permissions** are granted.

## Troubleshooting
- Ensure the **GitLab OAuth app** is properly configured.
- Verify that the **app_id, client_secret, and scopes** are correct.
- Check if the **GitLab instance allows API authentication via PATs**.
