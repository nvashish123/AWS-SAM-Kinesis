# AWS-SAM-Kinesis
AWS SAM template to create Lambda function with Kinesis Stream trigger

Steps - 
-----------------

1. Create Cloud9 environment
2. Make a directory `` mkdir buildersession `` and cd into it
3. Create new S3 bucket or use an existing bucket ``aws s3 mb s3://<YOUR_BUCKET_NAME ``
4. Package Lambda deployable zip by running the command - 

``sam package --template-file sam.yaml --s3-bucket <YOUR_BUCKET_NAME> --output-template-file packaged.yaml``

The above command will generate a new file named packaged.yaml in your working directory. This is the Cloudformation script which will create various resources in your environment in the next step.

5. Deploy Lambda function by running the following comamnd - 

``sam deploy --template-file /home/ec2-user/environment/buildersession/packaged.yaml --stack-name reInventBuilderSessionStack --capabilities CAPABILITY_IAM``

6. Check the progress on the Cloudformation by going to the Cloudformation console in your AWS account.

7. Verify that the Lambda function has been created by the Cloudformation, with Kinesis Stream as trigger.

8. Create Kinesis Data Generator utility in your account by running the following Cloudformation template - https://awslabs.github.io/amazon-kinesis-data-generator/web/help.html#template 

9. Log into the kinesis data generator by going to the URL printed in the output section of the above Cloudformation execution in step 8.

10. Select the AWS region and Kinesis Stream we just created from the drop down and start sending some data to the kinesis Stream. Sample apache access logs event record - 

``{{internet.ip}} - - [{{date.now("DD/MMM/YYYY:HH:mm:ss ZZ")}}] "GET /index.html HTTP/1.1" 200 104 "-" "ELB-HealthChecker/1.0" ``

11. The Lambda function simply polls the Kinesis Stream and prints it to the logs.

12. Verify the same by going to the CloudWatch logs console.

