# web

.NET 5.0 Amazon AWS Lambda Container Project Template

This project lets you create a .NET 5 empty ServiceStack web project ready for deployment as a AWS Lambda Function wired with API GateWay and packaged via a Docker image.

Create new project with [x dotnet tool](https://docs.servicestack.net/dotnet-new):

    $ dotnet tool install -g x
    $ x new aws-lambda-container ProjectName

### Prerequisites

- [.NET Core SDK 3.1 (runtime 5.0+) or higher](https://dotnet.microsoft.com/download/dotnet-core/3.1)
- [AWS .NET Core CLI](https://docs.aws.amazon.com/lambda/latest/dg/csharp-package-cli.html)
- [AWS account](https://aws.amazon.com/free/)

### Built with GitHub Actions
The above pre-requisites are setup ready to use with this template via GitHub Actions.

GitHub Actions need to authenticate with AWS required to deploy your application. To enable this you'll need to setup an IAM account specifically to enable this. This IAM account will need the permissions required to create and manage all the related AWS resources.

Below is an example of a IAM policy for a 'deploy' user that will be able to use the `serverless.template` CloudFormation file with AWS .NET Core CLI.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "lambda:CreateFunction",
                "iam:GetRole",
                "lambda:TagResource",
                "lambda:ListFunctions",
                "lambda:InvokeFunction",
                "lambda:GetFunction",
                "ecr:GetDownloadUrlForLayer",
                "ecr:GetAuthorizationToken",
                "iam:CreateRole",
                "iam:PassRole",
                "lambda:InvokeAsync",
                "lambda:GetFunctionConfiguration",
                "iam:AttachRolePolicy",
                "ecr:UploadLayerPart",
                "ecr:PutImage",
                "lambda:UpdateAlias",
                "lambda:UpdateFunctionCode",
                "ecr:BatchGetImage",
                "ecr:CompleteLayerUpload",
                "lambda:DeleteAlias",
                "ecr:DescribeRepositories",
                "lambda:PublishVersion",
                "ecr:InitiateLayerUpload",
                "ecr:BatchCheckLayerAvailability",
                "lambda:CreateAlias"
            ],
            "Resource": "*"
        }
    ]
}
```

### Use Case for ServiceStack App on AWS Lambda
AWS Lambda Containers allows the use of standard docker tooling with AWS serverless offering which for low traffic volume applications or APIs can be an extremely cost effective way of hosting an application.

AWS provides a Lambda [free-tier of 1M requests and 400,000GB-seconds of compute time per month](https://aws.amazon.com/lambda/pricing/). This can be enough traffic for many full ServiceStack APIs or applications.

### Deployment process
The provided GitHub Action YAML configures a workflow to build, test, package and deploy your ServiceStack Application with AWS Lambda and API GateWay.

Additional configuration is needed to get your application accessible from a friendly name subdomain. This can be done with API GateWay Mapping and Route 53.

### AWS Lambda Cold starts
Just like any AWS Lambda application or function, the latency impacts of 'cold starts' are something that can impact user experience significantly depending on your expected traffic load. When your function hasn't received any traffic for while, AWS Lambda will stop your container. Once a new request comes in, AWS Lambda needs to start up your application and wait for it to be ready to take your request. This creates the cold start latency problem. This also happens when there are already too many concurrent requests, to handle the increased load AWS Lambda will create new instances of your container to handle those requests.

If your load is low and expected, you can use tools like uptimerobot or others to periodically hit your application endpoint to keep at least one container alive to reduce this cold start latency.

Alternatively [AWS Provisioned Capacity](https://aws.amazon.com/blogs/aws/new-provisioned-concurrency-for-lambda-functions/) can be used (at additional cost) to mitigate these limitations.

