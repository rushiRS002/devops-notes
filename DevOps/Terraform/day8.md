## Terraform Modules Revise 
## terraform backend block 
- A Terraform backend block defines where your infrastructure's state data is stored and how it is accessed.
By default, this data is saved locally, but a backend block allows you to store state securely in a shared, remote location like AWS S3 or Google Cloud Storage.

## Storing `terraform.tfstate` on a Remote Location
### Why Store State Remotely?
- Storing the state file remotely enables team collaboration.
- Provides state locking to prevent conflicts during simultaneous updates.
- Secures sensitive data stored in the state file.

- https://developer.hashicorp.com/terraform/language/backend

```backend.tf
# state.tf
terraform {
  backend "s3" {
    bucket = "" 
    key    = ""
    region = ""
    profile= ""
  }
}
```
```example.tf
terraform {
  backend "s3" {
    bucket = "awsbackends3blocktestkaraychaahe" 
    key    = "terraform.tfstate"
    region = "ap-northeast-1"
  }
}
```

## Terraform workspcae Theory 
- https://developer.hashicorp.com/terraform/cli/workspaces

---

# “multi-environment with .tfvars” 
You keep one set of Terraform code (resources, modules, variables) and switch values (CIDRs, sizes, tags, etc.) via per-environment .tfvars files. That way you don’t duplicate code—only the inputs change.
# EC2 Multi-Environment Terraform Project

```bash
ec2-multi-env/
├─ main.tf
├─ variables.tf
├─ workspace/
│  ├─ dev.tfvars
│  ├─ stage.tfvars
│  └─ prod.tfvars
```

### 1) main.tf
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "my_ec2" {
  ami           = "ami-08c40ec9ead489470"   # Amazon Linux 2 AMI (example, region-specific)
  instance_type = var.instance_type

  tags = {
    Name = "ec2-${terraform.workspace}"
    Env  = terraform.workspace
  }
}
```
### 2) variables.tf
```hcl
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
}
```


### 3) Workspace-specific .tfvars
 - workspace/dev.tfvars
```hcl
instance_type = "t2.micro"
```

- workspace/stage.tfvars
```hcl
instance_type = "t3.small"
```
- workspace/prod.tfvars
```hcl
instance_type = "t3.medium"
```
### 4) Run Commands
- Initialize once
```sh
terraform init
```
- Create and select workspaces
```sh
terraform workspace new dev
terraform workspace new stage
terraform workspace new prod
```
- Apply for dev
```sh
terraform workspace select dev
terraform apply -var-file="workspace/dev.tfvars"
```
- Apply for stage
```sh
terraform workspace select stage
terraform apply -var-file="workspace/stage.tfvars"
```
- Apply for prod
```sh
terraform workspace select prod
terraform apply -var-file="workspace/prod.tfvars"
```
