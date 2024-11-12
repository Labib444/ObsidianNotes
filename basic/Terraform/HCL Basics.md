Terraform file type: main.tf
Resource Type: 1. Local Simple Type 2. Radapit Type
Resources have life cycle

```HCL
HCL Basics:
<block> <parameters> {
	key1 = value1
	key2 = value2
}

terraform init
terraform plan
terraform apply
terraform show

```

![[Pasted image 20241111230950.png]]
![[Pasted image 20241111231242.png]]

Update Changes:
```tf
resource "local_file" "pet"{
	filename = '/root/pets.txt'
	content = "We love pets!"
	file_permission = "0700"	
}

terraform plan
terraform apply
terraform destroy #destroys resources
```
# Providers

- terraform init can called many times without any worry

![[Pasted image 20241111232600.png]]

# Configuration Directory

- creating multiple tf files in a directory
![[Pasted image 20241111232724.png]]

```hcl
resource "local_file" "pet"{
	filename = '/root/pets.txt'
	content = "We love pets!"
	file_permission = "0700"	
}

#below resource random only prints on the terminal
resource "random_pet" "my-pet"{
	prefix = "Mrs"
	separator = "."
	length = "1"
}

terraform init (need to run this because we have added a new resource which it will download)
terraform plan
terraform apply
```

# Variables

- must be in same directory
![[Pasted image 20241111233354.png]]

![[Pasted image 20241111233433.png]]

terraform plan
terraform apply

![[Pasted image 20241111235245.png]]

### Variable Types

![[Pasted image 20241111234514.png]]
### List
```hcl
#variables.tf
variable "prefix" {
	default = ["Mr", "Mrs", "Sir"]
	type = list #list(string), list(number) 
}

#main.tf
resource "random_pet" "my-pet"{
	prefix = var.prefix[0] #or 1 and 2
	separator = "."
	length = "1"
}
```
- Sets are same just set(string), sets cannot have duplicates
### Map
```hcl
#variables.tf
variable "file-content" {
	type = map #map(string)
	default = {
		"statement1" = "1 2 3"
		"statement2" = "4 5 6"
	}
}

#main.tf
resource "random_pet" "my-pet"{
	prefix = var.file-content["statement2"]
	separator = "."
	length = "1"
}
```
### Objects
![[Pasted image 20241111234457.png]]
### Tuples 
- Tuples only expects 3 arguments all of them must be within (string, number, bool)
![[Pasted image 20241111234333.png]]

### Other ways of passing variables

- Might ask on terminal to give value
![[Pasted image 20241111235126.png]]

![[Pasted image 20241111234705.png]]

- When you have many variables
![[Pasted image 20241111234730.png]] 
![[Pasted image 20241111234833.png]]
- any other name will needed to be manually applied
![[Pasted image 20241111234903.png]]

### variables types precedence (Higher value has most priority)

![[Pasted image 20241111235044.png]]

# Resource Attributes

![[Pasted image 20241111235650.png]]

# Resource Dependencies 

![[Pasted image 20241112002404.png]]
![[Pasted image 20241112002451.png]]

# Output Variable

- terraform output (prints all output variables)
- used for debugging
- or for return data or output data for ansible and shell scripts
![[Pasted image 20241112002829.png]]
