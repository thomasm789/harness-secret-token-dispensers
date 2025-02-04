# AWS STS Temporary Credentials Dispenser (Harness Secret Manager)

## Overview
The **AWS STS Temporary Credentials Dispenser** is a Harness **Custom Secret Manager** script that retrieves **temporary AWS IAM credentials** using the **AWS Security Token Service (STS)**. This enables secure, short-lived access to AWS resources without storing long-lived credentials.

## Features
- Uses **AWS STS AssumeRole API** to generate temporary credentials.
- Provides **Access Key, Secret Key, and Session Token**.
- Supports **configurable session duration** for flexibility.
- Avoids storing long-lived AWS credentials in Harness pipelines.

## Prerequisites
- An **AWS IAM Role** with appropriate permissions for **STS AssumeRole**.
- Harness should have a **Custom Secret Manager** configured.
- AWS CLI must be installed on the delegate running this script.

## Inputs (Environment Variables)
| Variable Name      | Type   | Description |
|--------------------|--------|-------------|
| `role_arn`        | String | The **AWS IAM Role ARN** to assume. |
| `session_name`    | String | A unique **session name** for tracking access. |
| `session_duration`| String | The duration (in seconds) for which the credentials remain valid. |

## Outputs
| Variable Name         | Description |
|----------------------|-------------|
| `AWS_ACCESS_KEY_ID`  | The temporary AWS access key. |
| `AWS_SECRET_ACCESS_KEY` | The temporary AWS secret key. |
| `AWS_SESSION_TOKEN`  | The temporary AWS session token. |

## Usage in Harness
1. **Configure a Custom Secret Manager** in Harness.
2. **Use this script** as an inline script within the Secret Manager.
3. **Set input values** in the secret manager settings.
4. **Reference the output variables** (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`) in your pipeline.

## Example AWS CLI Usage
Once the credentials are retrieved, you can use them to interact with AWS:
```bash
export AWS_ACCESS_KEY_ID="$AWS_ACCESS_KEY_ID"
export AWS_SECRET_ACCESS_KEY="$AWS_SECRET_ACCESS_KEY"
export AWS_SESSION_TOKEN="$AWS_SESSION_TOKEN"

aws s3 ls # Example command to list S3 buckets
```

## Security Considerations
- The **temporary credentials expire** after the configured session duration.
- **Use IAM least privilege policies** to limit permissions of the assumed role.
- Ensure **delegates using this script are secure** and properly managed.

## Troubleshooting
- Ensure the **role_arn** is correct and the role is assumable by the delegate.
- Verify that the **AWS CLI is installed** and accessible on the delegate.
- Check **IAM policies** to confirm the role has **sts:AssumeRole** permissions.
