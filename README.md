The Cloudformation Template fulfills the below requirements.

1. An SQS queue which subscribes to a SNS topic based on a particular message type.

-->>For a particular message type, Here applied the **filterpolicy** which filters accroding to the buyer-class and only allows SNS Topics which has the vip as a buyer-class.



2. A lambda function which is listening to the SQS Queue (in point 1) events and writing the
messages body to s3.

-->>Created a lambda function which has the SQS Queue as a Event Source. Lambda function has runtime of nodejs14.x, timeout of 60 and MemorySize 512 MB. Here the lmabda function is written as a **psuedo-code in nodejs**, which has a bucketname, key and body as a parameter.

![2021-08-26_12-20](https://user-images.githubusercontent.com/28305636/130914973-cd0d3549-f239-4812-9897-5c186b9cdcea.png)



3. S3 bucket configured with versioning and lifecycle polices for that data.

-->>S3 bucket is created with the mentioned requirement. Versioning is Enabled and below Lifecycle policy is applied.

![2021-08-26_12-16](https://user-images.githubusercontent.com/28305636/130914709-579492a6-e0d4-4f0b-9348-8d5553fad538.png)



4. Necessary policies and roles required for this use-case.

-->>Necessary Roles and Policies is attached to the relevant resourecs with the minimal access provided.


**Improvements based on efficiency and security point of view**
* Leverage existing retry logic and dead letter queues. If the Lambda function does not return success, the message will not be deleted from the queue and will reappear after the visibility timeout has expired.
* Allow built-in support for **dead letter queues** to be leveraged. After a specified number of failures, the message will automatically be placed in the DLQ for later inspection.
* Messages are deleted after the Lambda function completes successfully. As such, the visibility timeout for queues and messages should be long enough that any tasks or jobs associated with the message can be completed in time.
