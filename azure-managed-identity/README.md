# Azure Managed Identity Token Dispenser (Harness Secret Manager)

## Overview
The **Azure Managed Identity Token Dispenser** is a Harness **Custom Secret Manager** script that retrieves an **OAuth 2.0 access token** for accessing **Azure services** using **Managed Identity authentication**. This eliminates the need for storing long-lived credentials.

## Features
- Uses **Azure Instance Metadata Service (IMDS)** for authentication.
- Supports **both system-assigned and user-assigned Managed Identities**.
- Avoids storing **static secrets** by dynamically retrieving a token.
- Enables secure authentication for **Azure API calls**.

## Prerequisites
- The Azure resource (VM, App Service, or Kubernetes pod) **must have a Managed Identity assigned**.
- The **Managed Identity must have the appropriate Azure RBAC permissions** for the requested resource.
- Harness should have a **Custom Secret Manager** configured.

## Inputs (Environment Variables)
| Variable Name  | Type   | Description |
|---------------|--------|-------------|
| `resource`    | String | The **Azure resource** that the token will be used for (e.g., `https://management.azure.com/`). |
| `identity_id` | String | *(Optional)* The **client ID of a user-assigned Managed Identity**. Leave blank for system-assigned identity. |

## Outputs
| Variable Name         | Description |
|----------------------|-------------|
| `AZURE_ACCESS_TOKEN` | The retrieved **OAuth 2.0 access token** for Azure authentication. |

## Usage in Harness
1. **Enable Managed Identity** on your Azure instance.
2. **Assign appropriate RBAC roles** to the identity.
3. **Configure a Custom Secret Manager** in Harness.
4. **Use this script** as an inline script within the Secret Manager.
5. **Set input values** in the secret manager settings.
6. **Reference the output variable** `AZURE_ACCESS_TOKEN` in your pipeline.

## Example Azure API Usage
Once the **AZURE_ACCESS_TOKEN** is generated, you can use it to interact with Azure services:
```bash
curl -X GET \
  -H "Authorization: Bearer $AZURE_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  "https://management.azure.com/subscriptions?api-version=2021-04-01"
```

## Security Considerations
- The **Azure token is short-lived** and should be refreshed periodically.
- **Ensure the Managed Identity has least-privilege permissions** in Azure RBAC.
- Requests to `http://169.254.169.254/metadata/identity/oauth2/token` **must be made from an Azure-hosted environment**.

## Troubleshooting
- **Ensure that a Managed Identity is assigned** to the Azure instance.
- Verify that the **resource value** matches the intended Azure service.
- Check the **IAM role assignments** for the Managed Identity.
- Run the following to confirm identity metadata:
  ```bash
  curl -H "Metadata: true" "http://169.254.169.254/metadata/instance?api-version=2021-02-01"
  ```
