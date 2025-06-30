�
� Know Your Customer (KYC) Onboarding
 Application
 Capstone project for AWS Cloud Application Developer Program
 🌟 Project Overview
 This repository contains my work building a serverless Know Your Customer (KYC) onboarding application
 for AnyCompany Bank as part of the Amazon Cloud Institute course. The system is designed to simulate
 secure and scalable customer onboarding using various AWS services, with each lab building upon the
 previous.
 📅 Lab Progress
 ✅ Lab 1 – Setup S3 Bucket and Lambda Function
 Goal: Store customer documents and process onboarding events.
 • 
• 
• 
Created an S3 bucket with HTTPS-only access.
 Deployed a Lambda function triggered by S3 uploads.
 Verified the event-driven integration.
 ✅ Lab 2 – DynamoDB and SNS Setup
 Goal: Store metadata and notify stakeholders.
 • 
• 
Created a DynamoDB table.
 Set up SNS for notifications.
 ✅ Lab 3 – Lambda to Unzip and Parse Files
 Goal: Extract and process .zip file contents.
 • 
• 
Lambda unzipped files and parsed filenames.
 Uploaded extracted documents to S3.
 ✅ Lab 4 – Parse Customer Metadata from CSV
 Goal: Extract customer data from CSV to DynamoDB.
 • 
• 
Enhanced Lambda to read CSVs.
 Stored structured customer data in DynamoDB.
 1
✅ Lab 5 – Amazon Textract for License Extraction
 Goal: Extract identity data using Textract.
 • 
• 
Used 
AnalyzeID API to process licenses.
 Matched extracted info to customer records.
 ✅ Lab 6 – Textract + SNS + DynamoDB Update
 Goal: Automate full metadata pipeline.
 • 
• 
• 
Analyzed documents.
 Published validation results via SNS.
 Updated records in DynamoDB.
 ✅ Lab 7 – Validate License Lambda + API Gateway
 Goal: Simulate a license validation backend.
 • 
• 
• 
Wrote 
ValidateLicenseLambdaFunction using Python 3.12.
 Integrated with API Gateway using a POST endpoint.
 Tested responses using 
curl and 
aws lambda invoke .
 ✅ Lab 8 – Submit License Lambda + SQS Integration
 Goal: Trigger license validation from SQS messages.
 • 
• 
• 
Created 
SubmitLicenseLambdaFunction .
 Triggered via SQS queue named 
LicenseQueue .
 Tested with messages using AWS CLI.
 ✅ Lab 9 – Asynchronous Lambda Refactor
 Goal: Decompose document Lambda into four services:
 1. 
2. 
3. 
4. 
5. 
UnzipLambdaFunction
 WriteToDynamoLambdaFunction
 CompareFacesLambdaFunction
 CompareDetailsLambdaFunction
 Prepared functions for Step Functions orchestration.
 ✅ Lab 10 – Step Functions + AWS X-Ray
 Goal: Orchestrate and monitor onboarding workflow.
 • 
Deployed 
DocumentStateMachine using Step Functions.
 2
• 
• 
Enabled AWS X-Ray tracing for all Lambdas.
 Monitored executions via CloudWatch and X-Ray.
 🚀 Deployment & Testing Instructions
 Prerequisites
 • 
• 
AWS CLI & SAM CLI installed
 IAM permissions configured
 Build & Deploy
 sam build
 sam deploy--guided
 Invoke Example
 aws lambda invoke--function-name UnzipLambdaFunction \--cli-binary-format raw-in-base64-out \--payload '{"detail": {"bucket": {"name": "YOUR_BUCKET"}, "object": {"key": 
"zipped/8d247914.zip"}}}' response.json
 Tail Logs
 aws logs tail /aws/lambda/UnzipLambdaFunction--follow
 💡 GitHub Actions CI/CD (Optional)
 Add 
.github/workflows/deploy.yml :
 name: Deploy Lambda
 on:
 push:
 branches: [master]
 jobs:
 deploy:
 runs-on: ubuntu-latest
 steps:- uses: actions/checkout@v3- uses: aws-actions/configure-aws-credentials@v2
 3
with:
 aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
 aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
 aws-region: us-west-2- run: sudo pip install aws-sam-cli- run: sam build- run: sam deploy --no-confirm-changeset --no-fail-on-empty-changeset \--stack-name KYCStack --capabilities CAPABILITY_IAM
 🎉 Final Project Summary
 I successfully completed all 10 labs of this KYC onboarding app using:
 • 
• 
• 
• 
• 
• 
• 
• 
• 
Amazon S3 – Secure file storage
 AWS Lambda – Stateless compute logic
 Amazon Textract – Identity document processing
 Amazon Rekognition – Facial verification
 Amazon DynamoDB – Structured metadata storage
 Amazon SNS – Real-time notifications
 Amazon SQS – Asynchronous message handling
 AWS Step Functions – Workflow orchestration
 AWS X-Ray – Monitoring and tracing
 Through this capstone, I built a real-world, serverless, AI-powered system and gained hands-on experience
 across the AWS stack.
 📄 Author
 Hiwot Jewore\ AWS Cloud Application Developer | CNA | Gospel Singer\ 
LinkedIn • 
YouTube • 
GitHub
 4
