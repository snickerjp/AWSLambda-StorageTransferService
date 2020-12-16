# AWSLambda-StorageTransferService

* Sync GCS & S3 from S3 for AWS Lambda
---

## Set AWS Lambda Environment

### Google Cloud

* GCP_PROJECT
* GCS_BUCKET

### AWS

* X_AWS_S3_BUCKET
* ACCESS_KEY
* SECRET_ACCESS_KEY


## Release Example

* Google Cloud. Enable -> [Storage Transfer API]

* Create AWS Lambda function.
	* ex.
        * Name: StorageTransferServiceStarter
        * Runtime: Python3.8

* Configure AWS CLI

* AWS Lambda Layer

```
env. Python3.8

mkdir python
pip install -r requirements.txt --target ./python
cd ./python
zip -r ../layer.zip .
cd ..

aws lambda publish-layer-version \
  --layer-name gcp-s3-layer \
  --zip-file fileb://layer.zip \
  --compatible-runtimes python3.8
```

* upload AWS Lambda function

```
zip -r my-deployment-package.zip lambda_function.py gcp-sa.json

aws lambda update-function-code \
  --function-name StorageTransferServiceStarter \
  --zip-file fileb://my-deployment-package.zip

aws lambda update-function-configuration \
  --function-name StorageTransferServiceStarter \
  --layers \
    "arn:aws:lambda:us-east-1:XXXXXXXXXXXX:layer:gcp-s3-layer:1" # layer arn
```

* Set trigger
* Set Role
* Set AWS Lambda Environment
