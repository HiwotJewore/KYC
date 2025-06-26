
# 🔐 Know Your Customer (KYC) Onboarding Application  
*Capstone project for AWS Cloud Application Developer program*  

---

## Project Overview

This repository contains the code and documentation for the **KYC onboarding solution** built as part of the Amazon Cloud Institute’s Cloud Application Developer course. The project simulates a real-world, serverless customer onboarding system for **AnyCompany Bank**, leveraging AWS services to ensure **security**, **scalability**, and **automation**.

The solution evolves across multiple labs, from setting up storage and Lambda functions to integrating AI-powered identity verification and preparing asynchronous workflows.

---

## Lab Progress

### ✅ Lab 1 – Setup S3 Bucket and Lambda Function

**Goal:**  
Create a secure Amazon S3 bucket to store customer documents and deploy a Lambda function to process onboarding events.

**Description:**  
Configured an S3 bucket with HTTPS-only access and created a Lambda function triggered by S3 events to start processing uploaded documents.

**Expected Outcome:**  
- Secure document storage  
- Event-driven processing with Lambda  
- Verified integration between S3 and Lambda  

---

### ✅ Lab 2 – DynamoDB and SNS Setup

**Goal:**  
Store customer metadata securely in DynamoDB and set up notifications using SNS.

**Description:**  
Created a DynamoDB table for customer metadata and an SNS topic to notify stakeholders about onboarding events.

**Expected Outcome:**  
- Reliable metadata storage  
- Real-time notifications via SNS  

---

### ✅ Lab 3 – Lambda Function to Unzip and Parse Files

**Goal:**  
Process `.zip` files by extracting and parsing customer documents.

**Description:**  
Enhanced the Lambda function to unzip files from S3, upload extracted contents, and parse filenames for downstream processing.

**Expected Outcome:**  
- Automated extraction and upload of onboarding files  
- Proper file organization in S3  
- Metadata prepared for next steps  

---

### ✅ Lab 4 – Parse Customer Metadata from CSV

**Goal:**  
Extract and store customer metadata from CSV files in DynamoDB.

**Description:**  
Parsed CSV inside uploaded `.zip` files to extract customer details and stored them in the DynamoDB table.

**Expected Outcome:**  
- Structured metadata ingestion  
- Customer data persisted for processing  

---

### ✅ Lab 5 – Integrate Amazon Textract for License Data Extraction

**Goal:**  
Extract driver’s license information using Amazon Textract.

**Description:**  
Integrated AnalyzeID API from Textract to extract and validate license data against CSV records.

**Expected Outcome:**  
- Automated extraction of identity info  
- Improved validation accuracy  

---

### ✅ Lab 6 – Textract Analysis, SNS Notification, and DynamoDB Update

**Goal:**  
Analyze ID documents, notify stakeholders, and update metadata.

**Description:**  
Used AWS SDK and Boto3 to perform Textract analysis, publish results to SNS, and update DynamoDB.

**Expected Outcome:**  
- Automated document analysis  
- Real-time notifications  
- Updated customer metadata  

---

### ✅ Lab 9 – Asynchronous Lambda Function Decomposition with Step Functions Preparation

**Goal:**  
Refactor the synchronous document Lambda function into multiple asynchronous Lambda functions orchestrated by AWS Step Functions.

**Description:**  
Decomposed the monolithic Lambda function into four smaller functions, each handling a distinct task in the onboarding workflow:

- **UnzipLambdaFunction:** Downloads and extracts zipped files from S3.
- **WriteToDynamoLambdaFunction:** Reads CSV and writes customer data to DynamoDB.
- **CompareFacesLambdaFunction:** Uses Amazon Rekognition to compare photos and updates DynamoDB.
- **CompareDetailsLambdaFunction:** Uses Amazon Textract to extract and validate license details.

**Expected Outcome:**  
- Modular, maintainable, asynchronous Lambda functions  
- Preparation for Step Functions orchestration  

---

## Lab 9 Tasks

| Task # | Lambda Function                | Description                                                        |
|--------|-------------------------------|--------------------------------------------------------------------|
| 1      | UnzipLambdaFunction            | Download, unzip, and upload files; extract `app_uuid`.             |
| 2      | WriteToDynamoLambdaFunction    | Parse CSV and write customer metadata to DynamoDB.                 |
| 3      | CompareFacesLambdaFunction     | Use Rekognition to compare photos; update DynamoDB and notify SNS. |
| 4      | CompareDetailsLambdaFunction   | Use Textract to extract license info; validate and update DynamoDB.|

