# web

.NET 5.0 Amazon AWS Lambda Container Project Template

This project lets you create a .NET 5 empty ServiceStack web project ready for deployment as a AWS Lambda Function wired with API GateWay and packaged via a Docker image.

Create new project with [app dotnet tool](https://docs.servicestack.net/netcore-windows-desktop):

    $ dotnet tool install -g app
    $ app new vue-desktop ProjectName

### Use Case for ServiceStack App on AWS Lambda
AWS Lambda Containers allows the use of standard docker tooling with AWS serverless offering which for low traffic volume applications or APIs can be an extremely cost effective way of hosting an application.

AWS provides a Lambda [free-tier of 1M requests and 400,000GB-seconds of compute time per month](https://aws.amazon.com/lambda/pricing/). This can be enough traffic for many full ServiceStack APIs or applications.

### Deployment process

### Built in GitHub Actions

### AWS Lambda Cold starts
Just like any AWS Lambda application or function, the latency impacts of 'cold starts' are something that can impact user experience significantly depending on your expected traffic load. When your function hasn't received any traffic for while, AWS Lambda will stop your container. Once a new request comes in, AWS Lambda needs to start up your application and wait for it to be ready to take your request. This creates the cold start latency problem. This also happens when there are already too many concurrent requests, to handle the increased load AWS Lambda will create new instances of your container to handle those requests.

If your load is low and expected, you can use tools like uptimerobot or others to periodically hit your application endpoint to keep at least one container alive to reduce this cold start latency.

Alternatively [AWS Provisioned Capacity](https://aws.amazon.com/blogs/aws/new-provisioned-concurrency-for-lambda-functions/) can be used (at additional cost) to mitigate these limitations.

