# Introduction to Terraform

## Objectives
- Understand what Terraform is and its purpose
- Learn about Terraform's architecture and how it operates

## What is Terraform?
Terraform is an open-source infrastructure as code (IaC) software tool created by HashiCorp. It enables users to define and provision infrastructure using a high-level configuration language known as HashiCorp Configuration Language (HCL), or optionally JSON. Terraform can manage both low-level components like compute instances, storage, and networking, as well as high-level components such as DNS entries and SaaS features.

### Key Features of Terraform
**Declarative Configuration:** Terraform uses a declarative language to describe the desired state of your infrastructure. You define what you want, and Terraform takes care of how to achieve that state.

**Version Control:** Since Terraform configurations are code, they can be versioned, shared, and collaborated on using version control systems like Git.

**Execution Plans:** Terraform generates an execution plan that shows what actions will be taken to reach the desired state, allowing you to review changes before applying them.

**Resource Graph:** Terraform builds a dependency graph to determine the correct order of resource creation, updates, and deletions.

**State Management:** Terraform maintains state files to track the current state of infrastructure, allowing it to detect changes and update resources efficiently.

## Terraform Architecture
Terraform's architecture consists of several key components that work together to manage infrastructure as code:

**1. Configuration Files**
Terraform configuration files are written in HCL or JSON. These files define the desired state of your infrastructure using blocks, arguments, and expressions. Configuration files are typically organized into modules, each responsible for a specific part of the infrastructure.

**Example configuration file (main.tf):**

```hcl
provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

**2. Providers**
Providers are responsible for interacting with the APIs of various infrastructure platforms and services. Each provider is configured with the necessary credentials and settings to manage resources. Terraform supports a wide range of providers, including AWS, Azure, Google Cloud, Kubernetes, and many more.

**Example of configuring a provider:**

```hcl
provider "aws" {
  region = "us-west-2"
}
```

**3. Resources**
Resources are the fundamental building blocks of your infrastructure. Each resource block in the configuration file defines a specific piece of infrastructure, such as a virtual machine, a database, or a network component. Resources can be created, updated, or deleted based on the desired state defined in the configuration.

**Example of defining a resource:**

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

**4. Modules**
Modules are reusable sets of Terraform configuration files that encapsulate a specific part of the infrastructure. They allow you to organize and reuse code across multiple projects and environments. Modules can be published and shared through the Terraform Registry or custom repositories.

**Example of using a module:**

```hcl
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  version = "2.64.0"

  name = "my-vpc"
  cidr = "10.0.0.0/16"
}
```

**5. State**
Terraform maintains a state file that tracks the current state of your infrastructure. The state file is crucial for Terraform to know what resources already exist, what needs to be created, updated, or deleted, and to map the real-world infrastructure to your configuration. State files can be stored locally or remotely (e.g., in an S3 bucket).

**6. Terraform CLI**
The Terraform CLI is the command-line interface used to interact with Terraform. It provides various commands to manage the lifecycle of your infrastructure, including initialization, validation, planning, applying, and destroying resources.

### Basic Commands

1. **`terraform init`**
   - Initializes a working directory containing Terraform configuration files.
   - Downloads necessary provider plugins.

2. **`terraform plan`**
   - Creates an execution plan, showing what actions Terraform will take to achieve the desired state as described in the configuration files.
   - Helps in understanding what Terraform will do before making any changes.

3. **`terraform apply`**
   - Applies the changes required to reach the desired state of the configuration.
   - It will execute the actions proposed in the `terraform plan`.

4. **`terraform destroy`**
   - Destroys the Terraform-managed infrastructure.
   - This command is used to clean up resources when they are no longer needed.

### Resource Management

1. **`terraform import`**
   - Imports existing infrastructure into your Terraform state.
   - Useful for bringing existing resources under Terraform management.

2. **`terraform taint`**
   - Marks a Terraform-managed resource as tainted, forcing it to be destroyed and recreated on the next apply.
   - Used when a resource needs to be replaced.

3. **`terraform untaint`**
   - Removes the 'tainted' state from a resource, so it will not be destroyed and recreated on the next apply.

### State Management

1. **`terraform state list`**
   - Lists resources in the Terraform state.

2. **`terraform state show [resource]`**
   - Shows detailed information about a single resource in the state file.

3. **`terraform state mv [source] [destination]`**
   - Moves a resource from one location to another within the state file.

4. **`terraform state rm [resource]`**
   - Removes a resource from the state file without destroying the infrastructure.

### Output and Debugging

1. **`terraform output`**
   - Reads an output variable from the state file and prints it.

2. **`terraform refresh`**
   - Updates the state file with the real-world status of resources.
   - Reconciles the state file with the real infrastructure.

3. **`terraform validate`**
   - Validates the configuration files in a directory, ensuring they are syntactically valid and internally consistent.

4. **`terraform fmt`**
   - Reformats configuration files to a canonical format and style.

### Workspaces

1. **`terraform workspace list`**
   - Lists all available workspaces.

2. **`terraform workspace new [name]`**
   - Creates a new workspace.

3. **`terraform workspace select [name]`**
   - Switches to a different workspace.

4. **`terraform workspace delete [name]`**
   - Deletes a workspace.

## Summary
In this lesson, you learned what Terraform is, its key features, and its architecture. Terraform's declarative approach, combined with its powerful execution plans, resource graph, and state management, makes it a robust tool for managing infrastructure as code.