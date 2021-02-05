# lambda-serverless-py


Basic boilerplate for a Python Lambda Function + deployment using `python-lambda`. Working with Lambda is VERY easy, but the process of packaging libraries and deploying your code is where things start going haywire. 


## Requirements

- The pipeline requires an Amazon Web Services account to deploy and run. Signup for an AWS Account [here](https://portal.aws.amazon.com/billing/signup#/start).
- Python 3.7+


## Setup

### AWS Signup & Configuration 

Sign up for an AWS Account [here](https://portal.aws.amazon.com/billing/signup#/start). Additonally, the AWS Command Line Interface is required to interact with AWS Services. Download AWS CLI from [here](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)

### Configuring your AWS CLI 

Download your AWS Access and Secret access keys for your AWS Account. Steps to generate and download your keys can be found [here](https://docs.amazonaws.cn/en_us/IAM/latest/UserGuide/id_credentials_access-keys.html) 


> :warning: Never share your access and secret access keys or push them to GitHub<br />


Open command line tool of choice on your machine and run `aws configure`. Enter your access and secret access keys and leave the default region name and output format as null. 

```
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: 
Default output format [None]: json
```


### Create an IAM Role

Create an IAM Role on your AWS account called `lambda_basic_execution` with the follwing policies attached 

- `LambdaBasicExecution` 
- `AmazonS3FullAccess`
- `AmazonDynamoDBFullAccess`


### Clone

Clone this repo to your local machine using `git clone https://github.com/holladileep/lambda-serverless-py.git`



## Build + Deploy Lambda 

Lambda functions only contain a basic installation of Python + `boto3` and do not contain any external libraries. Any external libraries need to be packaged and deployed to AWS.


### Building the Lambda

We'll make use for virtualenv to create a virtual environment. For correct virtualenv setup and configuration, refer this [documentation](https://virtualenvwrapper.readthedocs.io/en/latest/install.html)

> Create a Virtual Environment 


```
mkdir first-lambda
cd first-lambda
mkvirtualenv first_lambda
pip3 install python-lambda
pip3 install pandas
```

If your python script requires any additonal libraries - you may install it at this stage.

> Initiate Lambda Deployement 

```
lambda init
```

This creates the following files: `event.json`,` __init__.py`, `service.py`, and `config.yaml`. 

The `service.py` is the file we are interested in. Edit `service.py` with your Python code and we are good to go. 


#### Configuring the Lambda

`config.yaml` file contains configuration information required to deploy the Lambda package to AWS. Configure the file with your access keys, secret access keys and function name before packaging and deploying the Python code. An example is as follows

```
region: us-east-1

function_name: Lambda_Function_1
handler: service.handler
description: My first lambda function
runtime: python3.7
role: lambda_basic_execution

# if access key and secret are left blank, boto will use the credentials
# defined in the [default] section of ~/.aws/credentials.
aws_access_key_id: <Enter your Access Keys>
aws_secret_access_key: <Enter your Secret Access Keys>

# These may be changed based on how much memory your code needs
timeout: 15
memory_size: 512

# Experimental Environment variables
environment_variables:
    bucket_name: Your-Bucket-Name
    dynamo_table: Your-DynamoDB-Table

# Build options
build:
  source_directories: lib
```


### Deploying the Lambda Function

Although - you could zip the contents of the directory and upload the file to the Lambda console - let's take advantage of `python-lambda` to do it for us

```
lambda deploy
```
This should create a new Lambda function on your AWS Lambda Console


## Using this Repository

The directories prefixed with `lambda-` are all Lambda functions, they can be cloned and deployed on to your AWS Account. The directory contains the config.yaml (with all the configuration information) +  Pipfile containing the project dependencies. 

#### lambda-event-driven-s3-to-dynamo
