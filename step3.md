# Implementing Network module to set VPC

### Step 1:

```hcl
terraform init
```

This command is really essential to initialize modules and also to download AWS required dependancies so that terraform can provision the infrastructure.

After implementing this command I was getting an error 

![image](https://github.com/Nachiketa-A/Rest_API_-Project/assets/157089767/0bafb7eb-780f-4ad2-9e9b-ed8d5d3f082d)


The 403 Forbidden error indicates that your IAM user does not have the necessary permissions to access the specified S3 bucket.

#### Steps to create S3 Bucket

**Step1: Sign In to AWS Management Console:**

1.Go to AWS Management Console.

2.Sign in with your AWS credentials.

**Step2: Navigate to S3 Service:**

1.In the AWS Management Console, find the Services menu at the top.

2.Type S3 in the search bar and select S3 from the drop-down list.

**Step3: Create a New S3 Bucket:**

1.Click the Create bucket button.

**Step4:Configure Bucket Settings:**

1.Bucket Name: Enter a unique bucket name (e.g., dev-proj-1-jenkins-remote-state-bucket-12345).

2.AWS Region: Select the AWS region (e.g. us-east-1).

![image](https://github.com/Nachiketa-A/Rest_API_-Project/assets/157089767/06031e71-3c6d-4125-b206-96d7233bd6d0)



