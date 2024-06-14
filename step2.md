# Terraform Networking Module Setup for Jenkins

This guide provides a step-by-step explanation for setting up the Virtual Private Cloud (VPC) for Jenkins using the `networking` module in the `main.tf` file. 

### Step 1: Update `main.tf` File

To focus on setting up the VPC for Jenkins, comment out all other modules in the `main.tf` file except the networking module. 

The `main.tf` file should look like this:


```hcl
# Comment out other modules
# module "example_module" {
#   source = "./example_module"
#   ...
# }

# Networking module for VPC setup
module "networking" {
  source               = "./networking"
  vpc_cidr             = var.vpc_cidr
  vpc_name             = var.vpc_name
  cidr_public_subnet   = var.cidr_public_subnet
  eu_availability_zone = var.eu_availability_zone
  cidr_private_subnet  = var.cidr_private_subnet
}
```

### Step 2: Define Variables

Ensure the variables used in the networking module are defined in networking folder in main.tf file. 

These variables include:

**vpc_cidr:** The CIDR block for the VPC.

**vpc_name:** The name of the VPC.

**cidr_public_subnet:** The CIDR block for the public subnet.

**eu_availability_zone:** The availability zone in the EU region
.
**cidr_private_subnet:** The CIDR block for the private subnet.

#### main.tf

```hcl
variable "vpc_cidr" {}
variable "vpc_name" {}
variable "cidr_public_subnet" {}
variable "eu_availability_zone" {}
variable "cidr_private_subnet" {}

output "dev_proj_1_vpc_id" {
  value = aws_vpc.dev_proj_1_vpc_ap_south_1.id
}

output "dev_proj_1_public_subnets" {
  value = aws_subnet.dev_proj_1_public_subnets.*.id
}

output "public_subnet_cidr_block" {
  value = aws_subnet.dev_proj_1_public_subnets.*.cidr_block
}

# Setup VPC
resource "aws_vpc" "dev_proj_1_vpc_ap_south_1" {
  cidr_block = var.vpc_cidr
  tags = {
    Name = var.vpc_name
  }
}
```

### Step 3: Provide Variable Values

Provide values for these variables in a .tfvars file or through the Terraform CLI. 

#### terraform.tfvars

```hcl
bucket_name = "dev-proj-1-jenkins-remote-state-bucket-123456"

vpc_cidr             = "11.0.0.0/16"
vpc_name             = "dev-proj-jenkins-ap-south-vpc-1"
cidr_public_subnet   = ["11.0.1.0/24", "11.0.2.0/24"]
cidr_private_subnet  = ["11.0.3.0/24", "11.0.4.0/24"]
eu_availability_zone = ["ap-south-1a", "ap-south-1b"]

public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDwmgMHFJE7J4qepIzAZL3/yC6J0zsEAb/oHYL+WBBDNUjSH4TeHUnHVNe9b/pyPcub+O/HNvlGrzSxUp0xT0b3O7kkTtgBKiG8NaBbonj+c7byfOGER80DYxc5adlBltuIDd8StFe7OMzbYyUSr1mdxDTIWm/OoE39G/fu3hTqUGkykv072GAy8nMFejITRw9pf+53B9ziE5rsdOUH4uqBiQa/Ng/qKo7h9MtJGcloRATYiObXwAgrHtt3sDrtvkq2ZceT906/BJm1Twlm+BHlQecHV18Ak3bzm/6HzlsA/q+yORsoB+VxSUxvVy0nXTc1X8vJAD4KSYVL5DTrpisdnQAIcuqAbea+LMku2o4sdnrrIlUi8/8BXeVbI4TNNGd0+sWpCVcDEhb4gyA/XXTvloQyjTYrL4+am/9XEY6NGdsrPK74sjvtpUZPUrmzTJ/mJWG5ncGY88GAj+YZAsY5pnAqh2CkR2TUpglugldnWyrppbe2QyC9iQkgUGSkBTs= rahulwagh@Rahuls-MacBook-Pro.local"
ec2_ami_id = "ami-0694d931cee176e7d"
```


## Setup Subnet

**Public Subnet:** A public subnet is a subnet that is associated with a route table that has a route to an internet gateway. This means that instances launched in a public subnet can communicate directly with the internet. Public subnets are typically used for resources that need to be accessible from the internet, such as web servers.

In our project, the cidr_public_subnet variable specifies the CIDR blocks for our public subnets. These subnets will be used to host resources that need to interact with the internet, such as the Jenkins server.

```hcl
cidr_public_subnet = ["11.0.1.0/24", "11.0.2.0/24"]
```


**Private Subnet:** A private subnet is a subnet that is associated with a route table that does not have a route to an internet gateway. Instances in a private subnet cannot communicate directly with the internet. Instead, they can route their traffic through a NAT gateway or a NAT instance for internet access. Private subnets are typically used for resources that do not need to be directly accessible from the internet, such as databases or backend servers.

In our project, the cidr_private_subnet variable specifies the CIDR blocks for our private subnets. These subnets will be used to host resources that should not be directly exposed to the internet.

```hcl
cidr_private_subnet = ["11.0.3.0/24", "11.0.4.0/24"]
```

```hcl
# Setup public subnet
resource "aws_subnet" "dev_proj_1_public_subnets" {
  count             = length(var.cidr_public_subnet)
  vpc_id            = aws_vpc.dev_proj_1_vpc_ap_south_1.id
  cidr_block        = element(var.cidr_public_subnet, count.index)
  availability_zone = element(var.eu_availability_zone, count.index)

  tags = {
    Name = "dev-proj-public-subnet-${count.index + 1}"
  }
}

# Setup private subnet
resource "aws_subnet" "dev_proj_1_private_subnets" {
  count             = length(var.cidr_private_subnet)
  vpc_id            = aws_vpc.dev_proj_1_vpc_ap_south_1.id
  cidr_block        = element(var.cidr_private_subnet, count.index)
  availability_zone = element(var.eu_availability_zone, count.index)

  tags = {
    Name = "dev-proj-private-subnet-${count.index + 1}"
  }
}
```

Now we are ready to set VPC for Jenkins

Before that we will take a look at provider.tf file

```hcl
provider "aws" {
  region                   = "ap-south-1"
  shared_credentials_files = ["/Users/rahulwagh/.aws/credentials"]
}
```

# Setting Up AWS Credentials and Terraform Provider for Jenkins

## Step 1: Create AWS Credentials

1. **Sign in to AWS Management Console:**
   - Go to [AWS Management Console](https://aws.amazon.com/console/).
   - Sign in with your AWS account credentials.

2. **Navigate to Security Credentials:**

   - In the AWS Management Console, search for Security Credentials and open it.

![image](https://github.com/Nachiketa-A/Rest_API_-Project/assets/157089767/1ec10533-ba31-470c-947e-152432cc2037)

3. **Create a New IAM User:**

   - In the IAM Dashboard, click on `Users` in the left sidebar.

   - Click on `Add user`.

   - Enter a username (e.g., `terraform-user`).

   - Select `Programmatic access` to enable access via the command line and APIs.

4. **Set Permissions:**

   - Click on `Next: Permissions`.

   - Choose `Attach existing policies directly`.

   - Search for `AdministratorAccess` and select it.

   - Click `Next: Tags`, then `Next: Review`.

5. **Create User and Download Credentials:**

   - Click on `Create user`.

   - Download the `.csv` file containing the Access Key ID and Secret Access Key. Keep this file secure.


# Implementing Network module to set VPC

### Step 1:

```hcl
terraform init
```

This command is really essential to initialize modules and also to download AWS required dependancies so that terraform can provision the infrastructure .
