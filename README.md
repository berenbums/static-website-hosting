# Static website S3/CodePipeline project 
## Description
CloudFormation project to host a static website on Amazon S3/CloudFront and create a CodePipeline for continuous deployment.

The following AWS resources will be created:
- S3 bucket for storing files
- CloudFront distribution
- Route 53 record to access the front end
- CodePipeline for automatic deployments from GitHub to AWS (including IAM role, GitHub webhook and artifact bucket)
- Secrets Manager secret to store the GitHub access token.

## Prerequisites
Before the installation, make sure to have the following prepared:
- Hosted zone in Route 53, where the DNS record can be created
- A valid TLS certificate in Certificate Manager (us-east-1 region) including the certificate chain.

## Installation
Clone this repostory and deploy the project using the CloudFormation console or the AWS CLI:

    $ aws cloudformation create-stack \
      --region "$region" \
      --stack-name "$stackname" \
      --capabilities CAPABILITY_IAM \
      --template-body "file://$template" \
      --tags "Key=Name,Value=$stackname" \
      --parameters \
        "ParameterKey=GitHubPersonalAccessToken,ParameterValue=$domain" \
        "ParameterKey=GitHubRepo,ParameterValue=$domain" \
        "ParameterKey=GitHubUser,ParameterValue=$domain" \
        "ParameterKey=ProjectName,ParameterValue=$domain" \
        "ParameterKey=DomainName,ParameterValue=$domain" \
        "ParameterKey=CertificateArn,ParameterValue=$domain" \
        "ParameterKey=HostedZoneId,ParameterValue=$email"

