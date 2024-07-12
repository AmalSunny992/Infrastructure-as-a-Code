# Infrastructure as Code (IaC)

## Objectives
- Understand the concept of Infrastructure as Code (IaC)
- Learn about the benefits of IaC
- Get an overview of popular IaC tools
- Implement a simple IaC example using AWS CloudFormation

## Concepts

## What is Infrastructure as Code (IaC)?
Infrastructure as Code (IaC) is the practice of managing and provisioning computing infrastructure through machine-readable scripts, rather than through physical hardware configuration or interactive configuration tools. IaC allows you to automate the entire infrastructure setup process, ensuring consistency and reducing the potential for human error.

### Benefits of IaC
Consistency: Ensures that your infrastructure setup is consistent across different environments (development, staging, production).

Version Control: Infrastructure configurations can be versioned and managed just like application code.

Scalability: Easily scale infrastructure up or down based on demand.

Automation: Reduces manual intervention and automates repetitive tasks.

Disaster Recovery: Quickly rebuild infrastructure in case of a disaster.

### Popular IaC Tools
**Terraform:** An open-source tool by HashiCorp that allows you to define and provision data center infrastructure using a declarative configuration language.

**AWS CloudFormation:** A service by AWS that provides a common language for describing and provisioning all the infrastructure resources in your cloud environment.

**Ansible:** An open-source automation tool that can be used for configuration management, application deployment, and task automation.

**Puppet:** A configuration management tool that helps in managing infrastructure throughout its lifecycle.

**Chef:** A configuration management tool that uses a pure-Ruby, domain-specific language (DSL) for writing system configuration "recipes".

## Implementing IaC with AWS CloudFormation

### Prerequisites
- An AWS account for deploying the infrastructure.
- Install the AWS CLI: AWS CLI Installation Guide
- Configure AWS CLI with your credentials: aws configure

### Example: Creating an S3 Bucket and CloudFront Distribution using AWS CloudFormation

**Step 1: Create a CloudFormation Template**
- CloudFormation Template: Create a file named cloudformation-template.yaml

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: CloudFormation template to create an S3 bucket and CloudFront distribution.

Resources:
  S3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: your-unique-bucket-name

  CloudFrontDistribution:
    Type: AWS::CloudFront::Distribution
    Properties:
      DistributionConfig:
        Origins:
          - Id: S3Origin
            DomainName: !GetAtt S3Bucket.DomainName
            S3OriginConfig: {}
        Enabled: true
        DefaultCacheBehavior:
          TargetOriginId: S3Origin
          ViewerProtocolPolicy: redirect-to-https
          AllowedMethods:
            - GET
            - HEAD
            - OPTIONS
          CachedMethods:
            - GET
            - HEAD
          ForwardedValues:
            QueryString: false
            Cookies:
              Forward: none
        ViewerCertificate:
          CloudFrontDefaultCertificate: true

Outputs:
  S3BucketName:
    Value: !Ref S3Bucket
    Description: Name of the S3 bucket

  CloudFrontDomainName:
    Value: !GetAtt CloudFrontDistribution.DomainName
    Description: Domain name of the CloudFront distribution
Parameters: Ensure the BucketName is unique as S3 bucket names must be globally unique.
```

**Step 2: Deploy the CloudFormation Stack**
- Create the CloudFormation Stack:

```sh
aws cloudformation create-stack --stack-name MyCloudFormationStack --template-body file://cloudformation-template.yaml --capabilities CAPABILITY_NAMED_IAM
```
- Monitor the Stack Creation:
    - Use the AWS Management Console or the CLI to monitor the status of the stack creation:

```sh
aws cloudformation describe-stacks --stack-name MyCloudFormationStack
```

- Verify the Infrastructure:
    - Once the stack creation is complete, you can verify the S3 bucket and CloudFront distribution in the AWS Management Console.
    - Retrieve the output values:

```sh
aws cloudformation describe-stacks --stack-name MyCloudFormationStack --query "Stacks[0].Outputs"
```

**Step 3: Clean Up the Infrastructure**
- Delete the CloudFormation Stack:

```sh
aws cloudformation delete-stack --stack-name MyCloudFormationStack
```
- Verify Deletion:
    - Use the AWS Management Console or the CLI to monitor the status of the stack deletion:

```sh
aws cloudformation describe-stacks --stack-name MyCloudFormationStack
```

## Summary
In this lesson, you learned the concept of Infrastructure as Code (IaC), its benefits, and how to implement it using AWS CloudFormation. IaC allows you to automate the setup and management of your infrastructure, ensuring consistency, scalability, and reliability.