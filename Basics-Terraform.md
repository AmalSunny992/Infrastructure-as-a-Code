# Writing Terraform Configurations

## Objectives
- Learn how to write Terraform configurations
- Understand how to create and manage resources with Terraform
- Get acquainted with the Terraform Registry and how to utilize it

## Writing Terraform Configurations

### Structure of a Terraform Configuration
Terraform configurations are written in HCL (HashiCorp Configuration Language) and typically consist of one or more .tf files. Each file can contain various types of blocks that define providers, resources, variables, outputs, and modules.

**Example Configuration**
- Here’s an example of a basic Terraform configuration to create an AWS EC2 instance:

```hcl
provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

### Key Components
**Providers:** Define the cloud provider or service (e.g., AWS, Azure, GCP) and configuration details.

**Resources:** Define the actual infrastructure components you want to manage (e.g., virtual machines, databases, networking).

**Variables:** Allow you to parameterize your configuration for reusability and flexibility.

**Outputs:** Allow you to expose certain values from your Terraform configuration for use in other configurations or as a reference.

### Creating and Managing Resources

1. Define the Provider
    - To interact with a specific cloud provider, you need to define the provider block with the necessary configuration.

```hcl
provider "aws" {
  region = "us-west-2"
}
```

2. Define Resources
    - Resources are the building blocks of your infrastructure. Define each resource block with the necessary configuration to create or manage that resource.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

3. Initialize the Configuration
    - Before applying your configuration, you need to initialize the working directory using terraform init. This command downloads the necessary provider plugins.

```sh
terraform init
```

4. Plan the Changes
    - Use the terraform plan command to create an execution plan. This shows you what actions Terraform will take to achieve the desired state defined in your configuration.

```sh
terraform plan
```

5. Apply the Configuration
    - Use the terraform apply command to execute the actions proposed in the plan and apply your configuration.

```sh
terraform apply
```
6. Destroy Resources
    - If you need to remove the resources managed by Terraform, use the terraform destroy command.

```sh
terraform destroy
```

## Utilizing Terraform Registry
The Terraform Registry is a centralized repository of Terraform modules and providers. It allows you to discover, use, and share modules created by the community or by HashiCorp.

- Finding Modules
    - Visit the Terraform Registry to search for modules that meet your needs. You can browse by categories, providers, or specific use cases.

- Using Modules from the Registry
    - Search for a Module
    - Search for a module that you want to use. For example, a VPC module for AWS.

- Add the Module to Your Configuration
    - Copy the module source and version information from the registry and add it to your configuration.

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "2.64.0"

  name = "my-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["us-west-2a", "us-west-2b", "us-west-2c"]
  public_subnets  = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  private_subnets = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]

  enable_nat_gateway = true
  single_nat_gateway = true
}
```

- Initialize and Apply
    - Initialize the configuration to download the module and then apply it.

```sh
terraform init
terraform apply
```
## Example: Creating a VPC with a Module
Here’s a complete example of how to use a VPC module from the Terraform Registry:

```hcl
provider "aws" {
  region = "us-west-2"
}

module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "2.64.0"

  name = "my-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["us-west-2a", "us-west-2b", "us-west-2c"]
  public_subnets  = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  private_subnets = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]

  enable_nat_gateway = true
  single_nat_gateway = true
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = module.vpc.public_subnets[0]
}
```

## Summary
In this lesson, you learned how to write Terraform configurations, create and manage resources, and utilize the Terraform Registry to find and use reusable modules. By leveraging Terraform's powerful configuration language and the wealth of community-contributed modules, you can efficiently manage and scale your infrastructure.