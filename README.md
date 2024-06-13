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





