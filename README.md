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
