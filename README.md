# KYC
As a Cloud Application Developer at AnyCompany Bank, I have been assigned to develop and deploy serverless solution on AWS to help streamline new customer onboarding process. I will create a solution with storage, database, computing, extracting text, notification, recognition, API validation, queuing and security solutions.

lab 1.

Created an S3 bucket to store the customers’ documents.
Configured a bucket policy to deny any Amazon S3 actions over HTTP (TLS is not used).
Configured an IAM role for an AWS Lambda function and set the role permissions.
actual steps
Capstone Project: Building a Customer Onboarding App - Lab 01
SPL-PW-300-DVF351-1 - Version 1.0.3

© 2025 Amazon Web Services, Inc. or its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited. All trademarks are the property of their owners.

Note: Do not include any personal, identifying, or confidential information into the lab environment. Information entered may be visible to others.

Corrections, feedback, or other questions? Contact us at AWS Training and Certification.

Lab overview
Customer onboarding is one of the most important stages of a customer’s journey with a bank. During this stage, the bank and customers exchange significant information. This exchange involves obtaining documentation needed to meet regulatory requirements and provide relevant products and services to customers.

AnyCompany Bank has decided to develop and deploy a customer onboarding serverless application on AWS. With the proposed customer onboarding solutions on AWS, the bank can use artificial intelligence (AI), machine learning (ML), and digital tools to streamline the onboarding process. With these solutions, the bank can transform the ways that customers are onboarded and reduce friction during this essential process.

As a cloud developer at AnyCompany Bank, you have been assigned the task of building the new onboarding application on AWS. The application is named Know Your Customer (KYC).

This is the first lab of a series of labs that build the KYC application for banking services. Your goal is to build the solution over 10 labs. In each lab, you build a few components of the overall solution.

In this lab, you create an Amazon Simple Storage Service (Amazon S3) bucket to store the customers’ documents, configure the bucket policy, and create an AWS Identity and Access Management (IAM) role with specific permissions to access the S3 bucket.

This is a challenge-based lab. High-level guidance and references are provided to assist you in completing the lab tasks. Detailed solution instructions are provided in collapsible sections, which you can expand.

Objectives
By the end of this lab, you should be able to do the following:

Create an S3 bucket to store the customers’ documents.
Configure a bucket policy to deny Amazon S3 actions over HTTP (TLS is not used).
Configure an IAM role for an AWS Lambda function and set the role permissions.
Technical knowledge prerequisites
To successfully complete this lab, you should have a basic knowledge of:

Amazon S3
AWS Management Console
IAM
Icon key
 Caution: Information of special interest or importance (not so important to cause problems with the equipment or data if you miss it, but it could result in the need to repeat certain steps).
 Hint: A hint to a question or challenge.
 Note: A tip or important guidance.
 Task complete: A conclusion or summary point in the lab.
 Warning: An action that is irreversible and could potentially impact the failure of a command or process (including warnings about configurations that cannot be changed after they are made).
Start lab
To launch the lab, at the top of the page, choose Start Lab.

 Caution: You must wait for the provisioned AWS services to be ready before you can continue.

To open the lab, choose Open Console .

You are automatically signed in to the AWS Management Console in a new web browser tab.

 Warning: Do not change the Region unless instructed.

Common sign-in errors
Error: Choosing Start Lab has no effect
In some cases, certain pop-up or script blocker web browser extensions might prevent the Start Lab button from working as intended. If you experience an issue starting the lab:

Add the lab domain name to your pop-up or script blocker’s allow list or turn it off.
Refresh the page and try again.
Lab environment
In this lab, you create and configure the Document S3 bucket and Document Lambda function IAM role resources. These resources are highlighted in the following diagram.

Lab 1 architectural diagram.

Image description: The diagram depicts the KYC application architectural diagram. The diagram highlights the key resources that you need to create and configure in this lab. These two resources are the Document S3 bucket and the Document Lambda function IAM role.

Services used in this lab
Amazon S3
Amazon S3 is an object storage service that offers industry-leading scalability, data availability, security, and performance. It stores and protect any amount of data for a range of use cases, such as data lakes, websites, built-for-the-cloud applications, backups, archives, machine learning, and analytics.

IAM
AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. With IAM, you can centrally manage permissions that control which AWS resources users can access. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources.

AWS services not used in this lab
AWS service capabilities used in this lab are limited to what the lab requires. Expect errors when accessing other services or performing actions beyond those provided in this lab guide.

Task 1: Create an S3 bucket to store the customers’ documents
In this task, you create an S3 bucket to store the customers’ application documents.

Use the AWS Management Console to create an S3 bucket in the lab region with the following settings:

The bucket name must be documentbucket-${AWS::AccountId}. For example, if your account ID is 111122223333, your bucket name would be documentbucket-111122223333.
Bucket encryption must be set to SSE-S3.
The bucket must not be public.
Do it yourself

 Hint: Here are some references to assist you in solving the issue:

Specifying server-side encryption with Amazon S3 managed keys (SSE-S3)
Blocking public access to your Amazon S3 storage
Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
At the top of the AWS Management Console, in the search bar, search for and choose S3.

On the Buckets page, choose Create bucket.

On the Create bucket page, configure the following:

For Bucket name, enter documentbucket-111122223333 after replacing 111122223333 with the value of your AccountId value that is listed to the left of these instructions.
For Block Public Access settings for this bucket, ensure that Block all public access is selected.
For Default encryption, select Server-side encryption with Amazon S3 managed keys (SSE-S3).
Accept the default values for all other settings.
Choose Create bucket.

 Task complete: You successfully created an S3 bucket to store the customers’ application documents.

Task 2: Configure a bucket policy to enforce encrypted connections
In this task, you configure a bucket policy to ensure that all connections to the bucket are encrypted using TLS.

Configure a policy on the document bucket that you created in the previous task to deny all Amazon S3 actions from any principal over HTTP (TLS is not used).
Do it yourself

 Hint: Here are some references to assist you in solving the issue:

AWS global condition context keys
What S3 bucket policy can I use to comply with the AWS Config rule s3-bucket-ssl-requests-only?
Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
On the Buckets page, choose the link of the bucket you created in the previous task.

On the bucket page, choose the Permissions tab.

In the Bucket policy section, choose Edit.

In the Policy pane, add the following policy snippet, and then replace both INSERT_BUCKET_NAME placeholders values with the value of your document bucket name.


{
    "Version": "2012-10-17",
    "Statement": [
    {
        "Sid": "Allow SSL Requests Only",
        "Action": "s3:*",
        "Effect": "Deny",
        "Resource": [
        "arn:aws:s3:::INSERT_BUCKET_NAME",
        "arn:aws:s3:::INSERT_BUCKET_NAME/*"
        ],
        "Principal": "*",
        "Condition": {
            "Bool": {
            "aws:SecureTransport": "false"
            }
        }
    }
    ]
}
Choose Save changes.

 Task complete: You successfully configured a bucket policy to ensure that all connections to the bucket are encrypted using TLS.

Task 3: Create an IAM role and configure role permissions
Later in the project, you will configure an AWS Lambda function to process the customer’s documents. The Lambda function needs an IAM execution role with the required permissions.

In this task, you create the IAM role for the Lambda function and add Amazon S3 permissions to the role so the Lambda function can read, write, and delete objects from the document bucket that you created.

Create an IAM role with the following settings:

The role name must be DocumentLambdaRole (case sensitive).
The role can only be assumed by a Lambda function.
You must attach the Role-Boundary managed policy as a permission boundary when creating the role. Failing to do so will prevent you from creating the role. The permission boundary is used as a security feature in the lab platform and is not directly related to this lab.
The role must have the following permissions on the documentbucket-${AWS::AccountId} bucket that you created:
Read objects from the bucket.
Write objects into the bucket.
Delete objects from the bucket.
The role must follow the principle of least privilege. You must allow only the required service to assume the role and attach only the required permissions.
 Note: The role you create in this task will not be used in this lab. It will be attached to a Lambda function in a later lab in this capstone project.