---

## Deployment and Testing

- IAM roles and permissions are preconfigured for each Lambda function.
- Use the AWS CLI or Cloud9 environment to invoke functions with sample payloads for testing.
- Deploy Lambda functions using the following commands inside your Cloud9 terminal:

```bash
sam build
sam deploy
````

* Monitor Lambda function logs with:

```bash
aws logs tail /aws/lambda/<FunctionName>
```

---

## Repo Structure (Example)

```
/DocumentLambdaFunction
/UnzipLambdaFunction
/WriteToDynamoLambdaFunction
/CompareFacesLambdaFunction
/CompareDetailsLambdaFunction
/README.md
/template.yaml
/samconfig.toml
```

---

## Additional Notes

* This project is for educational purposes; do not use real customer data.
* Future labs will add full Step Functions workflow and third-party license validation.
* For questions or collaboration, please open an issue or contact me.

---
Awesome! Let’s add clear, concise **deployment** and **testing** instructions with sample commands, plus a **workflow automation snippet** for your README. This will make it super easy for you or anyone else to build, deploy, and test your Lambda functions step-by-step.

---

### Deployment & Testing Instructions (to add after your Lab 9 section)

````markdown
## 🚀 Deployment & Testing Instructions

### Prerequisites
- AWS CLI installed and configured with appropriate permissions.
- AWS SAM CLI installed.
- AWS Cloud9 environment (optional but recommended).

---

### Build and Deploy All Lambda Functions

Run these commands inside your project root directory where your `template.yaml` is located:

```bash
sam build
sam deploy --guided
````

* The `--guided` flag walks you through configuration the first time.
* This will package, deploy your Lambda functions, and create/update resources on AWS.

---

### Invoke Lambda Functions Manually for Testing

You can test each Lambda function with sample payloads like these (replace `YOUR_BUCKET_NAME` and function names as needed):

**UnzipLambdaFunction:**

```bash
aws lambda invoke --function-name UnzipLambdaFunction \
--cli-binary-format raw-in-base64-out \
--payload '{"detail": {"bucket": {"name": "YOUR_BUCKET_NAME"}, "object": {"key": "zipped/8d247914.zip"}}}' response1.json
cat response1.json
```

**WriteToDynamoLambdaFunction:**

```bash
aws lambda invoke --function-name WriteToDynamoLambdaFunction \
--cli-binary-format raw-in-base64-out \
--payload '{"detail": {"bucket": {"name": "YOUR_BUCKET_NAME"}}, "application": {"app_uuid": "8d247914"}}' response2.json
cat response2.json
```

**CompareFacesLambdaFunction:**

```bash
aws lambda invoke --function-name CompareFacesLambdaFunction \
--cli-binary-format raw-in-base64-out \
--payload '{"detail": {"bucket": {"name": "YOUR_BUCKET_NAME"}}, "application": {"app_uuid": "8d247914"}}' response3.json
cat response3.json
```

**CompareDetailsLambdaFunction:**

```bash
aws lambda invoke --function-name CompareDetailsLambdaFunction \
--cli-binary-format raw-in-base64-out \
--payload '{"detail": {"bucket": {"name": "YOUR_BUCKET_NAME"}}, "application": {"app_uuid": "8d247914"}}' response4.json
cat response4.json
```

---

### Monitor Logs

To view the logs of any Lambda function, run:

```bash
aws logs tail /aws/lambda/<FunctionName> --follow
```

Replace `<FunctionName>` with the actual function name (e.g., `UnzipLambdaFunction`).

---

## ⚙️ Automating Workflow with GitHub Actions (Optional)

You can automate build and deploy with this simple GitHub Actions workflow. Save this as `.github/workflows/deploy.yml` in your repo:

```yaml
name: Build and Deploy Lambda Functions

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Set up AWS CLI
      uses: aws-actions/configure-aws-credentials@v2
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-west-2  # change to your region

    - name: Install AWS SAM CLI
      run: |
        sudo pip install aws-sam-cli

    - name: Build SAM app
      run: sam build

    - name: Deploy SAM app
      run: sam deploy --no-confirm-changeset --no-fail-on-empty-changeset --stack-name KYCStack --capabilities CAPABILITY_IAM
```

* Add your AWS credentials as GitHub repository secrets (`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`).
* Adjust region and stack name as needed.
* On every push to master, your app will build and deploy automatically.

---


