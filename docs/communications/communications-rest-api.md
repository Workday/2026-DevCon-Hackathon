# Communications REST API

## Overview

The Communications REST API manages the lifecycle of programmatic messaging within Workday.

It provides tools for sending high-priority emails, tracking recipient engagement, and managing contact method verification through One-Time Passwords (OTP).

This service is designed to support both automated system alerts and user-driven interactions, offering a standardized way to ensure external recipients are reachable and verified before critical data is shared.

For reference documentation about the `communications` REST APIs, see `communications/externalRecipients and communications/v2Beta/email` in the [REST API Explorer on the Developer Site](https://developer.workday.com/rest-api-explorer).

## URL

```
https://{baseURL}/api/communications/v2Beta/gms
```

For Workday Extend or Integration apps, use the regional API Gateway base URL for your account. See [Reference: Workday Extend API Gateways and Authorization Base URLs](https://developer.workday.com/documentation/dlh1653340161856).

## Email Verification Workflow

This section describes the workflow for creating an external recipient, verifying their email via OTP, and sending follow-up emails.

**Step 1: Create an External Recipient**

Before sending communications to a non-worker or vendor, you must create a recipient record and associate it with a contact method.

Sample request: `POST /externalRecipients`

```
{
  "name": "John Doe",
  "contactMethods": [
    {
      "type": "email",
      "value": "jdoe@example.com"
    }
  ],
  "externalId": "EXT-998877"
}
 
```

Sample response: `201 Created`

```
{
  "id": "83f560b1ebee10006de1f1a392620000",
  "descriptor": "John Doe",
  "contactMethods": [
    {
      "id": "a51fdbbfcb7710001c84189659000000",
      "type": "email",
      "value": "jdoe@example.com"
    }
  ]
}
 
```

**Step 2: Initiate OTP Verification**

Use this endpoint to send a system-generated OTP to the recipient. You must include the "<%otp%>" placeholder in the body.

Sample request: `POST /externalRecipients/83f560b1ebee10006de1f1a392620000/contactMethodVerification`

```
{
  "contactMethod": { "id": "a51fdbbfcb7710001c84189659000000" },
  "subject": "Your Verification Code",
  "body": "<html><body>Your code is: <b>{{otp}}</b></body></html>",
  "transactional": true
}
 
```

**Step 3: Confirm the Contact Method**

Submit the 6-digit code received by the user to mark the contact method as verified.

Sample request: `POST /externalRecipients/83f560b1ebee10006de1f1a392620000/contactMethodConfirmation`

```
{
   "contactMethod": { "id": "a51fdbbfcb7710001c84189659000000" },
   "code": 123456
}
 
```

**Step 4: Send an Email**

Once verified, you can send standard notifications. Specify transactional: true for essential alerts to ensure high delivery priority.

Sample request: `POST /email`

```
{
  "emailAddress": "jdoe@example.com",
  "emailSubject": "Project Update",
  "emailBody": "<h1>Update</h1><p>Your project status has changed.</p>",
  "plainText": "Update: Your project status has changed.",
  "transactional": true
}
```

## Retrieval and Search Operations

**1. List or Search External Recipients**

Endpoint: `GET /externalRecipients`

Purpose: Use this operation to audit your recipient database or verify if a contact already exists before attempting a new creation. It supports standard Workday pagination and filtering.

Query Parameters:

* `contactMethodType`: Filter by method. Possible value: "email".
* `externalId`: Search for a specific record using your third-party system's identifier.
* `limit` and `offset`: Control pagination (Default: 20 per page).

Sample request: `GET /externalRecipients?externalId=EXT-998877&limit=10`

Sample response: `200 OK`

```
{
  "total": 1,
  "data": [
    {
      "id": "83f560b1ebee10006de1f1a392620000",
      "descriptor": "John Doe",
      "externalId": "EXT-998877",
      "contactMethods": [
        {
         "id": "a51fdbbfcb7710001c84189659000000",
         "type": "email",
         "value": "jdoe@example.com"
        }
      ]
    }
  ]
}
```

**2. Retrieve Specific External Recipient Details**

Endpoint: `GET /externalRecipients/{ID}`

Purpose: Returns the detailed information of a specific external recipient, including the list of all contact methods. This is useful for retrieving contact methods details or retrieving the Workday ID of a specific contact method.

Path Parameters:

* `{ID}`: The Workday ID of the recipient record.

Sample request: `GET /externalRecipients/83f560b1ebee10006de1f1a392620000/`

Sample response: `200 OK`

```
{
  "id": "83f560b1ebee10006de1f1a392620000",
  "descriptor": "John Doe",
  "contactMethods": [
    {
      "id": "a51fdbbfcb7710001c84189659000000",
      "type": "email",
      "value": "jdoe@example.com"
    }
  ]
}
 
```

**3. Retrieve Specific Contact Method Details**

Endpoint: `GET /externalRecipients/{ID}/contactMethod/{contactMethodID}`

Purpose: Returns the detailed status of a specific contact method, including its current verification standing (Verified, Pending, etc.). This is useful for troubleshooting why an OTP might not be sending or checking if a user has already completed verification.

Path Parameters:

* `{ID}`: The Workday ID of the recipient record.
* `{subresourceID}`: The Workday ID of the specific contact method (e.g., the specific email entry).You can use the `id` of a contact method for a specific external recipient from the `GET /externalRecipients/{ID}` method.

Sample request: `GET /externalRecipients/83f560b1ebee10006de1f1a392620000/contactMethod/a51fdbbfcb7710001c84189659000000`

Sample response: `200 OK`

```
{
  "type": "email",
  "value": "jdoe@example.com",
  "verificationRecord": {
    "status": "Verified",
    "verifiedAt": "2026-05-13T12:00:00Z",
    "remainingAttempts": 3
  },
  "id": "a51fdbbfcb7710001c84189659000000"
}
```

## Checking Validations

|Error Message|Error Code|Cause and Solution|
| --- | --- | --- |
|The contact method type is not supported. Valid contact method types: 'email'.|Invalid Contact Type - A3428|The contactMethods.type in the request body is not 'email'. Change the contact method type to 'email'.|
|Enter a valid email address. Example: john.doe@example.com. You can only enter 1 email address.|Invalid Input - A3425|The email address is invalid or multiple email addresses were provided. Ensure the contactMethods value is a single, valid email address format.|
|A recipient with this contact method value already exists.|Duplicate External Recipient - A3439|An attempt was made to create a recipient with a contact method (email) that is already in use. Use the GET /externalRecipients endpoint to find the existing recipient and update that record instead or use a different contact method value.|
|At least one of emailBody or plainText is required.|One of emailBody or plainText is required - A3275|The email request is missing both `emailBody` and `plainText` content. Ensure at least one of these fields is populated.|
|The contact method is already verified.|Invalid Input - A3425|Attempted to re-verify a contact method that is already verified. Check the status using the GET /externalRecipients/{ID}/contactMethod/{ID} endpoint. If verified, proceed to send the email.|
|The contact method is invalid for the external recipient.|Invalid Input - A3425|The contact method ID provided in the payload does not belong to the external recipient ID in the URL path. Verify that the IDs match.|
|Enter a valid display name that contains no more than 40 alphanumeric characters.|Invalid Input - A3425|The name field exceeds the maximum allowed length of 40 characters or contains invalid characters. Shorten the display name to 40 or fewer alphanumeric characters.|
|The field must contain the One-Time Password (OTP) placeholder. Specify the "<%otp%>" placeholder string.|OTP Placeholder Required - A3426|The body of the verification request is missing the required "<%otp%>" placeholder. Add the "<%otp%>" placeholder string to the body content.|
|Specify a body that contains no more than 65535 characters.|Invalid Input - A3425|The emailBody (HTML content) exceeds 65535 characters. Reduce the size of the emailBody.|
|Enter a plain text that contains no more than 65535 characters.|Invalid Input - A3425|The plainText content exceeds 65535 characters. Reduce the size of the plainText content.|
|Specify a subject that contains no more than 255 characters.|Invalid Input - A3425|The subject field exceeds the maximum allowed length of 255 characters. Shorten the subject to 255 characters or less.|
|The verification record status is Expired. Initiate a new verification request.|Verification Record Expired - A3433|The OTP confirmation attempt used an expired code. Initiate a new verification request ( POST /externalRecipients/{ID}/contactMethodVerification) to get a new code.|
|The verification record status is Verified. Initiate a new verification request.|Verification Record Verified - A3432|The contact method is already verified. If you need to re-verify, initiate a new verification request ( POST /externalRecipients/{ID}/contactMethodVerification).|
|The verification record status is Locked. Initiate a new verification request.|Verification Record Locked - A3434|The maximum number of failed OTP confirmation attempts has been reached. Initiate a new verification request ( POST /externalRecipients/{ID}/contactMethodVerification) to generate a fresh, unlocked OTP.|
|Specify a verification record for the contact method.|Invalid Input - A3425|Attempted to confirm an OTP for a contact method where verification was never initiated. First, initiate the verification process ( POST /externalRecipients/{ID}/contactMethodVerification).|

## Limitations

PATCH and DELETE operations are currently not available for the `externalRecipients` resource and `externalRecipients/{ID}/contactMethod` subresource.