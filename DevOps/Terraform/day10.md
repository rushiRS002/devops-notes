```.tf
provider "aws" {
  region = "ap-northeast-1"
}

resource "aws_instance" "web" {
  ami           = "ami-0f8faa29480e7e6de"
  instance_type = "t3.micro"
  key_name = "tokyo"
  provisioner "local-exec" {
    command = "touch sushant_sir"
  }

  provisioner "file" {
    source      = "nginx.sh"
    destination = "/home/ubuntu/nginx.sh"
  }

  connection {
    type        = "ssh"
    user        = "ubuntu"
    private_key = file("tokyo")
    host        = self.public_ip
  }
  provisioner "remote-exec" {
    inline = [
      "chmod +x /home/ubuntu/nginx.sh",
      "bash /home/ubuntu/nginx.sh"
    ]
  }
  tags = {
    Name = "WebServer"
  }
}

```
