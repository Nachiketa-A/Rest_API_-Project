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

**Step5: Configure Bucket Options (Optional):**

1.Object Ownership: Choose the appropriate setting.

2.Block Public Access: Leave the defaults.

3.Bucket Versioning: Enable if needed.

4.Tags: Add any tags if necessary.

5.Default Encryption: Enable if required.

**Step6: Review and Create the Bucket:**

1.Review your settings.

2.Click the Create bucket button.

**Step7:Set Up Bucket Permissions:**

1.Go to the Permissions tab of your new bucket.

2.Bucket Policy: Add a policy to grant access to specific IAM users or roles.

![image](https://github.com/Nachiketa-A/Rest_API_-Project/assets/157089767/0461a2c0-818a-4c63-b5a6-aea949c1842a)

3.Replace YOUR_ACCOUNT_ID with your AWS account ID and terraform-user with your IAM user name.

**Step8:Navigate to IAM User Permissions:**

1.Go to the AWS Management Console.

2.Open the IAM service.

3.Select the IAM user (Hermoine).

**Step9:Attach Inline Policy:**

1.Go to the Permissions tab and click Add inline policy.

![image](https://github.com/Nachiketa-A/Rest_API_-Project/assets/157089767/d721a66f-4fca-40f0-99e9-aa2e2d517d4a)

**Step10:Ensure Correct Configuration in Terraform**

1.Ensure your terraform block configuration correctly references the S3 bucket and the state file path

2.Reinitialize Terraform

```hcl
terraform init
```
![image](https://github.com/Nachiketa-A/Rest_API_-Project/assets/157089767/7b609730-e5e7-4b50-9e13-da3b55ef1dab)

