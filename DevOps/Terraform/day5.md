## ALB

```.tf 
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

variable "subid" {
  default = ["subnet-0b906851905b1f699", "subnet-0954717fa29d6bdc2", "subnet-0aa120e14c735770a"]
}

resource "aws_lb_target_group" "tg1" {
  name     = "test-tg"
  port     = 80
  protocol = "HTTP"
  vpc_id   = data.aws_vpc.default.id
  health_check {
    path                = "/"
    protocol            = "HTTP"
    matcher             = "200-399" # FIXED
    interval            = 30
    timeout             = 5
    healthy_threshold   = 2
    unhealthy_threshold = 2
  }
}

resource "aws_lb_target_group_attachment" "lb-tg" {
  target_group_arn = aws_lb_target_group.tg1.arn
  target_id        = aws_instance.ec2.id
  port             = 80
}


resource "aws_lb_listener" "alb-ls" {
  load_balancer_arn = aws_lb.lb.arn
  port              = 80
  protocol          = "HTTP"

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.tg1.arn
  }
}


resource "aws_lb" "lb" {
  name               = "alb-lb"
  internal           = false
  load_balancer_type = "application"
  subnets            = var.subid
  security_groups    = var.sg

  enable_deletion_protection = false

  tags = {
    Name = "app-lb"

  }
}

## output block 
output "lb_dns_name" {
  value = aws_lb.lb.dns_name
}

```
## data block used for vpc refrancing 
```.tf
data "aws_vpc" "default" {
  default = true
}

output "vpc" {
  value = data.aws_vpc.default.id
}
```

---

# Terraform Script (Load Balancer)

```hcl
provider "aws" {
  region = "ap-south-1"
}

# variable
variable "subnet_ids" {
  type = list(string)
}

variable "security_group_ids" {
  type = list(string)
}

variable "vpc_id" {
  type = string
}

# Load Balancer (ALB)
resource "aws_lb" "example" {
  name               = "example-lb"
  internal           = false
  load_balancer_type = "application"
  subnets            = var.subnet_ids
  security_groups    = var.security_group_ids

  enable_deletion_protection = false

  tags = {
    Name = "example-lb"
  }
}

## Target 
resource "aws_lb_target_group" "example" {
  name     = "example-tg"
  port     = 80
  protocol = "HTTP"
  vpc_id   = var.vpc_id

  health_check {
    path                = "/"
    protocol            = "HTTP"
    matcher             = "200-399"   # FIXED
    interval            = 30
    timeout             = 5
    healthy_threshold   = 2
    unhealthy_threshold = 2
  }
}

resource "aws_lb_target_group_attachment" "example" {
  target_group_arn = aws_lb_target_group.example.arn
  target_id        = aws_instance.example.id
  port             = 80
}

# Listener
resource "aws_lb_listener" "example" {
  load_balancer_arn = aws_lb.example.arn
  port              = 80
  protocol          = "HTTP"

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.example.arn
  }
}

# Output
output "lb_dns_name" {
  value = aws_lb.example.dns_name
}
```
```.tfvars
# Subnet IDs (at least 2 subnets required for ALB)
subnet_ids = [
  "subnet-1",
  "subnet-2"
]

# Security Group IDs
security_group_ids = [
  "sg-0123abcd4567efgh8"
]

# VPC ID
vpc_id = "vpc-0abc1234abcd56789"
```

---
## 1. aws_lb (Load Balancer Resource)

```hcl
resource "aws_lb" "example"
```

* Used to create **Application or Network Load Balancer**
* Important arguments:

  * `name` → LB name
  * `internal` → internet-facing or internal
  * `load_balancer_type` → `application` or `network`
  * `subnets` → required for LB placement
  * `security_groups` → required for ALB

### As per docs, `aws_lb` is the main resource for managing load balancers.

---

## 2. aws_lb_target_group

```hcl
resource "aws_lb_target_group" "example"
```

* Defines group of backend servers (EC2, IPs, etc.)
* Required for routing traffic
* Key attributes:

  * `port`
  * `protocol`
  * `vpc_id`

Target groups are used by load balancers to route requests to backend resources.

---

## 3. aws_lb_listener

```hcl
resource "aws_lb_listener" "example"
```

* Listens for incoming traffic
* Routes requests to target group
* Requires:

  * `load_balancer_arn`
  * `port`
  * `protocol`

