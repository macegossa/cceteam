# cceteam

# CRUD API with DynamoDB and Lambda

This repository contains a CloudFormation template that creates a CRUD (Create, Read, Update, Delete) API using AWS API Gateway, AWS Lambda, and AWS DynamoDB.

## Table of Contents
- [Overview](#overview)
- [Solution Architecture](#Solution-Architecture)
- [Prerequisites](#prerequisites)
- [Deployment](#deployment)
  - [Clone the repository](#clone-the-repository)
  - [Configure AWS credentials](#configure-aws-credentials)
  - [Deploy the CloudFormation template](#deploy-the-cloudformation-template)
- [Usage](#usage)
  - [Using the AWS Console](#using-the-aws-console)
  - [Using the AWS CLI](#using-the-aws-cli)
  - [Using Postman](#using-postman)

## Overview

This CloudFormation template creates the following resources:

1. **DynamoDB Table**: A DynamoDB table named `CrudApiTable` with a partition key of `ID`. This is the only required parameter, all others are flexible.
2. **Lambda Function**: A Lambda function named `CrudLambdaFunction` that handles the CRUD operations for the DynamoDB table.
3. **API Gateway Rest API**: An API Gateway REST API named `CrudApi` with a resource named `items` and an `ANY` method that invokes the `CrudLambdaFunction`.
4. **Lambda Execution Role**: An IAM role that grants the Lambda function the necessary permissions to access the DynamoDB table.
5. **API Gateway Deployment**: Deploys the API Gateway REST API to a stage named `Prod`.
6. **Lambda Function for Generating Summary**: A Lambda function named `GenerateSummaryLambdaFunction` that scans the DynamoDB table, generates a summary of the total items, and stores the summary in an S3 bucket.
7. **S3 Bucket**: An S3 bucket named `my-dynamodb-summary-bucket-<AWS_ACCOUNT_ID>` to store the generated summaries.
8. **EventBridge Rule**: A CloudWatch Events rule that triggers the `GenerateSummaryLambdaFunction` every 7 days (weekly).

## Solution-Architecture


1. **:Scalability**:: AWS Lambda and DynamoDB are both highly scalable services, allowing the infrastructure to automatically scale up or down based on demand, without the need for manual intervention.
2. **:Serverless Architecture**:: By using AWS Lambda, the application logic is decoupled from the infrastructure management, enabling a serverless architecture. This reduces the operational overhead and allows the team to focus on developing the application functionality.
3. **:Flexibility**:: The combination of API Gateway, Lambda, and DynamoDB provides a flexible and modular architecture, making it easy to extend or modify the application in the future.
4. **:Cost-Effectiveness**:: With a serverless architecture, you only pay for the resources you use, rather than for provisioned capacity. This can result in significant cost savings, especially for applications with variable or unpredictable traffic patterns.
5. **:Reliability**:: AWS managed services, such as DynamoDB and API Gateway, provide high availability and durability, ensuring the application remains reliable and fault-tolerant.
6. **:Simplified Development**:: The abstraction of infrastructure management allows developers to focus on building the application logic, rather than worrying about provisioning and maintaining servers.
7. **:Integrated Services**:: The integration between API Gateway, Lambda, and DynamoDB simplifies the development and deployment process, as they work seamlessly together.

## Prerequisites

- An AWS account with the necessary permissions to create the resources in the CloudFormation template.
- AWS CLI installed and configured on your local machine.
- Postman installed on your local machine.

## Deployment

### Clone the repository

1. Open a terminal or command prompt.
2. Navigate to the directory where you want to clone the repository.
3. Run the following command to clone the repository:

   ```
   git clone https://github.com:macegossa/cceteam.git
   ```
   
### Configure AWS credentials

Before deploying the CloudFormation template, you need to configure your AWS credentials. You can do this using the AWS CLI:

1. Run the following command to configure your AWS credentials:

   ```
   aws configure
   ```

2. Follow the prompts to enter your AWS Access Key ID, AWS Secret Access Key, default region, and default output format.

### Deploy the CloudFormation template

1. Navigate to the cloned repository directory in your terminal or command prompt.
2. Run the following command to deploy the CloudFormation template:

   ```
   aws cloudformation create-stack --stack-name CrudApi --template-body file://path/to/crud-api.yaml
   ```

   Replace `path/to/crud-api.yaml` with the actual path to the CloudFormation template file in your local repository.

3. The CloudFormation stack creation may take a few minutes to complete. You can monitor the progress in the AWS CloudFormation console.

## Usage

After the CloudFormation stack has been successfully created, you can use the CRUD API in the following ways:

### Using the AWS Console

1. Open the AWS Management Console and navigate to the API Gateway service.
2. Locate the `CrudApi` API and click on it.
3. Click on the `Prod` stage.
4. You can now test the CRUD operations by clicking on the `items` resource and using the available methods.

### Using the AWS CLI

1. Locate the API Gateway endpoint URL from the CloudFormation stack outputs or the API Gateway console.
2. Use the following commands to interact with the CRUD API:

   - Create an item:
     ```
     aws apigateway invoke-api \
       --rest-api-id <API_GATEWAY_ID> \
       --stage-name Prod \
       --path-override /items \
       --http-method POST \
       --body '{"ID": "1234", "Cliente": "John Doe", "Produto": "Product A"}'
     ```

   - Get all items:
     ```
     aws apigateway invoke-api \
       --rest-api-id <API_GATEWAY_ID> \
       --stage-name Prod \
       --path-override /items \
       --http-method GET
     ```

   - Update an item:
     ```
     aws apigateway invoke-api \
       --rest-api-id <API_GATEWAY_ID> \
       --stage-name Prod \
       --path-override /items/<ID> \
       --http-method PUT \
       --body '{"Cliente": "Jane Doe", "Produto": "Product B"}'
     ```

   - Delete an item:
     ```
     aws apigateway invoke-api \
       --rest-api-id <API_GATEWAY_ID> \
       --stage-name Prod \
       --path-override /items/<ID> \
       --http-method DELETE
     ```

   Replace `<API_GATEWAY_ID>` with the actual API Gateway ID from the CloudFormation stack outputs or the API Gateway console, and `<ID>` with the ID of the item you want to update or delete.

### Using Postman

1. Open Postman on your local machine.
2. Create a new request and set the HTTP method to the desired CRUD operation (POST, GET, PUT, or DELETE).
3. Set the URL to the API Gateway endpoint URL, including the appropriate path (e.g., `/items` for creating an item, `/items/<ID>` for updating or deleting an item).
4. In the request body, include the necessary data for the CRUD operation (e.g., `{"ID": "1234", "Cliente": "John Doe", "Produto": "Product A"}` for creating an item).
5. Send the request and observe the response.

Postman provides a user-friendly interface to interact with the CRUD API, and it can be a convenient alternative to using the AWS CLI.
