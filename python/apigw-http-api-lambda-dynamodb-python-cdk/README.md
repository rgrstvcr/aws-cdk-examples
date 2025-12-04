
# AWS API Gateway HTTP API to AWS Lambda in VPC to DynamoDB CDK Python Sample!


## Overview

Creates an [AWS Lambda](https://aws.amazon.com/lambda/) function writing to [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) and invoked by [Amazon API Gateway](https://aws.amazon.com/api-gateway/) REST API. 

![architecture](docs/architecture.png)

## Setup

The `cdk.json` file tells the CDK Toolkit how to execute your app.

This project is set up like a standard Python project.  The initialization
process also creates a virtualenv within this project, stored under the `.venv`
directory.  To create the virtualenv it assumes that there is a `python3`
(or `python` for Windows) executable in your path with access to the `venv`
package. If for any reason the automatic creation of the virtualenv fails,
you can create the virtualenv manually.

To manually create a virtualenv on MacOS and Linux:

```
$ python3 -m venv .venv
```

After the init process completes and the virtualenv is created, you can use the following
step to activate your virtualenv.

```
$ source .venv/bin/activate
```

If you are a Windows platform, you would activate the virtualenv like this:

```
% .venv\Scripts\activate.bat
```

Once the virtualenv is activated, you can install the required dependencies.

```
$ pip install -r requirements.txt
```

At this point you can now synthesize the CloudFormation template for this code.

```
$ cdk synth
```

To add additional dependencies, for example other CDK libraries, just add
them to your `setup.py` file and rerun the `pip install -r requirements.txt`
command.

## Deploy
At this point you can deploy the stack. 

Using the default profile

```
$ cdk deploy
```

With specific profile

```
$ cdk deploy --profile test
```

## After Deploy
Navigate to AWS API Gateway console and test the API with below sample data 
```json
{
    "year":"2023", 
    "title":"kkkg",
    "id":"12"
}
```

You should get below response 

```json
{"message": "Successfully inserted data!"}
```

## Monitoring and Observability

This application is instrumented with AWS X-Ray for end-to-end distributed tracing:

### Viewing X-Ray Traces

1. Navigate to the [AWS X-Ray Console](https://console.aws.amazon.com/xray/home)
2. Select **Service Map** to visualize the request flow: API Gateway → Lambda → DynamoDB
3. Select **Traces** to view individual request traces with detailed timing information
4. Use **Analytics** to identify performance bottlenecks and error patterns

### What's Being Traced

- **API Gateway**: All incoming HTTP requests and responses
- **Lambda Function**: Function invocations, cold starts, and execution time
- **DynamoDB**: All database operations (PutItem calls)

### Trace Retention

X-Ray traces are retained for 30 days by default. For longer retention, consider exporting traces to S3 or CloudWatch Logs.

## Security and Logging

This application implements comprehensive logging for security investigations and audit compliance:

### Configured Logging

- **Lambda Function Logs**: Retained for 1 year in CloudWatch Logs with security context (source IP, user agent, request ID)
- **API Gateway Access Logs**: Retained for 1 year with detailed request/response information
- **VPC Flow Logs**: Captures all network traffic within the VPC for security analysis
- **DynamoDB Point-in-Time Recovery**: Enabled for data protection and recovery

### Accessing Logs

1. **Lambda Logs**: Navigate to CloudWatch Logs → Log Groups → `/aws/lambda/apigw_handler`
2. **API Gateway Logs**: Navigate to CloudWatch Logs → Log Groups → Look for API Gateway access log group
3. **VPC Flow Logs**: Navigate to CloudWatch Logs → Log Groups → Look for VPC flow log group

### Log Retention

All logs are retained for 1 year to support security investigations and compliance requirements. Adjust retention periods in the CDK stack as needed.

### Security Requirements

This application requires the following logging services at the AWS account level:
- **AWS CloudTrail**: Should be enabled to capture API calls for Lambda, DynamoDB, and other AWS services
- **Recommended retention**: Minimum 90 days in CloudTrail, with longer-term storage in S3

## Cleanup 
Run below script to delete AWS resources created by this sample stack.
```
cdk destroy
```

## Useful commands

 * `cdk ls`          list all stacks in the app
 * `cdk synth`       emits the synthesized CloudFormation template
 * `cdk deploy`      deploy this stack to your default AWS account/region
 * `cdk diff`        compare deployed stack with current state
 * `cdk docs`        open CDK documentation

Enjoy!
