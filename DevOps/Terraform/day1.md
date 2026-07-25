
Terraform :
- Iac (Infrastructure as Code) tool
- Open source tool by Hashicorp
- Declarative language (HCL - Hashicorp Configuration Language)
- modules and workspace 
- module : ec2 
- workspace : dev, staging, prod
- variables
- state file : terraform.tfstate
- providers : aws, azure, gcp, etc

---

## terraform block list 
- provider block --> cloud provider (aws, azure, gcp, etc) integration
- resource block --> aws service (ec2, s3, rds, etc) configuration
- variable block ---> varible declaration and default value
- output block ---> resource create --> public ip --> output values  
- data block  ---> existing resource data source (aws_ami, aws_vpc, etc)
- local block ---> touch file --> local exec | remote exec provisioner
- module block ---> reusable scripts ec2 --> project 1, project 2, project 3
- workspace block --> dev, staging, prod
- backend block ---> remote state storage (s3, gcs, etc)
- terraform block ---> required_providers, required_version

---

Terraform Lifecycle :
- terraform init --> initialize the working directory and download providers plugins
- terraform plan --> Blueprint of the changes that will be made to the infrastructure
- terraform apply --> changes apply on cloud
- terraform destroy --> destroy the infrastructure created by terraform