Listener defines how traffic is handled and forwarded. 
---

# Architecture Flow

```
Client → Load Balancer → Listener → Target Group → EC2 Instances
```

---

* `aws_lb` replaces old `aws_alb` (same functionality) 
* Load balancer requires:

  * Subnets (for networking)
  * Security groups (for ALB)
* Listener + Target Group are mandatory for routing traffic
* ALB works at Layer 7 (HTTP/HTTPS routing)
---

## Summary

* `aws_lb` → creates load balancer
* `aws_lb_target_group` → backend servers
* `aws_lb_listener` → traffic routing
* Flow: LB → Listener → Target Group
* Always attach listener, otherwise LB is useless

---

# ASG 

# Terraform Script (ASG)

```hcl id="asg-heredoc"

provider "aws" {
  region = "ap-south-1"
}

# Launch Template
resource "aws_launch_template" "example" {
  name_prefix   = "example-template"
  image_id      = "ami-12345678" # Use valid Ubuntu AMI
  instance_type = "t2.micro"

  vpc_security_group_ids = ["sg-123456"]

  user_data = base64encode(<<EOF
#!/bin/bash
sudo apt update -y
sudo apt install nginx -y
systemctl start nginx
systemctl enable nginx
sudo echo " this si test server" > /var/www/html/index.html
EOF
  )
}

# Auto Scaling Group
resource "aws_autoscaling_group" "example" {
  name                = "example-asg"
  min_size            = 1
  max_size            = 3
  desired_capacity    = 2
  vpc_zone_identifier = ["subnet-123", "subnet-456"]

  launch_template {
    id      = aws_launch_template.example.id
    version = "$Latest"
  }

  health_check_type         = "EC2"
  health_check_grace_period = 300

  tag {
    key                 = "Name"
    value               = "nginx-server"
    propagate_at_launch = true
  }
}
```

---

### 1. HEREDOC + base64encode

* AWS expects `user_data` in **base64 encoded format**
* That’s why:

```hcl
user_data = base64encode(<<EOF ... EOF)
```

---

### 2. Ubuntu Requirement

Your script uses:

```bash
apt install nginx
```

So you must use an **Ubuntu AMI**, not Amazon Linux.

---

# Key Interview Points

* Use **HEREDOC** for multi-line scripts
* Always use **base64encode** with launch template
* Ensure OS compatibility (apt vs yum)
* Launch Template + ASG is modern approach
* User data runs only at instance launch

---

## ASG 
- aws asg requirements 
1. lanch template --> 6 steps 
2. Auto Scaling Group 


1. Launch Template:

```.tf
resource "aws_launch_template" "launch_v01" {
  name_prefix   = "ASG-template"
  image_id      = var.image_id
  instance_type = var.instance_type
  key_name      = var.key_pair
  vpc_security_group_ids = var.security_group_ids

  tags = {
    ENV = "DEV"
  }
# HEREDOC for user data script
  user_data = base64encode(<<EOF
#!/bin/bash
sudo apt update -y
sudo apt install nginx -y
systemctl start nginx
systemctl enable nginx
sudo echo " this is test server $HOSTNAME " > /var/www/html/index.html
EOF
  )
}
```
```variable.tf

variable "image_id" {
  type = string
}

variable "instance_type" {
  type = string
}

variable "security_group_ids" {
  type = list(string)
}

variable "key_pair" {
  type = string
}

```


2. Auto Scaling Group:

```.tf
resource "aws_autoscaling_group" "asg_v01" {
  name                = "ASG-Group"
  min_size            = var.min_size
  max_size            = var.max_size
  desired_capacity    = var.desired_capacity
  vpc_zone_identifier = var.subnet_ids 

  launch_template {
    id      = aws_launch_template.launch_v01.id
    version = "$Latest"
  }

  health_check_type         = "EC2"
  health_check_grace_period = 300

  tag {
    key                 = "Name"
    value               = "ASG-Instance"
    propagate_at_launch = true
  }
}
```

```variable.tf
variable "subnet_ids" {
  type = list(string)
}

variable "min_size" {
  type = number
}

variable "max_size" {
  type = number
}

variable "desired_capacity" {
  type = number
}

```


