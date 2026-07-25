## data types 
- The Terraform language uses the following types for its values:
```
string: a sequence of Unicode characters representing some text, like "hello".
number: a numeric value. The number type can represent both whole numbers like 15 and fractional values like 6.283185.
bool: a boolean value, either true or false. bool values can be used in conditional logic.
list (or tuple): a sequence of values, like ["us-west-1a", "us-west-1c"]. Identify elements in a list with consecutive whole numbers, starting with zero.
set: a collection of unique values that do not have any secondary identifiers or ordering.
map (or object): a group of values identified by named labels, like {name = "Mabel", age = 52}.
```
```.tf
provider "aws" {
  region = "ap-northeast-1"
}

resource "aws_instance" "ec2" {
  ami                         = var.ami
  instance_type               = var.instance_type
  key_name                    = var.key_name
  associate_public_ip_address = true
  vpc_security_group_ids      = var.sg
  user_data                   = <<EOF
#!/bin/bash 
sudo -i 
apt update 
apt install nginx -y 
systemctl start nginx 
systemctl enable nginx 
echo "this is server created from terraform " > /var/www/html/index.html 
EOF 
  tags = {
    Name = "server1"
  }
}

variable "ami" {
  default = "ami-0126975fb247bf2e7"
}
variable "instance_type" {
  default = "t3.micro"
}

variable "key_name" {
  default = "keynes"
}

variable "sg" {
  default = ["sg-0dc56f3814abc50b4"]

}


output "server_id" {
value = aws_instance.ec2.public_ip
}

```















