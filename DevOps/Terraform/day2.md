## Terraform installation 
- https://developer.hashicorp.com/terraform/install#linux
```
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform

```

- "+" resource to be creted
- "-" resorce to  be deleted
- "~" resource to be chnage

## Provider block 
- terraform block will give you information about which cloud we are using provider of cloud is specified here

```.tf
provider "aws" {
region = "ap-south-1"
}
```

## Terraform Block 
- resource block will give you an information about which cloud resources we are creating and maintaining

```.tf
resource "aws_instance" "tag" {
  ami                         = "ami-01a18c38ece67e620"
  instance_type               = "t3.micro"
  key_name                    = "new"
  subnet_id                   = "subnet-09f8bbbd0bcb46b95"
  associate_public_ip_address = true

  tags = {
    Name = "server-test"
    name = "test-tag"
    env  = "dev"
  }

}
```
- terraform refrance page
- https://registry.terraform.io

```.tf
resource "<resource_type>" "<resource_name>" {
  key = value
}
```



```bash
terraform init    ## intitlize terraform in currnt dir
```


```bash
terraform plan   ## gives blueprint of resources which are going to be create
```

```bash
terraform validate  ## validate the code
```

```bash
terraform fmt   ## gives allingment to terraform.tf files
```

```bash
terraform apply  ## apply the changes req. approval 
```
```bash
terraform apply -auto-approve    ## apply the chnages and approve them
```
```bash
terraform destroy -auto-approve   ## apply the chnages and approve them
```

## terraform lifecycle 
- terraform init - initialize the terraform config
- terraform plan - blueprint of infra after apply
- terraform apply - apply the changes in terraform configuration
- terraform destroy - delete the terraform configuration which are created with the help of terrafrom







