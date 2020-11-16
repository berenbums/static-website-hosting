# Static website S3/CodePipeline project 
## Description
CloudFormation template to host a static website on Amazon S3/CloudFront and create a CodePipeline for continuous deployment.

The following AWS resources will be created:
- S3 bucket for storing files
- CloudFront distribution
- Route 53 record to access the front end
- CodePipeline for automatic deployments from a GitHub repository to AWS (including IAM role, GitHub webhook and artifact bucket)
- Secrets Manager secret to store a GitHub access token.

## Prerequisites
Before the installation, make sure to have the following prepared:
- A hosted zone in Route 53, where the DNS record can be created
- A valid TLS certificate in Certificate Manager (us-east-1 region) including the certificate chain
- index.html/error.html files, which are specfied as default root/error objects (this can be changed inside cf-template.yaml)

## Installation
Clone this repostory and deploy the project using the CloudFormation console or the AWS CLI:

    aws_profile=my_profile
    aws_region=eu-west-1
    project_name=my-static-website
    github_repo=my_github_repo
    github_user=my_github_user
    github_token=my_personal_access_token
    domain=example.com
    certificate_arn=arn:aws:acm:<region>:<account>:certificate/123456789012-1234-1234-1234-12345678
    hosted_zone_id=Z1AB1Z2CDE3FG

    aws cloudformation create-stack \
      --profile "$aws_profile" \
      --region "$aws_region" \
      --stack-name "$project_name" \
      --capabilities CAPABILITY_NAMED_IAM \
      --template-body "file://cf-template.yaml" \
      --tags "Key=Name,Value=$project_name" \
      --parameters \
        "ParameterKey=GitHubRepo,ParameterValue=$github_repo" \
        "ParameterKey=GitHubUser,ParameterValue=$github_user" \
        "ParameterKey=GitHubPersonalAccessToken,ParameterValue=$github_token" \
        "ParameterKey=ProjectName,ParameterValue=$project_name" \
        "ParameterKey=DomainName,ParameterValue=$domain" \
        "ParameterKey=CertificateArn,ParameterValue=$certificate_arn" \
        "ParameterKey=HostedZoneId,ParameterValue=$hosted_zone_id"

