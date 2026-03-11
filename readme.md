# **Automated Backup System using AWS Lambda and Amazon S3**
## **Project Overview :**

This project demonstrates how to build an automated backup system in AWS using AWS Lambda and Amazon S3. The system automatically creates backups of important files or data and stores them securely in an S3 bucket.

Automation reduces manual work, improves reliability, and ensures that backups happen on a scheduled basis.

## **Architecture :**

### **The backup workflow works as follows:**

1. A scheduled trigger (Amazon EventBridge / CloudWatch Events) invokes the Lambda function.

 2. The Lambda function processes the backup logic.

 3. The data or files are copied to an Amazon S3 bucket.

 4. The S3 bucket stores the backup securely and can be used for recovery when needed.

### **AWS Services Used:**

- AWS Lambda – Executes the backup script automatically.

- Amazon S3 – Stores the backup files.

- Amazon EventBridge / CloudWatch Events – Triggers the Lambda function on a schedule.

- IAM – Provides permissions for Lambda to access S3.

### **Project Architecture Diagram :**
![](./img/94ffac2b-b0d2-4f74-bb3d-ed99a039c5c1.png)

## **Project Setup Steps:**

**1. Create 2 S3 Bucket :**

  - Go to AWS Management Console.

- Open Amazon S3.

- Click Create bucket.

- Enter a unique bucket name.

- Select region.

- Create the bucket.
 
 ![](./img/Screenshot%20(154).png)

**2. Create IAM Role for Lambda :**

- Open IAM Console.

- Click Roles → Create Role.

- Select AWS Service → Lambda.

**Attach policies:**

AmazonS3FullAccess (or limited S3 access policy)

AWSLambdaBasicExecutionRole

![](./img/Screenshot%20(151).png)


Create the role.

**3. Create Lambda Function:**

- Open AWS Lambda.

- Click Create Function.

- Select Author from scratch.

- Runtime: Python 3.12

![](./img/Screenshot%20(157).png)

Attach the IAM role created earlier.

![](./img/Screenshot%20(156).png)
Example Lambda Code:

```
import boto3
import datetime

s3 = boto3.client('s3')

source_bucket = 'project-backup-source-bucket'
destination_bucket = 'project-backup-destination-bucket'

def lambda_handler(event, context):

    response = s3.list_objects_v2(Bucket=source_bucket)

    if 'Contents' in response:
        for file in response['Contents']:
            file_key = file['Key']

            copy_source = {
                'Bucket': source_bucket,
                'Key': file_key
            }

            timestamp = datetime.datetime.now().strftime("%Y-%m-%d-%H-%M-%S")

            backup_key = f"backup-{timestamp}-{file_key}"

            s3.copy_object(
                CopySource=copy_source,
                Bucket=destination_bucket,
                Key=backup_key
            )

    return {
        'statusCode': 200,
        'body': 'Backup completed successfully'
    }
```
![](./img/Screenshot%20(158).png)



**4. Create Scheduled Trigger:**

- Open Amazon EventBridge.

- Click Create Rule.

- Choose Schedule Pattern.

Example schedule:  Run every day
 

- Select Lambda function as target.

![](./img/Screenshot%20(159).png)

This will automatically run the backup process.

![](./img/Screenshot%20(160).png)

![](./img/Screenshot%20(161).png)

### **Project Features:**

1. Automated backups

2. Serverless architecture

3. Secure cloud storage

4. Scheduled backup execution

5. Scalable and cost-effective

### **Use Cases:**

1. Website file backups

2. Application log backups

3. Database export backups

4. Disaster recovery preparation

### **Conclusion:**

Automating backups is an essential part of modern cloud infrastructure. Using AWS services like Lambda and S3, we can build a reliable backup system without managing servers.

This project demonstrates how serverless automation can simplify infrastructure management while improving data safety.