## ✅ Lab Progress

### ✅ Lab 1 - Setup S3 Bucket and Lambda Function

**Goal:**  
Create a secure Amazon S3 bucket to store customer documents and deploy an AWS Lambda function to process onboarding events.

**Description:**  
Set up an S3 bucket with enforced HTTPS-only access for secure document storage. Developed a Lambda function triggered by S3 events to start processing uploaded customer documents.

**Expected Outcome:**  
- Secure storage for onboarding documents.  
- Event-driven processing setup using Lambda.  
- Initial integration between S3 and Lambda validated.

---

### ✅ Lab 2 - DynamoDB and SNS Setup

**Goal:**  
Store customer metadata securely in DynamoDB and set up notification using SNS.

**Description:**  
Created a DynamoDB table for customer metadata storage. Set up an SNS topic for onboarding notifications and subscribed with an email to receive alerts on customer events.

**Expected Outcome:**  
- Reliable storage of onboarding metadata.  
- Real-time notifications to stakeholders via SNS.

---

### ✅ Lab 3 - Lambda Function to Unzip and Parse Files

**Goal:**  
Process uploaded `.zip` files by extracting and parsing customer documents.

**Description:**  
Enhanced the Lambda function to unzip `.zip` files upon upload to S3, upload extracted contents to a dedicated S3 prefix, and parse filenames (like app_uuid, selfie, license) for logging and downstream processing.

**Expected Outcome:**  
- Automated extraction of compressed onboarding documents.  
- Proper organization of files in S3.  
- Metadata available for next processing steps.

---

### ✅ Lab 4 - Parse Customer Metadata from CSV

**Goal:**  
Extract customer metadata from CSV files and store it in DynamoDB.

**Description:**  
Implemented parsing of CSV files inside the uploaded `.zip` to retrieve customer details such as name, DOB, and document number, then stored this information in the DynamoDB table.

**Expected Outcome:**  
- Structured metadata ingestion.  
- Customer details persisted for KYC processing.

---

### ✅ Lab 5 - Integrate Amazon Textract for License Data Extraction

**Goal:**  
Use Amazon Textract to extract driver’s license information from uploaded documents.

**Description:**  
Integrated Amazon Textract’s AnalyzeID API to automatically extract and analyze license data from uploaded images. Compared extracted data with CSV records for validation.

**Expected Outcome:**  
- Automated extraction of identity information.  
- Improved accuracy by cross-checking with CSV data.

---

### ✅ Lab 6 – Textract Analysis, SNS Notification, and DynamoDB Update
Goal:
Leverage AWS SDK to analyze ID documents, notify stakeholders via SNS, and update customer metadata in DynamoDB.

Highlights:

Configured IAM role with textract:AnalyzeID permissions.

Used Boto3 to perform Textract analysis on license images.

Published onboarding results to an SNS topic.

Updated customer records in DynamoDB based on extracted ID details.


### ✅ Lab 9 - [Your Lab 9 Title Here]

---
**Goal:**  
Briefly state the objective of Lab 9.

**Description:**  
Explain what you built or worked on in Lab 9.

**Expected Outcome:**  
Summarize what Lab 9 delivers or accomplishes.

---

### ✅ Lab 9 Progress

-### ✅ Lab 9 – Asynchronous Lambda Function Decomposition with Step Functions Preparation

---

**Lab Environment**

In this lab, the goal was to refactor the existing synchronous document Lambda function into multiple smaller Lambda functions that can run asynchronously. This prepares the solution for orchestration with AWS Step Functions in the next lab.

**Summary of tasks your document Lambda function performed synchronously:**

- Download and unzip files from the `zipped/` S3 prefix, uploading extracted files to `unzipped/` prefix.
- Parse and write customer metadata from CSV files to DynamoDB.
- Compare faces between selfie and license images using Amazon Rekognition.
- Compare license details using Amazon Textract.
- Send license info to a third-party API via SQS (implemented later).

The senior developer suggested decomposing these tasks into individual Lambda functions, each responsible for one task, and orchestrated by Step Functions.

---

### Lab 9 Tasks:

**Task 1:** Develop the **UnzipLambdaFunction**

- Downloads compressed files from `zipped/` prefix in S3.
- Extracts files and uploads to `unzipped/` prefix.
- Extracts the `app-uuid` from files for downstream processing.

**Task 2:** Develop the **WriteToDynamoLambdaFunction**

- Reads and parses customer CSV files.
- Writes customer metadata to DynamoDB under the `app-uuid` hash key.

**Task 3:** Develop the **CompareFacesLambdaFunction**

- Uses Amazon Rekognition to compare selfie and license photos.
- Updates DynamoDB with face match results.
- Sends SNS notification on failure.

**Task 4:** Develop the **CompareDetailsLambdaFunction**

- Uses Amazon Textract to extract license details.
- Compares extracted details with DynamoDB data.
- Updates DynamoDB with comparison results.
- Sends SNS notification on failure.

---

### Lab Outcome:

- Decomposed a monolithic Lambda function into four specialized Lambda functions.
- Prepared codebase for integration with AWS Step Functions state machine (next lab).
- Gained experience in asynchronous workflows and Lambda function modularization.

---

### Notes:

- IAM roles and permissions for each Lambda function were preconfigured.
- Lambda functions can be tested individually with sample payloads using AWS CLI.
- Cloud9 environment includes starter `app.py` files for each Lambda function.
- Use `sam build && sam deploy` commands to deploy functions.

---

*(Add links or references to your Lambda function folders and code files here if desired.)*
