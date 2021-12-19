# Serverless Application for importing CSV files to DynamoDB By Using Lambda Function:

This serverless application created for automatically importing CSV files to DynamoDB when new csv files are uploaded into S3 bucket. It is an example for AWS Lambda.
I used python for this project and import boto3 module. 

Firstly, I examined link related to Amazon S3 notification event as shown below. You can easily see structure of the json of the S3 notification event.
* https://docs.aws.amazon.com/lambda/latest/dg/with-s3.html

and I assign variable bucket_name and s3_file_name as shown below.

   $  bucket_name = event['Records'][0]['s3']['bucket']['name']
   
   $  s3_file_name = event['Records'][0]['s3']['object']['key']
    
By using link as shown below,

* https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html#S3.Client.get_object

$ s3_client = boto3.client("s3") // get s3 object

$ resp = s3_client.get_object(Bucket=bucket_name, Key=s3_file_name) // get elements from s3 bucket csv file

And after some modification of the data, it is ready to add dynamodb.

emp_data is variable that is list element which is added to dynamodb by using for loop. 


            table.put_item(
            Item={
                    'id': emp_data[0],
                    'name': emp_data[1],
                    'location': emp_data[2]
            }
            )
## Infrastructure

AWS CDK (Python) used for Infrastructure as Code (Seperate repository)

* DynamoDB
* S3 Bucket
* Lambda (Triggered with S3 object created event)

IaC repository: https://github.com/anil1994/aws-iac

## CICD

 I used Github Actions used for auto deploying the Lambda. when my code is on the github, it is so easy to integrate with CI/CD by using github action. There are many predefined github action modules. By referring these module, I can easily integrate with CI/CD.
   
   When your environment has many platforms such as AWS,Google Cloud,Openshift, I suggest you should use jenkins CI/CD tool. Because, Jenkins have so many plugins that integrate with so many platforms (AWS,Azure,Openshift,GKE etc). When our environment has only AWS platform, we can use AWS Codebuild,Codedeploy,Codepipeline resources. Our github account can easily sync to AWS and we can easily use codepipeline.  
