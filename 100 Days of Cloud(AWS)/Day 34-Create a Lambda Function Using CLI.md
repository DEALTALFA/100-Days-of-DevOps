The Nautilus DevOps team continues to explore serverless architecture by setting up another Lambda function. This time, the task must be completed using the AWS Console to familiarize the team with the web interface. The function will return a custom greeting and demonstrate the capabilities of AWS Lambda effectively.

Create Python Script: Create a Python script named lambda_function.py with a function that returns the body Welcome to KKE AWS Labs! and status code 200.

Zip the Python Script: Zip the script into a file named function.zip.

Create Lambda Function: Create a Lambda function named datacenter-lambda-cli using the zipped file and specify Python as the runtime.

IAM Role: Use the IAM role named lambda_execution_role.

Test the Lambda Function: Test the function to ensure it returns the expected response.

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Welcome to KKE AWS Labs!'
    }
```
```bash
# get role info
aws iam list-roles | grep "role/lambda_exec*"
aws iam list-roles | grep "role/lambda_exec*" | cut -d '"' -f 4
 arn=$(aws iam list-roles | grep "role/lambda_exec*" | cut -d'"' -f4)
zip function.zip lambda_function.py
aws lambda create-function --function-name datacenter-lambda-cli \
--runtime python3.8 \
--role $arn \
--handler lambda_function.lambda_handler \
--zip-file fileb://function.zip
```
