## Description

This template is compatible with the [vpc-env](../../environment-templates/vpc-env) template. It uses AWS Serverless Application Model (SAM) to create a [lambda function](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-function.html) that will be invoked by an SQS queue when new messages are available. Other microservices in your environment can publish events to the shared [Amazon SNS topic](https://docs.aws.amazon.com/sns/latest/dg/welcome.html) that can then be consumed by this Worker Service. 

The Lambda function is connect to the VPC in order to access private resources while the function is running. The function can be configured to run in a Public subnet or a Privat subnet using the subnet_type parameter. Connecting the function to the public subnet doesn't give it internet access or a public IP address. To give your function access to the internet, route outbound traffic to the NAT gateway in the public subnet. The NAT gateway has a public IP address and can connect to the internet through the VPC's internet gateway. Please see [Lambda Networking](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc.html) for more info. Since the private subnet has internet access through the NAT Gateway, lambda functions instantiated inside the private subnet will have internet access. 

Lambda function parameters like the function handler, runtime, memory size, timeout limit, and function code's Amazon S3 URI can be specified through the service input parameters.

The template also provisions a CodePipeline based pipeline to pull your application source code before building and deploying it to the Proton service. To use sample application code, please fork the sample code repository [aws-proton-sample-services](https://github.com/aws-samples/aws-proton-sample-services). By default, the template deploys a [function](https://github.com/aws-samples/aws-proton-sample-services/tree/main/lambda-worker) that writes the event, context object and SQS message to CloudWatch Logs. 

## Architecture

### Public Subnet
![worker-lambda-public-srv](../../images/worker-lambda-public-srv.png)

### Private Subnet
![worker-lambda-private-srv](../../images/worker-lambda-private-srv.png)

## Parameters

### Service Inputs

1. lambda_handler: The function within your code that is called to begin execution
2. lambda_memory: The size of your Lambda functions in MB
3. lambda_timeout: The timeout in seconds of your Lambda function
4. lambda_runtime: The runtime for your Lambda service
5. code_uri: The s3 link to your application
6. subnet_type: Subnet type for your function

### Pipeline Inputs

1. code_dir: Source directory for the service
2. unit_test_command: The command to run to unit test the application code
3. packaging_command: The commands which packages your code into a file called function.zip
4. environment_account_ids: The environment account ids for service instances using cross account environment

## Security

See [CONTRIBUTING](../../CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the [LICENSE](../../LICENSE) file.

