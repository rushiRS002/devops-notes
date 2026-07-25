- Explain Loops (count, for-each, for)
## Count 
- The count meta-argument allows you to create multiple instances of a resource based on a specified count value.
- want to create and Ec2 instance 30 with identical ami id and instance type baesd on exact same configuration 


```hcl
resource "aws_instance" "example" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  count         = 30
}
```
- In this example, the aws_instance resource will be created 30 times, with each instance having the same AMI and instance type. 
## Count Index 
```.tf
resource "aws_iam_user" "user" {
  name  = "dev-${count.index}"
  count = 50
}
```


## For_each
- The for_each meta-argument allows you to create multiple instances of a resource based on a specified collection, such as a list or a map.


Each Object :
1. each.key : maps key (set of members) correspondings to this instances
2. each.value : maps value (set of attributes) corresponding to this instance

## Terraform for_each 

`for_each` is used when we want to create **multiple resources using a loop**.

```hcl
variable "my-server" {
    default = { 
        key = "value"
        key1= "value1"
    
    }
}
```

## Example: Create Multiple EC2 Instances

### main.tf

```hcl
resource "aws_instance" "example" {
  for_each = {
    server1 = "t2.micro"
    server2 = "t2.small"
  }

  ami           = "ami-12345678"
  instance_type = each.value

  tags = {
    Name = each.key
  }
}
```
```bash
terraform plan
```

## 3. for
- **Definition**: The `for` expression is used to transform or filter collections.
- **Usage**: Commonly used in variables and outputs.

#### Example:
```hcl
variable "names" {
  default = ["Mukund", "ANUP", "suju"]
}

output "uppercase_names" {
  value = [for name in var.names : upper(name)]
}
```
This outputs the names in uppercase: `["Mukund", "Anup", "suju"]`.

#### Filtering with `for`:
```hcl
output "filtered_names" {
  value = [for name in var.names : name if length(name) > 4]
}
```
This filters names longer than three characters.

---
## Files 
```.tf
resource "aws_iam_user" "iam" {
  for_each = {
    user1 = "Atul"
    user2 = "Tushar"
  }
  name = each.value
}

output "user" {
  value = [for name, user in aws_iam_user.iam : upper(user.name)]
}
```
```.tf
resource "aws_instance" "ec2" {
   for_each = {
    server1-ubuntu = "ami-0126975fb247bf2e7"
    server2-amazon = "ami-0aae00de4a3cf9639"
  }
  ami           = each.value
  instance_type = "t3.micro"
  tags = {
    Name = each.key
  }
}
```
---

## Terraform Commands

### 1. **Taint Command**
The `taint` command marks a resource for recreation during the next `terraform apply`. This is useful when a specific resource needs to be replaced without altering the rest of the infrastructure.

#### **Syntax:**
```bash
terraform taint <resource_name>.<tag>
```

#### **Example:**
```bash
terraform taint aws_instance.my_instance
```
This marks the `aws_instance.my_instance` resource for recreation.


## Terraform command 
### `terraform taint` command --> `terraform replace` command command is used v0.15.2
- if you have made lots of manual changes to terraform /delete / recrete resources
- `terraform apply -replace "aws_instance.my-ec2"` --> to distroy and create an new ec2 instance of privious version that terraform had 

---

### 2. **Import Command**
The `import` command allows importing existing infrastructure resources into Terraform state. This is helpful when managing resources created outside of Terraform.

#### **Syntax:**
```bash
terraform import <resource_type>.<resource_name> <resource_id>
```

#### **Example:**
```bash
terraform import aws_instance.my_instance i-qwe12345
```
This imports the AWS EC2 instance with ID `i-qwe12345` into Terraform as `aws_instance.my_instance`.

---

### 3. **Destroy Command**
The `destroy` command removes all resources defined in the configuration.

#### **Targeted Destroy (-t)**
You can destroy specific resources using the `-target` flag.

#### **Syntax:**
```bash
terraform destroy -target=<resource_type>.<resource_name>
```









