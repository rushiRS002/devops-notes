## Data block 
```
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"] # Canonical's official AWS Account ID

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd-gp3/ubuntu-noble-24.04-amd64-server-*"]
  }
}
```
## terraform block version 
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}
```

## provider alias 
```
provider "<PROVIDER_NAME>" {
  <PROVIDER_ARGUMENTS>
  alias   = "<ALIAS_NAME>"
  version = "<VERSION_CONSTRAINT>"  # Deprecated
}
```

## data type 
# Terraform 
### Understanding the terraform data types 
- String
- Number
- Boolean
- List  
- Map
- Set
- null
- any 


## example of using data types in terraform 

- string 
```hcl
variable "ami_id" {
  description = "The ID of the AMI to use for the instance"
  type        = string  
}   

```
- number
```hcl
variable "instance_count" {
  description = "The number of instances to create"
  type        = number  
}
```

- boolean 
```hcl
variable "enable_monitoring" {
    description = "Whether to enable monitoring for the instance"
    type        = boolean
}
```
- list 
```hcl
variable "availability_zones" {
  description = "A list of availability zones to deploy the instances in"
    type        = list(string)
}
```

- map 
```hcl
variable "instance_tags" {
  description = "A map of tags to apply to the instance"
    type        = map(string)
}
```
- map (number)
```hcl
variable "instance_counts" {
  description = "A map of instance types to the number of instances to create"
    type        = map(number)
}
```
- set
```hcl
variable "security_group_ids" {
  description = "A set of security group IDs to associate with the instance"
    type        = set(string)
}
``` 
- null : the null data type represents the absence of a value. 
```hcl
variable "optional_variable" {
  description = "An optional variable that can be set to null"
  type        = string
  default     = null
}
```
- any : when you dont know the type of value which is requred
```hcl
variable "tf-any" {
description = "when you dont know the type of values"
type = any
default = {
u1 = "shubham"
u2 = ["mukund","anup"]
u3 = 90
u4 = {
     alpha = "alia"
     cbz = "atul"
     kothrud = {
          team = "cdec"
         }
      }
   }
}
```





