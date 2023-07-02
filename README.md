# CloudFormation Stack: S3 Bucket Deployment with User-Defined Encryption Type

This CloudFormation stack deploys an S3 bucket with user-defined encryption type. The user can select either SSE-S3 or KMS encryption for the S3 bucket during stack creation. All the values, including the bucket name and encryption type, are inputted by the user as parameters. The stack also utilizes Outputs to provide information about the deployed resources.

## Prerequisites

- AWS CLI installed and configured on your machine
- Basic knowledge of AWS CloudFormation

## Getting Started

1. Create a new repository on your preferred version control platform (e.g., GitHub, GitLab).

2. Clone the newly created repository to your local machine.

3. In the repository directory, create a new file named `s3-stack.yaml`.

4. Copy and paste the following CloudFormation template code into the `s3-stack.yaml` file:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Deploy an S3 bucket with user-defined encryption type

Parameters:
  S3BucketName:
    Description: Enter S3 bucket name
    Type: String
    MinLength: 3
    MaxLength: 10
    AllowedPattern: "[A-Za-z]+"
  EncryptionType:
    Description: Select the encryption type for the S3 bucket
    Type: String
    Default: SSE-S3
    AllowedValues:
      - SSE-S3
      - KMS

Resources:
  S3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Ref BucketName
      BucketEncryption:
        ServerSideEncryptionConfiguration:
          - ServerSideEncryptionByDefault:
              SSEAlgorithm: !Ref EncryptionType

Outputs:
  BucketNameOutput:
    Description: The name of the created S3 bucket
    Value: !Ref BucketName
  EncryptionTypeOutput:
    Description: The name encryptionType
    Value: !Ref 'EncryptionType'
  BucketARNOutput:
    Description: The ARN of the created S3 bucket
    Value: !GetAtt S3Bucket.Arn
```

5. Save the `s3-stack.yaml` file.

6. Open your terminal and ensure that you have the AWS CLI installed and configured with the appropriate AWS credentials.

7. Navigate to the directory of your cloned repository.

8. Run the following command to create the CloudFormation stack:

```shell
aws cloudformation create-stack --stack-name <STACK_NAME> --template-body file://s3-stack.yaml --parameters ParameterKey=BucketName,ParameterValue=<BUCKET_NAME> ParameterKey=EncryptionType,ParameterValue=<ENCRYPTION_TYPE>
```

Replace `<STACK_NAME>` with the desired name for your stack, `<BUCKET_NAME>` with the desired S3 bucket name, and `<ENCRYPTION_TYPE>` with the desired encryption type (SSE-S3 or KMS).

9. Wait for the stack creation to complete. You can check the status of your stack by running the following command:

```shell
aws cloudformation describe-stacks --stack-name <STACK_NAME> --query 'Stacks[0].StackStatus'
```

Replace `<STACK_NAME>` with the name of your stack.

10. Once the stack creation is complete, you can retrieve the output values of the stack. Run the following command:

```shell
aws cloudformation describe-stacks --stack-name <STACK_NAME> --query 'Stacks[0].Outputs'
```

Replace `<STACK_NAME>` with the name of your stack.

## Cleanup

To delete the CloudFormation stack and remove the S3 bucket, run the following command:

```shell
aws cloudformation delete-stack --stack-name <STACK_NAME>
```

Replace `<STACK_NAME>` with the name of your stack. Confirm the deletion when prompted.

## Screenshot of Successful Deployment and Resources

A![image](https://github.com/Eunice2000/cloudformation/assets/90851478/60aca43f-e8b3-4f81-90d8-4a6ec4c4beeb)
![image](https://github.com/Eunice2000/cloudformation/assets/90851478/176e3d34-bd17-4cf7-946b-be0dd37fb7bd)

