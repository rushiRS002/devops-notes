```.tf
resource "aws_eip" "eip" {
  domain   = "vpc"
  instance = aws_instance.ec2.id
}

resource "aws_instance" "ec2" {
  ami                    = var.ami
  instance_type          = var.ec2_type
  key_name               = var.key
  vpc_security_group_ids = [aws_security_group.sg.id]

  root_block_device {
    delete_on_termination = true
    volume_size           = 10
    volume_type           = "gp2"
  }

  tags = {
    Name = "Server1"
  }
}


resource "aws_security_group" "sg" {
  name        = "test-sg"
  description = "to test sg in terraform "
  vpc_id      = "vpc-08c40d91b76b9b4a0"

  tags = {
    Name = "test-sg-tag"
  }
}


resource "aws_vpc_security_group_ingress_rule" "inbound" {
  security_group_id = aws_security_group.sg.id

  cidr_ipv4   = "0.0.0.0/0"
  from_port   = 22
  ip_protocol = "tcp"
  to_port     = 22
}

resource "aws_vpc_security_group_ingress_rule" "inboun" {
  security_group_id = aws_security_group.sg.id

  cidr_ipv4   = "0.0.0.0/0"
  from_port   = 80
  ip_protocol = "tcp"
  to_port     = 80
}


resource "aws_vpc_security_group_ingress_rule" "inboun3" {
  security_group_id = aws_security_group.sg.id

  cidr_ipv4   = "0.0.0.0/0"
  from_port   = 81
  ip_protocol = "tcp"
  to_port     = 90
}


resource "aws_vpc_security_group_egress_rule" "out" {
  security_group_id = aws_security_group.sg.id
  cidr_ipv4          = "0.0.0.0/0"
  ip_protocol       = "-1" # semantically equivalent to all ports
}
```



## Atribute refrancing 
```
resource "aws_eip" "eip" {
  domain   = "vpc"
  instance = aws_instance.ec2.id
}

resource "aws_instance" "ec2" {
  ami                    = var.ami
  instance_type          = var.ec2_type
  key_name               = var.key
  vpc_security_group_ids = var.sg
  root_block_device {
    delete_on_termination = false
    volume_size           = 10
    volume_type           = "gp2"
  }

  tags = {
    Name = "Server1"
  }
}
```

## SG 
```.tf

resource "aws_security_group" "sg" {
  name        = "test-sg"
  description = "to test sg in terraform "
  vpc_id      = "vpc-08c40d91b76b9b4a0"

  tags = {
    Name = "test-sg-tag"
  }
}


resource "aws_vpc_security_group_ingress_rule" "inbound" {
  security_group_id = aws_security_group.sg.id

  cidr_ipv4   = "0.0.0.0/0"
  from_port   = 22
  ip_protocol = "tcp"
  to_port     = 22
}

resource "aws_vpc_security_group_ingress_rule" "inboun" {
  security_group_id = aws_security_group.sg.id

  cidr_ipv4   = "0.0.0.0/0"
  from_port   = 80
  ip_protocol = "tcp"
  to_port     = 80
}


resource "aws_vpc_security_group_ingress_rule" "inboun3" {
  security_group_id = aws_security_group.sg.id

  cidr_ipv4   = "0.0.0.0/0"
  from_port   = 81
  ip_protocol = "tcp"
  to_port     = 90
}


resource "aws_vpc_security_group_egress_rule" "out" {
  security_group_id = aws_security_group.sg.id
  cidr_ipv4          = "0.0.0.0/0"
  ip_protocol       = "-1" # semantically equivalent to all ports
}

```


## Data 
