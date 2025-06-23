<<<<<<< HEAD
         ___        ______     ____ _                 _  ___  
        / \ \      / / ___|   / ___| | ___  _   _  __| |/ _ \ 
       / _ \ \ /\ / /\___ \  | |   | |/ _ \| | | |/ _` | (_) |
      / ___ \ V  V /  ___) | | |___| | (_) | |_| | (_| |\__, |
     /_/   \_\_/\_/  |____/   \____|_|\___/ \__,_|\__,_|  /_/ 
 ----------------------------------------------------------------- 


Hi there! Welcome to AWS Cloud9!

To get started, create some files, play with the terminal,
or visit https://docs.aws.amazon.com/console/cloud9/ for our documentation.

Happy coding!
=======
# 🔐 Know Your Customer (KYC) Onboarding Application

This project is part of a capstone assignment for the **Cloud Application Developer** program at **Amazon Cloud Institute**. It simulates a real-world, serverless customer onboarding solution for **AnyCompany Bank**, focusing on **security**, **scalability**, and **automation** using AWS services.

---

## 👩🏾‍💻 Project Role
Cloud Application Developer at AnyCompany Bank

---

## 🎯 Objective
To streamline the **KYC (Know Your Customer)** onboarding process using a fully serverless architecture on AWS, ensuring customer documentation is processed **securely and efficiently**.

---

## 🛠️ Key AWS Services Used
- **Amazon S3** – Secure document storage with enforced HTTPS access only.
- **AWS Lambda** – Serverless compute to process onboarding tasks.
- **IAM Roles & Policies** – Secure permissions for Lambda and S3.
- *(Future labs may include Amazon Textract, Rekognition, SNS, API Gateway, SQS, etc.)*

---

## 📂 Project Files
- `lambda_function.py` – AWS Lambda handler to process events (e.g., file uploads).
- `8d247914_details.csv` – Sample customer data for onboarding.
- `README.md` – Project overview and documentation.

---

<details>
<summary>✅ Lab Progress</summary>

### ✅ Lab 1
- Created and secured an Amazon S3 bucket for customer documents.
- Wrote and uploaded a Lambda function.
- Configured IAM roles and permissions for secure access.
- Documented setup steps and tested integration between services.

### ✅ Lab 2
- Created DynamoDB table to store customer metadata.
- Created SNS topic for onboarding notifications.
- Subscribed to SNS with an email address.
- Updated IAM policies to allow access to DynamoDB and SNS.

### ✅ Lab 3
- Built `DocumentLambdaFunction` triggered by `.zip` uploads to S3.
- Extracted and uploaded unzipped files to `unzipped/` prefix.
- Parsed and logged file names (e.g., app_uuid, selfie, license) to CloudWatch Logs.

### ✅ Lab 4
- Parsed customer metadata from `.csv` file inside the `.zip`.
- Stored metadata in DynamoDB.
- Prepared inputs for text extraction and identity matching in future labs.

### 🧪 Planned: Lab 5
- Integrate Amazon Textract for license info extraction.
- Compare extracted data with CSV for accuracy.


### 🧪 Planned: Lab 6
- Use Amazon Rekognition to verify face match between selfie and license photo.

### 🧪 Planned: Lab 7
- Use Amazon SQS for async third-party driver’s license validation.
- Send SNS notifications with final onboarding result.

</details>

---

## 📌 Notes
- **Do not use real customer data** – This project is for educational purposes only.
- Future labs will expand functionality with additional AWS AI/ML services and automation tools.

---

## 📚 License
© 2025 Amazon Web Services, Inc. All rights reserved. For educational use only.
>>>>>>> 9b6e301e8e0b02facb2431a02711e19007f4b7ba
