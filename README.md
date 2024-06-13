# Project Flow

![image](https://github.com/Nachiketa-A/Rest_API_-Project/assets/157089767/f57a94ad-8491-4c98-bc6e-f0fea9dfaa0a)


![image](https://github.com/Nachiketa-A/Rest_API_-Project/assets/157089767/5a6c67ee-8842-4f07-8798-c8400d9b2a2b)


## Jenkins setup using visual studio

### 1. First step is to set VPC using VS:

#### Cloning a Git Repository in Visual Studio

### Step-by-Step Guide

#### 1. Open Visual Studio
- Start Visual Studio from your desktop or start menu.

#### 2. Open the Start Window
- If you see the start window, click on the "Clone a repository" option.
- If you're already in a project or don't see this option, go to the next step.

#### 3. Open the Git Repository Window
- Go to the **View** menu and select **Git Repository** to open the Git Repository window.

#### 4. Clone a Repository
- In the Git Repository window, click on the **Clone** option.

#### 5. Enter Repository URL
- Enter the URL of the Git repository you want to clone. This URL can be found on your Git hosting service (e.g., GitHub, GitLab, Bitbucket).
  - Example URL: `(https://github.com/Nachiketa-A/terraform-jenkins.git)`

#### 6. Choose Local Path
- Select a local path where you want the repository to be cloned to by clicking the **Browse** button and navigating to your desired directory.

#### 7. Clone the Repository
- Click the **Clone** button to start the cloning process.
- Visual Studio will clone the repository to the specified local path.

#### 8. Open the Project
- Once the cloning process is complete, Visual Studio may automatically open the cloned repository.
- If not, you can open it manually by going to **File** > **Open** > **Project/Solution** and navigating to the cloned repository's folder.

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

variable "vpc_cidr" {}
variable "vpc_name" {}
variable "cidr_public_subnet" {}
variable "eu_availability_zone" {}
variable "cidr_private_subnet" {}

output "dev_proj_1_vpc_id" {
  value = aws_vpc.dev_proj_1_vpc_eu_central_1.id
}

output "dev_proj_1_public_subnets" {
  value = aws_subnet.dev_proj_1_public_subnets.*.id
}

output "public_subnet_cidr_block" {
  value = aws_subnet.dev_proj_1_public_subnets.*.cidr_block
}

# Setup VPC
resource "aws_vpc" "dev_proj_1_vpc_eu_central_1" {
  cidr_block = var.vpc_cidr
  tags = {
    Name = var.vpc_name
  }
}




### Step 3: Provide Variable Values

Provide values for these variables in a .tfvars file or through the Terraform CLI. 

#### terraform.tfvars

bucket_name = "dev-proj-1-jenkins-remote-state-bucket-123456"

vpc_cidr             = "11.0.0.0/16"
vpc_name             = "dev-proj-jenkins-eu-west-vpc-1"
cidr_public_subnet   = ["11.0.1.0/24", "11.0.2.0/24"]
cidr_private_subnet  = ["11.0.3.0/24", "11.0.4.0/24"]
eu_availability_zone = ["eu-west-1a", "eu-west-1b"]

public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDwmgMHFJE7J4qepIzAZL3/yC6J0zsEAb/oHYL+WBBDNUjSH4TeHUnHVNe9b/pyPcub+O/HNvlGrzSxUp0xT0b3O7kkTtgBKiG8NaBbonj+c7byfOGER80DYxc5adlBltuIDd8StFe7OMzbYyUSr1mdxDTIWm/OoE39G/fu3hTqUGkykv072GAy8nMFejITRw9pf+53B9ziE5rsdOUH4uqBiQa/Ng/qKo7h9MtJGcloRATYiObXwAgrHtt3sDrtvkq2ZceT906/BJm1Twlm+BHlQecHV18Ak3bzm/6HzlsA/q+yORsoB+VxSUxvVy0nXTc1X8vJAD4KSYVL5DTrpisdnQAIcuqAbea+LMku2o4sdnrrIlUi8/8BXeVbI4TNNGd0+sWpCVcDEhb4gyA/XXTvloQyjTYrL4+am/9XEY6NGdsrPK74sjvtpUZPUrmzTJ/mJWG5ncGY88GAj+YZAsY5pnAqh2CkR2TUpglugldnWyrppbe2QyC9iQkgUGSkBTs= rahulwagh@Rahuls-MacBook-Pro.local"
ec2_ami_id = "ami-0694d931cee176e7d"