```.tf
resource "aws_autoscaling_policy" "asg-home" {
  name                   = "asg-home-policy"
  autoscaling_group_name = aws_autoscaling_group.asg_v01.name
  policy_type            = "TargetTrackingScaling"
  target_tracking_configuration {
    predefined_metric_specification {
    predefined_metric_type = "ASGAverageCPUUtilization"
  }
  target_value = 50.0
}
}

```


```.tfvars
image_id = "ami-05d2d839d4f73aafb" # Use valid Ubuntu AMI
instance_type = "t3.micro"
key_pair = "project"
security_group_ids = ["sg-0f0cc734a754b1d15"]  
subnet_ids = ["subnet-062164aa169b88fb9", "subnet-0c4b90ebc6b0b3e79"]
min_size = 1
max_size = 3
desired_capacity = 2
vpc_id = "vpc-07e3d60b0c1f6f6e6"


```

---


```app.tf

provider "aws" {
  region = "ap-south-1"
}

############################
# VARIABLES
############################

variable "image_id" {}
variable "instance_type" {}
variable "key_pair" {}
variable "security_group_ids" { type = list(string) }
variable "subnet_ids" { type = list(string) }
variable "vpc_id" {}
variable "min_size" {}
variable "max_size" {}
variable "desired_capacity" {}

############################
# LAUNCH TEMPLATE
############################

resource "aws_launch_template" "launch_v01" {
  name_prefix   = "ASG-template"
  image_id      = var.image_id
  instance_type = var.instance_type
  key_name      = var.key_pair

  vpc_security_group_ids = var.security_group_ids

  user_data = base64encode(<<EOF
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx
echo "This is test server $(hostname)" > /var/www/html/index.html
EOF
  )

  tag_specifications {
    resource_type = "instance"

    tags = {
      Name = "ASG-Instance"
    }
  }
}

############################
# LOAD BALANCER
############################

resource "aws_lb" "alb_v01" {
  name               = "alb-v01"
  internal           = false
  load_balancer_type = "application"
  security_groups    = var.security_group_ids
  subnets            = var.subnet_ids

  tags = {
    Name = "ALB"
  }
}

############################
# TARGET GROUP
############################

resource "aws_lb_target_group" "tg_v01" {
  name     = "tg-v01"
  port     = 80
  protocol = "HTTP"
  vpc_id   = var.vpc_id

  health_check {
    path                = "/"
    protocol            = "HTTP"
    interval            = 30
    timeout             = 5
    healthy_threshold   = 2
    unhealthy_threshold = 2
  }
}

############################
# LISTENER
############################

resource "aws_lb_listener" "listener_v01" {
  load_balancer_arn = aws_lb.alb_v01.arn
  port              = 80
  protocol          = "HTTP"

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.tg_v01.arn
  }
}

############################
# AUTO SCALING GROUP
############################

resource "aws_autoscaling_group" "asg_v01" {
  name                = "ASG-Group"
  min_size            = var.min_size
  max_size            = var.max_size
  desired_capacity    = var.desired_capacity
  vpc_zone_identifier = var.subnet_ids

  launch_template {
    id      = aws_launch_template.launch_v01.id
    version = "$Latest"
  }

  target_group_arns = [aws_lb_target_group.tg_v01.arn]

  health_check_type         = "ELB"
  health_check_grace_period = 300

  depends_on = [aws_lb_listener.listener_v01]

  tag {
    key                 = "Name"
    value               = "ASG-Instance"
    propagate_at_launch = true
  }
}

############################
# SCALING POLICY
############################

resource "aws_autoscaling_policy" "asg_policy" {
  name                   = "asg-cpu-policy"
  autoscaling_group_name = aws_autoscaling_group.asg_v01.name
  policy_type            = "TargetTrackingScaling"

  target_tracking_configuration {
    predefined_metric_specification {
      predefined_metric_type = "ASGAverageCPUUtilization"
    }
    target_value = 50.0
  }
}

############################
# OUTPUT
############################

output "alb_dns" {
  value = aws_lb.alb_v01.dns_name
}

```


```.tfvars
image_id = "ami-05d2d839d4f73aafb" # Use valid Ubuntu AMI
instance_type = "t3.micro"
key_pair = "project"
security_group_ids = ["sg-0f0cc734a754b1d15"]  
subnet_ids = ["subnet-062164aa169b88fb9", "subnet-0c4b90ebc6b0b3e79"]
min_size = 1
max_size = 3
desired_capacity = 2
vpc_id = "vpc-07e3d60b0c1f6f6e6"

```



















