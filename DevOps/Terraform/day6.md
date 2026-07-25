## VPC 

```.tf
resource "aws_vpc" "main" {
  cidr_block = "192.168.0.0/16"
  tags = {
    Name = "main"
  }
}

resource "aws_subnet" "sub1" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "192.168.0.0/22"

  tags = {
    Name = "subnet-1"
  }
}


resource "aws_subnet" "sub2" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "192.168.4.0/22"

  tags = {
    Name = "subnet-2"
  }
}

resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main.id

  tags = {
    Name = "IGW-1"
  }
}

resource "aws_eip" "eip" {
  domain = "vpc"
}

resource "aws_nat_gateway" "nat" {
  allocation_id = aws_eip.eip.id
  subnet_id     = aws_subnet.sub1.id

  tags = {
    Name = "NAT-GW"
  }
}

resource "aws_route_table" "rt" {
  vpc_id = aws_vpc.main.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.igw.id
  }

  tags = {
    Name = "public-RT"
  }
}
resource "aws_route_table_association" "tr-as" {
  subnet_id      = aws_subnet.sub1.id
  route_table_id = aws_route_table.rt.id
}


resource "aws_route_table" "rt-1" {
  vpc_id = aws_vpc.main.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_nat_gateway.nat.id
  }

  tags = {
    Name = "private-RT"
  }
}

resource "aws_route_table_association" "rt-nat" {
  subnet_id      = aws_subnet.sub2.id
  route_table_id = aws_route_table.rt-1.id
}

```



