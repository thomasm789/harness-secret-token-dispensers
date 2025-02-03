# GCP OAuth Token Dispenser (Harness Secret Manager)

## Overview
The **GCP OAuth Token Dispenser** is a Harness **Custom Secret Manager** script that generates a **short-lived access token** for **Google Cloud services** using OAuth 2.0.

## Features
- Uses **JWT-based OAuth 2.0 authentication** to obtain an access token.
- Supports **service account authentication** for secure API access.
- Avoids storing **long-lived credentials** in Harness pipelines.
- Enables authentication to **Google Cloud APIs** dynamically.

## Prerequisites
- A **Google Cloud Service Account** with appropriate **IAM roles**.
- Harness should have a **Custom Secret Manager** configured.
- The **private key for the service account** should be available.

## Inputs (Environment Variables)
| Variable Name  | Type   | Description |
|---------------|--------|-------------|
| `client_email` | String | The **email address** of the service account. |
| `private_key`  | String | The **private key** associated with the service account. |
| `token_uri`    | String | The **OAuth 2.0 token endpoint** for GCP authentication. |

## Outputs
| Variable Name | Description |
|--------------|-------------|
| `ACCESS_TOKEN` | The **OAuth access token** for Google Cloud APIs. |

## Usage in Harness
1. **Configure a Custom Secret Manager** in Harness.
2. **Use this script** as an inline script within the Secret Manager.
3. **Set input values** in the secret manager settings.
4. **Reference the output variable** `ACCESS_TOKEN` in your pipeline.

## Example GCP API Usage
Once the **ACCESS_TOKEN** is generated, you can use it for API calls:
```bash
curl -X GET \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Accept: application/json" \
  "https://www.googleapis.com/storage/v1/b"
