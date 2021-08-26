The Cloudformation Template fulfills the below requirements.

1. An SQS queue which subscribes to a SNS topic based on a particular message type.
--> For a particular message type, Here applied the filterpolicy which filters accroding to the buyer-class and only allows SNS Topics which has the vip as a buyer-class.

2. A lambda function which is listening to the SQS Queue (in point 1) events and writing the
messages body to s3.
--> Created a lambda function which has the SQS Queue as a Event Source. Lambda function has runtime of nodejs14.x, timeout of 60 and MemorySize 512 MB. Here the lmabda function is written as a psuedo-code in nodejs, which has a bucketname, key and body as a parameter.

3. S3 bucket configured with versioning and lifecycle polices for that data.
--> S3 bucket is created with the mentioned requirement.

4. Necessary policies and roles required for this use-case.
--> Necessary Roles and Policies is attached to the relevant resourecs with the minimal access provided.