Do it yourself

 Hint: Here are some references to assist you in solving the issue:

Using IAM roles
Creating a role to delegate permissions to an AWS service
Lambda execution role
Creating IAM policies
 Note: If you need to edit your policy after creating it, you must delete the old policy and then re-create a new one.

Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
At the top of the AWS Management Console, in the search bar, search for and choose IAM.

First, create the permissions policy and then create the role*.

In the left navigation pane, in the Access management section, choose Policies.

On the Policies page, choose Create policy.

On the Specify permissions page, choose JSON.

In the Policy editor pane, add the following policy snippet, and then replace the INSERT_BUCKET_NAME placeholder value with the value of your document bucket name.


{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "S3Permissions",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::INSERT_BUCKET_NAME/*"
        }
    ]
}
 Note: If you see an error saying Access denied to access-analyzer:ValidatePolicy, you can ignore this error.

Choose Next.

On the Review and create page, in the Policy name box, enter DocumentLambdaPermissions.

Choose Create policy.

Now, create the role.

In the left navigation pane, in the Access management section, choose Roles.

Choose Create role.

On the Select trusted entity page, in the Trusted entity type section, select AWS service.

In the Use case section, on the Service or use case menu, select Lambda.

Choose Next.

On the Add permissions page, in the Permissions policies section, find and select the DocumentLambdaPermissions policy.

 Note: You can type DocumentLambdaPermissions in the search box to find the policy.

Expand the Set permissions boundary - optional section and select Use a permissions boundary to control the maximum role permissions.

Select the Role-Boundary policy.

 Note: You can type Role-Boundary in the search box to find the policy.

Choose Next.

On the Name, review, and create page, for the Role name, enter DocumentLambdaRole.

Choose Create role.

 Task complete: You successfully created the IAM role for the Lambda function and added Amazon S3 permissions to the role so the Lambda function can read, write, and delete objects from the document bucket that you created.

Conclusion
You successfully completed the following:

Created an S3 bucket to store the customers’ documents.
Configured a bucket policy to deny any Amazon S3 actions over HTTP (TLS is not used).
Configured an IAM role for an AWS Lambda function and set the role permissions.
End lab
lab 2

Created and configured a DynamoDB table to store customers’ metadata.
Created an SNS topic for the application notifications and subscribed to the topic.
Configured the IAM role to allow permissions for the DynamoDB table and SNS topic

actual steps

Lab environment
In this lab, you create and configure the resources that are highlighted in the following diagram.

Lab 2 architectural diagram.

Image description: The diagram depicts the KYC application architectural diagram. The diagram highlights the key resources that you must create and configure in this lab. These resources are the Customer DynamoDB table, the Document Lambda function IAM role, and the SNS topic.

Services used in this lab
Amazon S3
Amazon S3 is an object storage service that offers industry-leading scalability, data availability, security, and performance. It stores and protect any amount of data for a range of use cases, such as data lakes, websites, built-for-the-cloud applications, backups, archives, machine learning (ML), and analytics.

Amazon SNS
Amazon Simple Notification Service (Amazon SNS) is a managed service that provides message delivery from publishers to subscribers (also known as producers and consumers). Publishers communicate asynchronously with subscribers by sending messages to a topic, which is a logical access point and communication channel. Clients can subscribe to the Amazon SNS topic and receive published messages using a supported endpoint type, such as Amazon Data Firehose, Amazon SQS, AWS Lambda, HTTP, email, mobile push notifications, and mobile text messages (SMS).

DynamoDB
Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability. DynamoDB lets you offload the administrative burdens of operating and scaling a distributed database so that you don’t have to worry about hardware provisioning, setup and configuration, replication, software patching, or cluster scaling. DynamoDB also offers encryption at rest, which eliminates the operational burden and complexity involved in protecting sensitive data.

IAM
AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. With IAM, you can centrally manage permissions that control which AWS resources users can access. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources.

AWS services not used in this lab
AWS service capabilities used in this lab are limited to what the lab requires. Expect errors when accessing other services or performing actions beyond those provided in this lab guide.

Task 1: Create and configure a DynamoDB table to store customers’ metadata
The next step in building the KYC application is to create a database storage to store the customers’ information that was uploaded by the application. In this scenario, a DynamoDB table will be used for this purpose.

In this task, you create a DynamoDB table and configure its capacity provisioning.

Use the AWS Management Console to create a DynamoDB table in the lab region to store the customer’s metadata. The DynamoDB table should have the following settings:

The table must be named as CustomerMetadataTable.
The table partition key is named APP_UUID and has a type of string.
No sort key is required for the table.
The table should use standard class.
The table capacity is provisioned with two RCUs and two WCUs.
The table capacity must scale automatically when it reaches 70% of its provisioned capacity. The capacity should not be less than 2 RCU/WCU or more than 20 RCU/WCU at any given time.
After the table is created, capture and save the table Amazon Resource Name (ARN) of the table, as you will use it in a later task in the lab.

Do it yourself

 Hints: Here are some references to assist you in solving the issue:

Getting started with DynamoDB.
Managing throughput capacity automatically with DynamoDB auto scaling.
Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
At the top of the AWS Management Console, in the search bar, search for and choose DynamoDB.

Choose Create table.

On the Create table page, for the Table details section, configure the following options:

For Table name, enter CustomerMetadataTable.
For Partition key, enter APP_UUID and then select String.
In the Table settings section, select Customize settings.

In the Table class section, for Choose table class, select DynamoDB Standard.

In the Read/write capacity settings section, for Capacity mode, select Provisioned.

In the Write capacity section, configure the following:

For Auto scaling, select On.
For Minimum capacity units, enter 2.
For Maximum capacity units, enter 20.
For Target utilization (%), enter 70.
In the Read capacity section, configure the following:

For Auto scaling, select On.
For Minimum capacity units, enter 2.
For Maximum capacity units, enter 20.
For Target utilization (%), enter 70.
Accept the default values for all other settings.

Choose Create table.

After the table is created, on the Tables page, choose the link for the CustomerMetadataTable.

On the CustomerMetadataTable page, under the General information section, expand the Additional info section.

Locate and copy the table ARN and save it in your preferred note editor.

 Task complete: You successfully created a DynamoDB table and configured its capacity provisioning.

Task 2: Create an SNS topic for the application notifications and configure its subscription
Throughout the phase of submitting and verifying the customers’ documents, the solution must send notifications back to the customers.

In this task, you create an SNS topic and subscribe to it. This allows the code that you build in later labs to use the SNS topic to send notifications about the status of customers’ applications.

Use the AWS Management Console to:

Create a standard SNS topic named ApplicationNotifications in the lab region.
The topic must be encrypted using the (Default) alias/aws/sns key.
Subscribe to the topic using an email address.
Ensure that you confirm the subscription in the SNS topic.
After the topic is created, capture and save the table ARN of the topic, as you will use it in a later task in the lab.

 Note:

The topic you create will be used to send notifications in future labs.
With Amazon SNS, you normally send push notification messages directly to apps on mobile devices using the mobile push notification feature, rather than email notifications. However, for the purpose of this lab, you send the notification by subscribing to the topic using email.
Do it yourself

 Hints: Here are some references to assist you in solving the issue:

Creating an Amazon SNS topic.
Subscribing to an Amazon SNS topic.
Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
At the top of the AWS Management Console, in the search bar, search for and choose Simple Notification Service.

On the Amazon Simple Notification Service page, choose Next step.

On the Create topic page, configure the following:

For Type, select Standard.
For Name, enter ApplicationNotifications.
Expand the Encryption - optional section and enable Encryption.
Accept the default values for all other settings.
Choose Create topic.

On the ApplicationNotifications page, in the Details section, locate and copy the topic ARN of the topic and save it in your preferred note editor.

In the Subscriptions section, choose Create subscription.

On the Create subscription page, configure the following:

For Protocol, select Email.
For Endpoint, enter a valid email address that you can access.
Choose Create subscription.

Check the email address inbox that you entered. You should receive an email titled AWS Notifications, indicating that you subscribed to the SNS topic. Choose the Confirm Subscription link in the email to confirm.

Return to the AWS Management Console Amazon SNS page and refresh the subscription page.

Ensure that the subscription status is changed to Confirmed.

 Task complete: You successfully created an SNS topic to send notifications about the status of customers’ applications and subscribed to it.

Task 3: Configure the IAM role to allow permissions for the DynamoDB table SNS topic
In the previous lab, you created the DocumentLambdaRole that your Lambda function assumes in a later lab. The DocumentLambdaRole is provisioned for you as part of the lab build and already has the Amazon S3 permissions that you configured previously.

In this task, you add DynamoDB and Amazon SNS permissions to the DocumentLambdaRole.

Create a new managed policy and add the following permissions to it:

Write and update items to the newly created DynamoDB table.
Publish to the newly created SNS topic.
After you create the policy, attach it to the DocumentLambdaRole.
The policy must follow the principle of least privilege.
Do it yourself

 Hint: Here are some references to assist you in solving the issue:

Creating IAM policies (console).
Amazon DynamoDB: Allows access to a specific table.
Using identity-based policies with Amazon SNS.
 Note: If you need to edit your policy after creating it, you must delete the old policy and re-create a new one.

Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
At the top of the AWS Management Console, in the search bar, search for and choose IAM.

First, create the new permissions policy and then attach it to the role.

In the left navigation pane, in the Access management section, choose Policies.

On the Policies page, choose Create policy.

On the Specify permissions page, choose JSON.

In the Policy editor pane, replace any existing policy with the following policy snippet, and then make the following changes:

Replace the INSERT_YOUR_TABLE_ARN placeholder value with the value of your DynamoDB table ARN and keep the quotes.
Replace the INSERT_YOUR_TOPIC_ARN placeholder value with the value of your SNS topic ARN and keep the quotes.

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DynamoDBPermissions",
            "Effect": "Allow",
            "Action": [
                "dynamodb:PutItem",
                "dynamodb:UpdateItem"
            ],
            "Resource": "INSERT_YOUR_TABLE_ARN"
        },
        {
            "Sid": "SNSPermissions",
            "Effect": "Allow",
            "Action": "sns:Publish",
            "Resource": "INSERT_YOUR_TOPIC_ARN"
        }
    ]
}
choose Next.

In the Review and create pane, for the Policy name, enter Lab2DocumentPolicy.

Choose Create policy.

Now, attach the policy to the role.

On the Policies page, find and select the Lab2DocumentPolicy.

 Note: You can type Lab2DocumentPolicy in the search box to find the policy.

From the Actions menu, choose Attach.

On the Attach as a permissions policy page, find and select the DocumentLambdaRole.

 Note: You can type DocumentLambdaRole in the search box to find the policy.

Choose Attach policy.

 Task complete: You successfully configured the IAM role to allow permissions for the DynamoDB table SNS topic.

Conclusion
You successfully completed the following:

Created and configured a DynamoDB table to store customers’ metadata.
Created an SNS topic for the application notifications and subscribed to the topic.
Configured the IAM role to allow permissions for the DynamoDB table and SNS topic.
End lab

Lab 3.

Lab environment
In this lab, you create and configure the Document Lambda function and Document Lambda function IAM role resources. These resources are highlighted in the following diagram.

Lab 3 architectural diagram.

Image description: The diagram depicts the KYC application architectural diagram. The diagram highlights the key resources that you must create or configure in this lab. These two resources are the Document Lambda function and the Document Lambda function IAM role.

Services used in this lab
AWS Cloud9
AWS Cloud9 is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It includes a code editor, debugger, and terminal. AWS Cloud9 comes prepackaged with essential tools for popular programming languages, including JavaScript, PHP, Python, and more, so you don’t need to install files or configure your development machine to start new projects.

AWS IAM
AWS Identity and Access Management is a web service that helps you securely control access to AWS resources. With IAM, you can centrally manage permissions that control which AWS resources users can access. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources.

AWS Lambda
AWS Lambda is a compute service that lets you run code without provisioning or managing servers. Lambda runs your code on a high availability compute infrastructure and performs the administration of the compute resources, including server and operating system maintenance, capacity provisioning and automatic scaling, and logging. With Lambda, all you need to do is supply your code in one of the language runtimes that Lambda supports.

AWS services not used in this lab
AWS service capabilities used in this lab are limited to what the lab requires. Expect errors when accessing other services or performing actions beyond those provided in this lab guide.

Task 1: Configure the Lambda function IAM role permissions
AWS Lambda automatically monitors Lambda functions on your behalf, pushing logs to Amazon CloudWatch. To help you troubleshoot failures in a function, after you set up permissions, Lambda logs all requests handled by your function and automatically stores logs generated by your code through Amazon CloudWatch Logs.

You can insert logging statements into your code to help you validate that your code is working as expected. Lambda automatically integrates with CloudWatch Logs and pushes all logs from your code to a CloudWatch log group associated with a Lambda function, which is named /aws/lambda/LAMBDA_FUNCTION_NAME.

You will start developing the Lambda function code and testing it in a later task in this lab. However, to monitor the output of your Lambda function, you first must give permissions to the function to write logs to CloudWatch Logs.

In this task, you add CloudWatch Logs permissions to the Lambda function IAM role. The Lambda function IAM role is named DocumentLambdaRole. It is provisioned for you as part of the lab build and already has all the permissions that you configured previously.

Create a new IAM managed policy and add the following permissions to it:

Standard Lambda function permissions to create a log group.
Standard Lambda function permissions to create a log stream and put the log events into the stream.
After you create the policy, attach it to the DocumentLambdaRole.
The policy must follow the principle of least privilege.
Do it yourself

 Hint: Here is a reference to assist you in solving the issue:

Access to CloudWatch Logs
 Caution: When you are on the IAM > Policies > Create policy > Specify permissions page in the AWS Management Console, use the JSON policy editor instead of the Visual policy editor. The Visual policy editor may have an issue creating a resource that has a forward slash “/” in its name.

Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
At the top of the AWS Management Console, in the search bar, search for and choose IAM.

First, create the new permissions policy and then attach it to the role.

In the left navigation pane, in the Access management section, choose Policies.

On the Policies page, choose Create policy.

On the Specify permissions page, choose JSON.

In the Policy editor pane, replace any existing policy with the following policy snippet, and then make the following changes:

Replace the INSERT_LAB_REGION placeholders values with the value of your LabRegion listed to the left of these instructions.
Replace the INSERT_ACCOUNT_ID placeholders values with the value of your AccountId listed to the left of these instructions.

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "logs:CreateLogGroup",
            "Resource": "arn:aws:logs:INSERT_LAB_REGION:INSERT_ACCOUNT_ID:*"
        },

        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:logs:INSERT_LAB_REGION:INSERT_ACCOUNT_ID:log-group:/aws/lambda/DocumentLambdaFunction:*"
            ]
        }
    ]
}
Choose Next.

On the Review and create page, for the Policy name, enter Lab3DocumentPolicy.

Choose Create policy.

Now, attach the policy to the role.

On the Policies page, find and select the Lab3DocumentPolicy.

 Note: You can type Lab3DocumentPolicy in the search box to find the policy.

From the Actions menu, choose Attach.

On the Attach as a permissions policy page, find and select the DocumentLambdaRole.

 Note: You can type DocumentLambdaRole in the search box to find the policy.

Choose Attach policy.

 Task complete: You successfully added CloudWatch Logs permissions to the Lambda function IAM role.

Task 2: Get started with the AWS Cloud9 environment
AWS Cloud9 is an integrated development environment.

The AWS Cloud9 IDE offers a rich code-editing experience with support for several programming languages and runtime debuggers, and a built-in terminal. It contains a collection of tools that you can use to code, build, run, test, and debug software, and it helps you release software in the cloud.

You can access the AWS Cloud9 IDE through a web browser. You can configure the IDE to your preferences. For instance, you can switch color themes, bind shortcut keys, enable programming language-specific syntax coloring and code formatting, and more.

In this task, you start using the AWS Cloud9 environment provisioned in the lab.

To start the AWS Cloud9 environment, locate the Cloud9Url value listed to the left of these instructions and open it in a new browser tab.

At the top of the AWS Cloud9 work area, close the Welcome tab.

By default, the AWS Cloud9 work area top section is used as a file editor while the lower section is used as a bash terminal. The pane on the left is the file explorer. You can customize the work area as you prefer.

Navigate through the AWS Cloud9 environment.

 Learn more: For more information on AWS Cloud9 navigation, refer to Working with the AWS Cloud9 Integrated Development Environment (IDE)

Complete the following tasks:

Verify the AWS Cloud environment Python3 runtime version.
Create a new text file using the AWS Cloud9 editor. Name the file my_s3_object.txt, add some text to the file, and save it in your working directory.
Create a new Python code using the AWS Cloud9 editor. The code should upload the my_s3_object.txt file to the documentbucket-${AccountId} S3 bucket, which is provisioned during the lab build.
Use an AWS Command Line Interface (AWS CLI) command to list the objects in the documentbucket-${AccountId} S3 bucket and verify that the object was uploaded successfully.
Do it yourself

 Hint: Here is a reference to assist you in solving the issue:

Python tutorial for AWS Cloud9
Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
Verify the AWS Cloud9 environment Python3 runtime version

 Command: To find the Python3 runtime version, run the following command at the AWS Cloud9 bash terminal.


python -V
 Expected output: Your environment might be running a different minor Python version.


******************************
******* EXAMPLE OUTPUT *******
******************************

Python 3.12.3
Create a new text file

In the AWS Cloud9 menu, from the File menu, choose New File.

 Note: You can also choose the + icon on the file editor tab to create a new file.

In the new file, type any text like: This is an object to test Amazon S3 upload.

From the File menu, choose Save.

You can choose where to save the file. For this task, save the file on your home directory (default).

In the Filename field, enter my_s3_object.txt, and then choose Save. The file appears in the file explorer pane to the left.

Create a Python code to upload the file to the S3 bucket

Create a new file in the AWS Cloud9 environment. You can use steps similar to the ones you followed in the preceding step to create a text file.

Copy the following Python script, paste it into the new file, and then replace the INSERT_DOCUMENT_BUCKET with your DocumentBucketName value listed to the left of these instructions (keep the quotes).


# Import AWS boto3 module
import boto3

# Define file_name, object_name, bucket_name, and boto3 s3 client

file_name = 'my_s3_object.txt'
object_name = 'my_s3_object.txt'

# Replace <DOCUMENT_BUCKET> with your DocumentBucketName value listed to the left of these instructions (keep the quotes)

bucket_name = 'INSERT_DOCUMENT_BUCKET' 
s3_client = boto3.client('s3')

response = s3_client.upload_file(file_name, bucket_name, object_name)
Save the new file as s3_upload.py.

Test the code

 Command: From the bash terminal, run the following command to run the Python code.


python s3_upload.py
 Expected output:

None, unless there is an error.

Verify the object upload using the AWS CLI

You can verify that the object was uploaded successfully by using AWS CLI to list the objects in the bucket.

 Command: To list the objects in the bucket, in the bash terminal, run the following command. Replace the INSERT_DOCUMENT_BUCKET with your DocumentBucketName listed to the left of these instructions (keep the quotes).


aws s3 ls INSERT_DOCUMENT_BUCKET
 Expected output


******************************
******* EXAMPLE OUTPUT *******
******************************

2024-05-14 05:00:35         35 my_s3_object.txt
The file should be listed with details of the date created and size in bytes.

 Task complete: You successfully started using the AWS Cloud9 environment provisioned in the lab.

Task 3: Develop a Lambda function and configure its settings
Now, it is time to start developing the first Lambda function of the solution, which is referred to as the DocumentLambdaFunction. This function is invoked when a .zip file containing the customer information, license image, and selfie image is uploaded into the Document bucket.

In this task, you create the Lambda function; configure its permissions, invocation, and code; and verify it.

The Lambda function will perform multiple actions that you develop in its code throughout multiple labs. Here is a summary of what this Lambda function will perform:

Retrieve the .zip file from the S3 bucket, unzip the file, and write the individual unzipped files to another prefix in the S3 bucket.
Write the customer application data from the .csv file to the DynamoDB table.
Send a request to Amazon Rekognition to compare the faces on the driver’s license and selfie photos.
Send a request to Amazon Textract to extract the customer information from the driver’s license image.
Compare the extracted data (name, address, date of birth) with the application data provided by the customer.
Send the driver’s license number to a third-party validation API via Amazon Simple Queue Service (Amazon SQS).
Send notifications applications to the customer using the Amazon Simple Notification Service (Amazon SNS) topic based on the response of each validation.
The Lambda function needs permissions to perform the preceding actions. These permissions are provided by the DocumentLambdaRole that you created and configured in earlier labs.

Create a Lambda function with the following settings:

The function name is DocumentLambdaFunction.
Runtime should be Python 3.12.
The function should assume the DocumentLambdaRole when invoked.
The function should not run for more than 20 seconds.
The function should be invoked whenever an object is created by an Amazon S3 Put event in the zipped/ prefix in the Document bucket.
The Python code for the Lambda function should be a simple code that writes the Amazon S3 put event, which invoked the function, into CloudWatch Logs.
Do it yourself

 Hint: Here are some references to assist you in solving the issue:

Getting started with Lambda
Enabling and configuring event notifications using the Amazon S3 console
Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
Configure the Lambda function

At the top of the AWS Management Console, in the search bar, search for and choose Lambda.

On the Functions page, choose Create function.

On the Create function page, configure the following:

For Function name, enter DocumentLambdaFunction.
For Runtime, select Python3.12.
Expand Change default execution role, select Use an existing role, and then select DocumentLambdaRole from the Existing role menu.
Choose Create function.
On the DocumentLambdaFunction page, choose the Configuration tab.
In the General configuration section, choose Edit.
In the Timeout field, enter 0 for min and 20 for sec.
Choose Save.
On the DocumentLambdaFunction page, choose the Code tab.
In the Code source section, in the code pane, replace the existing code with the following code snippet.


def lambda_handler(event, context):
    print(event)
Choose Deploy.
Configure Amazon S3 event notifications

At the top of the AWS Management Console, in the search bar, search for and choose S3.

On the Buckets page, choose the link for the documentbucket-${AccountId}.

On the bucket page, choose the Properties tab.

In the Event notifications section, choose Create event notification.

On the Create event notification page, configure the following:

For Event name, enter lambda_trigger.
For Prefix-optional, enter zipped/.
In the Event type section, select Put (s3:ObjectCreated:Put). This should be the only event type you select.
For Destination, select Lambda function.
For Specify Lambda function, select Choose from your Lambda functions.
From the Lambda function menu, select DocumentLambdaFunction.
Choose Save changes.
Test the Lambda function

To test the Lambda function:

Create a prefix in the Document bucket named zipped (case sensitive).

Download the sample .zip file 8d247914.zip.

Upload the .zip file into the zipped/ prefix and check CloudWatch Logs to verify that your function is invoked and printing the Amazon S3 event into CloudWatch Logs.

Expand the following Verification instructions section for the full solution.

Verification instructions
Upload the .zip file to the S3 bucket

On the document S3 bucket page, choose the Objects tab.

Choose Create folder.

For Folder name, enter zipped.

Choose Create folder.

On the Objects page, choose the zipped/ link.

On the zipped/ page, choose Upload.

On the Upload page, choose Add files and add the 8d247914.zip file that you downloaded.

Choose Upload.

Verify CloudWatch Logs

At the top of the AWS Management Console, in the search bar, search for and choose Lambda.

On the Functions page, choose DocumentLambdaFunction link.

On the DocumentLambdaFunction, page, choose the Monitor tab.

Choose View CloudWatch logs.

On the /aws/lambda/DocumentLambdaFunction page, choose the link for the latest log stream.

Verify that the Lambda function was invoked and it printed the invocation event into the CloudWatch Logs.

 Task complete: You successfully created the Lambda function; configured its permissions, invocation, and code; and verified it.

Task 4: Develop Lambda function to interact with Amazon S3 events
Now that you created the DocumentLambdaFunction in the previous task, you can start developing its code to perform its first task.

The customer onboarding process starts by the customer using a mobile app to apply as a new customer to the bank. The customer, through the mobile app, enters personal details, uploads a driver’s license ID image, and takes a selfie. The mobile app writes the customer information into a .csv file, and then generates a unique identifier, denoted as app_uuid, for each customer. Then, the mobile app archives the customer details, driver’s license ID, and selfie into a .zip file and uploads it to the zipped/ prefix in the S3 bucket.

The uploaded .zip file has the following structure (for this example, “9c123456” is the app_uuid).


├── 9c123456.zip
│   ├── 9c123456_details.csv
│   ├── 9c123456_license.png
│   ├── 9c123456_selfie.png
In this task, you start developing the first task performed by the DocumentLambdaFunction to download the .zip file from the bucket and extract its contents to start processing and validating the customer details.

Modify the DocumentLambdaFunction to do the following:

Download the .zip file object from zipped/ prefix in the S3 bucket. Your code should know the .zip file name from the invocation event that you captured in the previous task.
Extract the .zip file into the /tmp directory in the Lambda function ephemeral storage.
Upload the three extracted files to the unzipped/ prefix in the same documentbucket-${AccountId} bucket. The individual unzipped files must be stored into a different prefix, as they will be required later in the validation so your Lambda function can reference them for future use.
Your code should also perform the following:

Extract the app_uuid value from the file name. You can save it into a variable named app_uuid.
Extract the unzipped selfie file object name in the unzipped/ prefix in the S3 bucket. The key should include the prefix, as well. You will use this file from the bucket for future sections of the code. You can save it into a variable named selfie_key.
Extract the unzipped driver’s license file object name in the unzipped/ prefix in the S3 bucket. The key should include the prefix, as well. You will use this file from the bucket for future sections of the code. You can save it into a variable named license_key.
Extract the local details file name in the Lambda function. You will use this file from the bucket for future sections of the code. You can save it into a variable named details_file.
Do it yourself

 Hint: Here are some hints to assist you in solving the issue:

Work with the assumption that the .zip file, individual files names, and types are always correct as they were validated by the mobile app.
Make sure that you upload the individual files to an S3 bucket prefix named unzipped/ (case sensitive).
Try following good practice when writing your code:
Add comments so your code is readable.
The DocumentLambdaFunction will perform multiple tasks that you code throughout this capstone project. It is recommended that you write these tasks as Python functions and pass the required I/O to or from the main program to each specific function. This also helps in case you need to reuse some code later.
You can use the CloudWatch Logs Live Tail feature to view the Lambda function logs. Refer to Use Live Tail to view logs in near real time.
Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
Update the Lambda function code

At the top of the AWS Management Console, in the search bar, search for and choose Lambda.

On the Functions page, choose the link for the DocumentLambdaFunction.

On the DocumentLambdaFunction page, choose the Code tab.

In the Code source section, in the code pane, replace the existing code with the following code snippet.


"Lambda function to process license, data, and selfie .zip file"

import json
import os
import zipfile
import boto3

unzipped_dir = "/tmp/unzipped/"
unzipped_s3_prefix = "unzipped/"

s3 = boto3.client('s3')

def unzip_object(bucket, key):
    """Download zip file, extract, return bucket name, object names, app_uuid,
    delete zip file, and uploading objects to incoming"""
    zip_name = os.path.basename(key)
    zip_fullpath = f'/tmp/{zip_name}'
    s3.download_file(bucket, key, zip_fullpath)
    with zipfile.ZipFile(zip_fullpath, 'r') as zip_ref:
        zip_ref.extractall(unzipped_dir)
    os.remove(zip_fullpath)

    zipped_files = os.listdir(unzipped_dir)
    return zipped_files

def lambda_handler(event, context):
    "Main lambda handler"
    record = event['Records'][0]
    bucket = record['s3']['bucket']['name']
    key = record['s3']['object']['key']

    # Unzip the object from the event
    files_list = unzip_object(bucket, key)

    # upload files to the unzipped location
    for file in files_list:
        s3.upload_file(unzipped_dir + file, bucket, unzipped_s3_prefix + file)

    # retrieve app_uuid, selfie_key, license_key, and details_file and save them as variables for later use
    app_uuid = os.path.basename(key).replace(".zip", "")
    selfie_key = f"{unzipped_s3_prefix}{app_uuid}_selfie.png"
    license_key = f"{unzipped_s3_prefix}{app_uuid}_license.png"
    details_file = f"{unzipped_dir}{app_uuid}_details.csv"

    # Add print to verify your solution by checking CloudWatch logs
    # You can remove these print statements once you verified your solution
    print(f"app_uuid = {app_uuid}")
    print(f"selfie_key = {selfie_key}")
    print(f"license_key = {license_key}")
    print(f"details_file = {details_file}")
Choose Deploy.

Test the function as described in the following section.

Test the Lambda function

Invoke your Lambda function by uploading the provided sample .zip file to the zipped/ prefix on the S3 bucket. You can retry uploading the file, as needed. Each upload will overwrite the existing object and create a new Lambda invocation event.

Verify that the Lambda function uploaded all three individual files to the unzipped/ prefix in the S3 bucket.

For debugging, you can use print statements in your code to verify that your code is working properly. You can view the CloudWatch Logs when the function is invoked to verify that your Lambda function code is extracting the app_uuid, selfie_key, license_key, and details_file into CloudWatch Logs.

Once you validate your function, you can remove the print statements from your code.

Lab files
The lab environment is ephemeral. All the lab resources are deleted when the lab time expires. If you want to keep a copy of the lab code, you can download all the lab code using the AWS Cloud9 environment.

To save the files on your local computer, in the AWS Cloud9 menu bar, choose File, and then choose Download Project.
 Task complete: You successfully developed the first task performed by the DocumentLambdaFunction to download the .zip file from the bucket and extract it to start processing and validating the customer details.

Conclusion
You successfully completed the following:

Configured the Lambda function IAM role to add permissions for standard Lambda logging.
Started the AWS Cloud9 instance and became familiar with the environment.
Configured a new Lambda function and attach its permissions.
Configured an Amazon S3 event notification for the bucket to trigger the Lambda function.
Developed Python code using the AWS SDK for Python (Boto3) to interact with Amazon S3.
Developed Python code to verify the customer files.
End lab

Lab 4
Lab environment
In this lab, you will create and configure the Document Lambda function and its code to parse the customer details from a .csv file and write it to a DynamoDB table. The Lambda function is highlighted in the following diagram.

Lab 4 architectural diagram.

Image description: The diagram depicts the KYC application architectural diagram. The Document Lambda function that you will build in this lab is highlighted.

Services used in this lab
AWS Cloud9
AWS Cloud9 is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It includes a code editor, debugger, and terminal. AWS Cloud9 comes prepackaged with essential tools for popular programming languages, including JavaScript, PHP, Python, and more, so you don’t need to install files or configure your development machine to start new projects.

AWS Lambda
AWS Lambda is a compute service that lets you run code without provisioning or managing servers. Lambda runs your code on a high availability compute infrastructure and performs all of the administration of the compute resources, including server and operating system maintenance, capacity provisioning and automatic scaling, and logging. With Lambda, all you need to do is supply your code in one of the language runtimes that Lambda supports.

AWS SAM
AWS SAM consists of two parts, AWS SAM templates and the AWS SAM CLI. AWS SAM templates provide a shorthand syntax, optimized for defining infrastructure as code (IaC) for serverless applications. An extension of AWS CloudFormation, you deploy AWS SAM templates directly to CloudFormation, benefiting from its extensive IaC support on AWS. The AWS SAM CLI is a developer tool that puts AWS SAM features at your fingertips. Use it to quickly create, develop, and deploy serverless applications.

AWS services not used in this lab
AWS service capabilities used in this lab are limited to what the lab requires. Expect errors when accessing other services or performing actions beyond those provided in this lab guide.

Task 1: Deploy resources using AWS SAM
In previous labs, you created resources using the AWS Management Console. Using the console is useful and simple when learning new concepts. However, as a developer, you might want to use other tools to create these resources using code. AWS provides many tools that can help you build your infrastructure as a code.

AWS Serverless Application Model is an open-source framework for building serverless applications. It provides shorthand syntax to express functions, APIs, databases, and event source mappings. With just a few lines per resource, you can define the application you want and model it using YAML. During deployment, AWS SAM transforms and expands the AWS SAM syntax into AWS CloudFormation syntax, enabling you to build serverless applications faster.

To deploy AWS resources using AWS SAM, you build these resources as a code in a template file. Then, use the AWS SAM CLI to build and deploy the template file. This will create a CloudFormation stack that builds the resources in your AWS account.

In this task, you are provided with a template.yaml file that already has the full code to deploy most of the resources that you built in previous labs using the console. Your task is to use the AWS SAM CLI to build and deploy an AWS SAM application that will build the lab resources.

The template file contains the following resources:

S3 bucket and its policy
DynamoDB table and its scaling configuration
Amazon Simple Notification Service (Amazon SNS) topic
You will use AWS SAM to add the Lambda function DocumentLambdaFunction and its code in the next task.

Do it yourself

Open the AWS Cloud9 environment and complete the following steps:

Verify that AWS SAM is running on AWS Cloud9 environment and check its version.
Explore the AWS SAM files that are provisioned in the lab build. You should see the template.yaml file under the ~/environment directory.
Open the template.yaml file and view its contents without changing it.
From the AWS Cloud9 bash terminal, use the command sam build to build the application.
From the AWS Cloud9 bash terminal, use the command sam deploy --stack-name kyc-app --guided to deploy the application. Accept the default option for all prompts when running the command.
 Note: Here is a summary of the options used in the sam deploy command:

stack-name kyc-app represents the CloudFormation stack that will be built by AWS SAM to deploy the resources. Do not use a different name for the stack.
guided allows the AWS SAM CLI to use prompts to guide you through the deployment.
This will start deploying a CloudFormation stack that builds the resources defined in the template. It also creates a samconfig.toml file that has the deployment parameters.

Once the stack is successfully deployed, use the AWS Management Console or AWS CLI to verify that the resources are correctly deployed.

 Note: Here is a description of the files in the AWS Cloud9 home directory:

8d247914.zip: Sample compressed file, which you can upload to Amazon S3 to invoke the Lambda function
DocumentLambdaFunction folder:
app.py: Lambda Python code for the DocumentLambdaFunction
previous_lab_solutions folder:
previous_document_lambda_code.py: Python code from the previous lab
README.md: Information file for AWS Cloud9
sample_lambda_resource.yaml: Sample Lambda function resource snippet to use when adding the Lambda function
template.yaml: AWS SAM template that you use to build lab resources
Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
To start the AWS Cloud9 environment, locate the Cloud9Url value listed to the left of these instructions and open it in a new browser tab.

At the top of the AWS Cloud9 work area, close the Welcome tab.

Verify the AWS SAM version and explore the template file

 Command: To verify that the AWS SAM is running on AWS Cloud9 and check its version, run the following command at the bash terminal.


sam --version
 Expected output: Your environment might be running a different minor AWS SAM version.


******************************
******* EXAMPLE OUTPUT *******
******************************

SAM CLI, version 1.116.0
In the AWS Cloud9 file explorer, expand the ~/environment directory, and then open the template.yaml file and explore its contents.

Do not make any changes to the template.yaml file at this point.

Build and deploy the application using the AWS SAM CLI

 Command: To build the application, run the following command in the AWS Cloud9 bash terminal.


sam build
 Expected output: Output has been truncated.


******************************
******* EXAMPLE OUTPUT *******
******************************

Build Succeeded
 Command: To deploy the application, run the following command in the AWS Cloud9 bash terminal.


sam deploy --guided --stack-name kyc-app
Accept all default answers for all the prompts.
 Expected output: Output has been truncated and might be slightly different.


******************************
******* EXAMPLE OUTPUT *******
******************************

Deploying with following values
        ===============================
        Stack name                   : kyc-app
        Region                       : us-west-2
        Confirm changeset            : False
        Disable rollback             : False
        Deployment s3 bucket         : aws-sam-cli-managed-default-samclisourcebucket-cxecxpvtaoax
        Capabilities                 : ["CAPABILITY_IAM"]
        Parameter overrides          : {}
        Signing Profiles             : {}

Successfully created/updated stack - kyc-app in us-west-2
Verify that the Amazon S3, DynamoDB, and Amazon SNS resources are built using the AWS Management Console or AWS CLI.
 Task complete: You successfully deployed resources using AWS SAM.

Task 2: Deploy a Lambda function using AWS SAM
In the previous task, you built resources using AWS SAM. However, the template file was already provisioned for you.

In this task, you will deploy a Lambda function using AWS SAM.

To deploy a Lambda function with a Python runtime using AWS SAM, you update the following two files:

AWS SAM template file (template.yaml).
Lambda code file (app.py). This file resides in the DocumentLambdaFunction directory.
For this task, the app.py file is configured with a simple Python code that will print the invocation event to CloudWatch Logs.

However, you will update the template.yaml file to add a Lambda function to your resources.

Since this might be the first time you write a resource using an AWS SAM template, a senior member of your team provided you with a sample template for creating a Lambda function using AWS SAM. The sample is in the sample_lambda_resource.yaml file in your AWS Cloud9 home directory.

Do it yourself

In the AWS Cloud9 environment, open the sample_lambda_resource.yaml file in the ~/environment directory.

Update the values for the parameters labelled REPLACE_WITH_ as below:

The function name must be DocumentLambdaFunction.
Runtime should be Python 3.12.
The function code should point to the path where the app.py is stored. This should be a relative path from where your template.yaml is stored.
The function should assume the DocumentLambdaRole when invoked. The DocumentLambdaRoleArn value is listed to the left of these instructions.
The function should not run for more than 20 seconds.
Copy the contents of the sample_lambda_resource.yaml file and paste it at the end of the template.yaml file.

Save the template.yaml file.

Make sure that you do not change anything in the template.yaml file except adding the Lambda function resource.

Deploy the changes using the sam build and sam deploy CLI commands. You do not need to use any options for the sam deploy command.

Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
Update the “template.yaml” file

In the AWS Cloud9 file explorer, expand the ~/environment directory, and then open the sample_lambda_resource.yaml file.

In the AWS Cloud9 editor, update the values for the following parameters:

For FunctionName, enter DocumentLambdaFunction.
For Timeout, enter 20.
For CodeUri, enter DocumentLambdaFunction/ (This is the relative path to the app.py file).
For Runtime, enter python3.12.
For Role, copy and paste the DocumentLambdaRoleArn value listed to the left of these instructions.
Copy all the contents of the file and paste it at the end of the template.yaml file. Make sure that your indentation is correct. The DocumentLambdaFunction resource must be aligned with the other resources in the template. Correct any misalignment due to the copy and paste operation.

Save the template.yaml file.

Build and deploy the application using AWS SAM CLI

 Command: To build the application, run the following command in the AWS Cloud9 bash terminal.


sam build && sam deploy
 Expected output: Output has been truncated and might be slightly different.


******************************
******* EXAMPLE OUTPUT *******
******************************

Build Succeeded

Successfully created/updated stack - kyc-app in us-west-2
Once the stack is successfully deployed, use the AWS Management Console or AWS CLI to verify that the function is correctly deployed.

 Task complete: You successfully deployed a Lambda function using AWS SAM.

Task 3: Configure an Amazon S3 event notification for a bucket to invoke the Lambda function using AWS SAM
After you deployed the Lambda function using AWS SAM, you configure the Amazon S3 event notification to invoke the Lambda function. This is similar to the task you did in the previous lab using the AWS Management Console. However, this time you complete the task using AWS SAM.

Do it yourself

Modify the template.yaml file to add an Amazon S3 event notification to trigger the DocumentLambdaFunction whenever an object is created by an Amazon S3 Put event in the zipped/ prefix in the document bucket.

 Hint: Here are some hints to assist you in completing the task:

A senior member of your team provided this reference to assist you in completing the task.
The Event key that you add is a property of the Lambda function resource in the AWS SAM template.
Ensure that you maintain the correct indentation when adding the Event key under the Lambda resource in the template file.
Once you update and save the template.yaml file, use the AWS SAM CLI to build and deploy commands.

Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
In the AWS Cloud9 file explorer, expand the ~/environment directory, and then open the template.yaml file.

Add the following snippet at the end of the file. The Events key must be aligned as one of the Lambda function Properties.


Events:
  S3Event:
    Type: S3
    Properties:
      Bucket: !Ref DocumentBucket # This is the bucket resource name in the template
      Events: s3:ObjectCreated:Put
      Filter:
        S3Key:
          Rules:
          - Name: prefix
            Value: zipped/
 Note: The Event key must be aligned exactly under the Role key in the Lambda function resource. Make sure that your indentation is correct. Correct any misalignment due to the copy and paste operation.

Save the template.yaml file.

 Command: To re-deploy the application, run the following command from the AWS Cloud9 bash terminal.


sam build && sam deploy
 Expected output: Output has been truncated and might be slightly different.


******************************
******* EXAMPLE OUTPUT *******
******************************

Build Succeeded

Successfully created/updated stack - kyc-app in us-west-2
Test the Lambda function

To test the Lambda function:

Create a prefix in the document bucket named zipped (case-sensitive).

Download the sample .zip file 8d247914.zip.

Upload the zipped file into the zipped/ prefix and check CloudWatch Logs to verify that your function is invoked and printing the Amazon S3 event into CloudWatch Logs.

Expand the following Verification instructions section for the full solution.

Verification instructions
Upload the zipped file to the S3 bucket

On the Amazon S3 document bucket page, choose the Objects tab.

Choose Create folder.

For Folder name, enter zipped.

Choose Create folder.

On the Objects page, choose the zipped/ link.

On the zipped/ page, choose Upload.

On the Upload page, choose Add files and add the 8d247914.zip file that you downloaded.

Choose Upload.

Verify CloudWatch Logs

At the top of the AWS Management Console, in the search bar, search for and choose Lambda.

On the Functions page, choose DocumentLambdaFunction link.

On the DocumentLambdaFunction page, choose the Monitor tab.

Choose View CloudWatch logs.

On the /aws/lambda/DocumentLambdaFunction page, choose the link for the latest log stream.

Verify that the Lambda function was invoked and printed the invocation event into the CloudWatch Logs.

 Task complete: You successfully configured the Amazon S3 event notification to invoke the Lambda function using AWS SAM.

Task 4: Develop Python code to parse the customer details .csv file
Now that you have created the DocumentLambdaFunction and configured the Amazon S3 event notification in the previous tasks, you will update the code of the Lambda function using AWS SAM.

The code will be similar to the code you developed in the last task of the previous lab so you do not need to rewrite the code from scratch. Instead, you can get the code from one of these options:

If you wrote your own code in the previous lab, you can use a copy of your code that you saved and downloaded at the end of Lab 03 - Task 4.
Use a sample solution provided in the previous_lab_solutions/previous_document_lambda_code.py file, which you can find in the home directory of your AWS Cloud9 environment.
 Note: Throughout the remaining labs in this project, you will regularly update the code of many Lambda functions. It is strongly recommended that you familiarize yourself with AWS SAM and use it as the preferred method to update the Lambda functions’ code.

Do it yourself

Modify the code in the app.py file located under the DocumentLambdaFunction directory using a working version of your code from the last lab or the sample solution provided.

Here is a reminder of what the code should perform:

Download the compressed file object from the zipped/ prefix in the S3 bucket. Your code should know the compressed file name from the invocation event that you captured in the previous task.

Extract the compressed file into the /tmp directory in the Lambda function ephemeral storage.

Upload the three extracted files to the unzipped/ prefix in the same documentbucket-${AccountId} bucket. The unzipped files must be stored into a different prefix as they will be required later in the validation so your Lambda function can reference them for future use.

Your code should also perform the following:

Extract the app_uuid from the file name. You can save it into a variable named app_uuid.
Extract the unzipped selfie file object name in the unzipped/ prefix in the S3 bucket. The key should include the prefix. You will need to use this file from the bucket for future sections of the code. You can save it into a variable named selfie_key.
Extract the unzipped license file object name in the unzipped/ prefix in the S3 bucket. The key should include the prefix. You will need to use this file from the bucket for future sections of the code. You can save it into a variable named license_key.
Extract the local details file name in the Lambda function. You will need to use this file from the bucket for future sections of the code. You can save it into a variable named details_file.
Once you update the app.py file, save it, and then build and deploy it using the AWS SAM CLI.

 Hint:

Work with the assumption that the compressed file and individual files names and types are always correct, as they were validated by the mobile app.
Make sure that you upload the individual files to an S3 bucket prefix named unzipped.
Adhere to good practice when writing your code:
Add comments so your code is easily readable and understandable.
The DocumentLambdaFunction will perform multiple tasks that you code throughout this capstone project. Write these tasks as Python functions and pass the required I/O to and from the main program to each specific function. This also helps when you need to reuse code later.
You can also use the AWS Cloud9 terminal to monitor the Lambda function logs using the following command.

aws logs tail /aws/lambda/DocumentLambdaFunction
Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
Update the Lambda function code

In the AWS Cloud9 file explorer, expand the ~/environment directory, and then open the app.py file under the DocumentLambdaFunction directory.

Replace the existing code with the following code snippet.


"Lambda function to process license, zip and selfie zip files"

import json
import os
import zipfile
import boto3

unzipped_dir = "/tmp/unzipped/"
unzipped_s3_prefix = "unzipped/"

s3 = boto3.client('s3')

def unzip_object(bucket, key):
    """Download .zip file, extract, return bucket name, object names, app_uuid,
    delete .zip file, and uploading objects to incoming"""
    zip_name = os.path.basename(key)
    zip_fullpath = f'/tmp/{zip_name}'
    s3.download_file(bucket, key, zip_fullpath)
    with zipfile.ZipFile(zip_fullpath, 'r') as zip_ref:
        zip_ref.extractall(unzipped_dir)
    os.remove(zip_fullpath)

    zipped_files = os.listdir(unzipped_dir)
    return zipped_files

def lambda_handler(event, context):
    "Main lambda handler"
    record = event['Records'][0]
    bucket = record['s3']['bucket']['name']
    key = record['s3']['object']['key']

    # Unzip the object from the event
    files_list = unzip_object(bucket, key)

    # Upload files to the unzipped location
    for file in files_list:
        s3.upload_file(unzipped_dir + file, bucket, unzipped_s3_prefix + file)

    # Retrieve app_uuid, selfie_key, license_key, and details_file and save them as variables for later use
    app_uuid = os.path.basename(key).replace(".zip", "")
    selfie_key = f"{unzipped_s3_prefix}{app_uuid}_selfie.png"
    license_key = f"{unzipped_s3_prefix}{app_uuid}_license.png"
    details_file = f"{unzipped_dir}{app_uuid}_details.csv"

    # Add print to verify your solution by checking CloudWatch Logs
    # You can remove these print statements once you verified your solution
    print(f"app_uuid = {app_uuid}")
    print(f"selfie_key = {selfie_key}")
    print(f"license_key = {license_key}")
    print(f"details_file = {details_file}")
Save the app.py file.

 Command: To redeploy the application, run the following command from the AWS Cloud9 bash terminal.


sam build && sam deploy
 Expected output: Output has been truncated and might be slightly different.


******************************
******* EXAMPLE OUTPUT *******
******************************

Build Succeeded

Successfully created/updated stack - kyc-app in us-west-2
Test the function as described in the following section.

Test the Lambda function

Invoke your Lambda function by uploading the provided sample compressed file to the zipped/ prefix on the S3 bucket. You can retry uploading the file, as needed. Each upload will overwrite the existing object and create a new Lambda invocation event.

Verify that the Lambda function uploaded all three individual files to the unzipped/ prefix in the S3 bucket.

For debugging, you can use print statements in your code to verify that your code is working properly. You can view the CloudWatch Logs when the function is invoked to verify that your Lambda function code is extracting the app_uuid, selfie_key, license_key, and details_file into CloudWatch Logs.

 Task complete: You successfully updated the code of the Lambda function using AWS SAM to parse the customer details .csv file.

Task 5: Develop Python code using the AWS SDK for Python to interact with a DynamoDB table
After your code unzipped the three files (details.csv, selfie.png, and license.png), you use these files to verify your customer details. The first step is to write the customer details to the DynamoDB table.

In this task, you will modify the DocumentLambdaFunction code to read and parse the details.csv file and then writes its parsed contents to the DynamoDB table, which you created earlier CustomerMetadataTable.

You have options to update the Lambda function code using the method you prefer. However, it is strongly recommended that you complete this task and all future tasks where code update is required using AWS SAM CLI.

Do it yourself

Modify the DocumentLambdaFunction to do the following:

Read and parse the details.csv file.
Write its parsed contents to the DynamoDB table, which you created earlier CustomerMetadataTable.
Recall that the details.csv file is still saved in the Lambda function /tmp directory. So you do not need to download it from the S3 bucket.
When writing the parsed .csv contents to the DynamoDB table, you must use the app_uuid value (which you extracted in the previous task) as the DynamoDB table HASH key.
You do not need to change your previous code. You only need to add extra code to it.
 Hint: Here are some changes you need to consider to add to your code for this task:

Add the Dynamodb table Boto3 resource, as you need to use it in this task.
Add the DynamoDB table as an environment variable named TABLE using the AWS SAM template file template.yaml. For more information, refer to the Environment section in the AWS SAM documentation.
At the end of your previous code, call a new function (you can name it parse_csv_ddb) and pass to it the app_uuid and the .csv file name. The app_uuid will be used as the HASH key for the DynamoDB table.
Define in your code the parse_csv_ddb function. The function should be a simple function that will open the .csv file, read its contents using the DictReader method, and then writes the contents to the DynamoDB table.
For more information on reading and writing .csv files in Python, refer to CSV File Reading and Writing.
Solution

Expand the following Detailed instructions section for the full solution.

Detailed instructions
Update the Lambda function code

In the AWS Cloud9 file explorer, expand the ~/environment directory, and then open the app.py file under the DocumentLambdaFunction directory.

Replace the existing code with the following code snippet. (This snippet has the full code of previous and current tasks.)


"Lambda function to process license, zip and selfie zip files"
# Environment variables:
# TABLE = CustomerMetaDataTable

import json
import os
import csv
import zipfile
import boto3

unzipped_dir = "/tmp/unzipped/"
unzipped_s3_prefix = "unzipped/"
env_table = os.environ['TABLE']

s3 = boto3.client('s3')
dynamodb = boto3.resource('dynamodb')
ddb_table = dynamodb.Table(env_table)

def unzip_object(bucket, key):
    """Download .zip file, extract, return bucket name, object names, app_uuid,
    delete .zip file, and uploading objects to incoming"""
    zip_name = os.path.basename(key)
    zip_fullpath = f'/tmp/{zip_name}'
    s3.download_file(bucket, key, zip_fullpath)
    with zipfile.ZipFile(zip_fullpath, 'r') as zip_ref:
        zip_ref.extractall(unzipped_dir)
    os.remove(zip_fullpath)

    zipped_files = os.listdir(unzipped_dir)
    return zipped_files

def parse_csv_ddb(app_uuid, details_file):
    "Load CSV and save to dynamo"
    with open(details_file, 'r', encoding="utf-8") as file:
        reader = csv.DictReader(file)
        details_dict = next(reader)

    ddb_table.put_item(Item={**details_dict, "APP_UUID": app_uuid})

    return details_dict

def lambda_handler(event, context):
    "Main lambda handler"
    record = event['Records'][0]
    bucket = record['s3']['bucket']['name']
    key = record['s3']['object']['key']

    # Unzip the object from the event
    files_list = unzip_object(bucket, key)

    # Upload files to the unzipped location
    for file in files_list:
        s3.upload_file(unzipped_dir + file, bucket, unzipped_s3_prefix + file)

    # Retrieve app_uuid from .zip file
    app_uuid = os.path.basename(key).replace(".zip", "")
    selfie_key = f"{unzipped_s3_prefix}{app_uuid}_selfie.png"
    license_key = f"{unzipped_s3_prefix}{app_uuid}_license.png"
    details_file = f"{unzipped_dir}{app_uuid}_details.csv"

    # Save CSV to dynamo
    
    details_dict = parse_csv_ddb(app_uuid, details_file)
Save the app.py file.

Update the template.yaml code to add the environment variable for the table

In the AWS Cloud9 file explorer, expand the ~/environment directory, and then open the template.yaml file.

Add the following snippet under the Properties of the DocumentLambdaFunction resource. Environment key must be aligned as one of the Lambda function Properties. Note that you are using CloudFormation !Ref to refer to the table. This means that when you deploy the same template in a different account, the function will always get the correct table name as an environment variable.


Environment:
  Variables:
    TABLE:  !Ref CustomerDDBTable
 Note: Make sure that your indentation is correct. Correct any misalignment due to the copy and paste operation.

Save the template.yaml file.

 Command: To redeploy the application, run the following command from the AWS Cloud9 bash terminal.


sam build && sam deploy
 Expected output: Output has been truncated and might be slightly different.


******************************
******* EXAMPLE OUTPUT *******
******************************

Build Succeeded

Successfully created/updated stack - kyc-app in us-west-2
Test the function as described in the following section.

Test the Lambda function

Invoke your Lambda function by uploading the provided sample compressed file to the zipped/ prefix on the S3 bucket. You can retry uploading the file, as needed. Each upload will overwrite the existing object and create a new Lambda invocation event.

Verify that the Lambda function wrote the key-value pairs of the .csv file into the DynamoDB table using the app-uuid hash key.

Expand the following DynamoDB table verification section for detailed instructions.

DynamoDB table verification
At the top of the AWS Management Console, in the search bar, search for and choose DynamoDB.

On the Amazon DynamoDB page, in the navigation pane at the left of the page, choose Tables.

On the Tables page, choose the link for the CustomerMetadataTable table.

On the CustomerMetadataTable page, choose Explore table items.

Locate the item in the Items returned section and verify that its hash key has the correct app-uuid value.

Lab files
The lab environment is ephemeral. All the lab resources are deleted when the lab time expires. If you want to keep a copy of the lab code, you can download all the lab code using the AWS Cloud9 environment.

To save the files on your local computer, in the AWS Cloud9 menu bar, choose File, and then choose Download Project.
 Task complete: You successfully modified the DocumentLambdaFunction code to read and parse the details.csv file and then writes its parsed contents to the DynamoDB table.

Conclusion
You successfully completed the following:

Deployed resources using AWS SAM.
Configured a new Lambda function, attached its permissions, and updated its code using AWS SAM.
Configured an Amazon S3 event notification for a bucket to invoke the Lambda function using AWS SAM.
Developed Python code to parse the customer details .csv file.
Developed code using the AWS SDK for Python to interact with a DynamoDB table.
End lab

Lab 5

Lab 6
