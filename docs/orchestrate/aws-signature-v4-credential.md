## Overview

**[AWS Signature v4 Credential](https://developer.workday.com/documentation/GUID-07dfd182-d274-458d-9cf1-0695bcb9659e-enHYPHENus)** is a new credential type in Workday Orchestrate that lets a **Send HTTP Request** step authenticate against any AWS service that accepts the AWS Signature Version 4 protocol — AWS Lambda, S3, DynamoDB, and most other AWS services. **Releasing at DevCon 2026 hackathon.** Available for all Orchestrate template types allowing you to invoke your custom owned **custom code via AWS Lambda** from inside a low-code Orchestration. When you needed that before it meant dropping back to Studio, not anymore. 

> ⚠️ **You own the AWS side.** Orchestrate authenticates and connects; deploying the Lambda function or provisioning the S3 bucket is your responsibility in your own AWS account.

## The big idea: two credential modes, one credential type
Bring your own AWS Account to Orchestrate and Integrations using AWSv4. Bring your AI inference to Orchestrate using a Lambda wrapping a Bedrock / SageMaker call and fold the response back into the orchestration flow.


There are two ways to supply AWS credentials to an Orchestration. The choice depends on which tenant environment you are targeting. Both modes live behind the same **AWS Signature v4 Credentials** credential type — you pick the source when you create the credential.

| | **Mode A — CredStore Reference** | **Mode B — Temporary Security Token** |
|---|---|---|
| **Where the secret lives** | Tenant AWS CredStore (encrypted at rest) | Inline on the credential |
| **Allowed tenant environments** | All | DEV / IMPL / SBOX only |
| **Required configuration** | Access Key ID, AWS Region, AWS Service | Access Key ID, Secret Access Key, **Session Token**, AWS Region, AWS Service |
| **Typical use case** | Production integrations with stable IAM users | Local dev against short-lived STS / AssumeRole credentials |
| **Token lifetime** | Until rotated | 32 hours max (typically 15 min – few hours) |

**Mode B is for non Production Only.** 

## How it works — Mode A (CredStore Reference)

**Prerequisite:** an app containing an orchestration; permission to run the **Create AWS Access Key CredStore** task on your tenant.

### 1. Provision the secret (tenant-side, once)

On the tenant, run **Create AWS Access Key CredStore**:

- **App ID** — must match the ID of your app (Orchestrate uses this to look up the CredStore at runtime)
- **Access Key ID** — your AWS IAM user's access key
- **Secret Access Key** — stored encrypted, never read outside the runtime
- **Expiration Time** (optional) — for advance rotation planning

A single AWS Access Key CredStore can be shared across multiple Applications. The Secret Access Key only ever lives in the CredStore — never in the orchestration definition.

### 2. Configure the credential in the orchestration

In Orchestration Builder:

1. Open **Settings** → **AWSv4**.
2. Give the credential a **Name**.
3. From the **Type** drop-down, select **CredStore Reference**.
4. In the **Reference** field, use Expression Builder to provide the **Access Key ID** you stored in the tenant CredStore.
5. Specify the **AWS Region** (e.g. `us-west-2`).
6. Specify the **AWS Service** (e.g. `S3`, `lambda`).
7. Save.

### 3. Wire the credential to a Send HTTP Request step

On the **Send HTTP Request** step → **Auth** tab → **Authentication** drop-down → pick the AWSv4 credential you just created. That's it.

### 4. Runtime — what happens when the step fires

1. At launch, the runtime queries the tenant AWS CredStore for the App ID associated with the Orchestration and looks up the configured Access Key ID. The matching Secret Access Key is retrieved securely. The lookup is cached for efficiency across repeated calls in the same run.
2. The runtime opens the connection to the AWS service in the configured Region.
3. The runtime computes the AWS Signature v4 against the request (endpoint, method, headers, payload), adds it to the request headers, and sends. AWS performs the same signing calculation server-side; matching signatures = authenticated request.

The secret never leaves the CredStore. The orchestration only references the **shape** of the credential (Access Key ID, Region, Service).

## How it works — Mode B (Temporary Security Token, DEV only)

For local development against an assumed role or short-lived STS credentials. **All five attributes are required** — none are optional:

| Attribute | Description |
|---|---|
| Access Key ID | Temporary access key issued by STS / AssumeRole |
| Secret Access Key | Temporary secret key paired with the Access Key ID. Inlined directly into the credential — never read from CredStore |
| Session Token | STS-issued session token. AWS rejects signed requests using temporary credentials if this header is missing |
| AWS Region | Region of the target service (e.g. `eu-west-1`) |
| AWS Service | Service name expected by the signing string (e.g. `lambda`) |



## Tips before you start

- **The App ID on the CredStore must match your app's ID** — that's the lookup key the runtime uses
- **Pick the right AWS Service value at credential time** — it's part of the signing string; getting it wrong produces a signature mismatch from AWS, not a Workday error
- **For Lambda invocations**, use the AWS Lambda **Invoke** endpoint (`https://lambda.<region>.amazonaws.com/2015-03-31/functions/<function-name>/invocations`) with method `POST`
- **For S3**, the endpoint shape is `https://<bucket>.s3.<region>.amazonaws.com/<key>` — the bucket name appears in the host, the object key in the path

## Privacy & security

- **The Secret Access Key never appears in the orchestration definition.** It lives only in the tenant CredStore
- **The orchestration references the Access Key ID, Region, and Service** they describe *which* AWS account and service the orchestration is reaching. 
- **AWSv4 uses long-lived access keys.** If your security posture prefers short-lived credentials with limited blast radius, an assumed-role pattern with STS is the comparison point — discuss with your AWS administrator. 


## Further reading on the Developer Site

- [Create AWS Signature v4 Credentials](https://developer.workday.com/documentation/GUID-07dfd182-d274-458d-9cf1-0695bcb9659e-enHYPHENus) — the step-by-step in the official docs (Settings → AWSv4 flow, CredStore Reference vs Temporary Security Token)