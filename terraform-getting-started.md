# Getting Started with Terraform

Terraform is the most popular langauge for defining and provisioning infrastructure as code (IaC). Get started by installing Terraform, creating infrastructure, then destroying it when you are done. 

## Learning Objectives
* Download and install Terraform
* Create infrastructure by creating a file in a directory, then initializing Terraform.
* Provision your resource by applying Terraform
* Destroy the infrastucture you created 

## Prerequisites
* IDE
* Command prompt
* Docker

## Installing Terraform

To install Terraform, find the appropriate package for your system and download it from [Terraform.io](https://www.terraform.io/downloads.html). 

## Creating Infrastructure

Create a new directory on your local machine to put your Terraform configuration code inside it.

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

Next, create a file for your Terraform configuration code.

```shell
$ touch main.tf
```

Paste the following lines of code into the file.

```hcl
terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
    }
  }
}
provider "docker" {
    host = "unix:///var/run/docker.sock"
}
resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "training"
  ports {
    internal = 80
    external = 80
  }
}
resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```

Initialize Terraform with the `init` command to install the AWS provider. 

```shell
$ terraform init
```

```shell
C:\Users\Sue Machtley\Documents\GitHub\https-github.com-hashicorp-interviews-education-assignments\terraform-demo>terraform init

Initializing the backend...

Initializing provider plugins...
- Reusing previous version of kreuzwerker/docker from the dependency lock file
- Using previously-installed kreuzwerker/docker v2.16.0

Terraform has been successfully initialized!

```

You shoud check for any errors. If it ran successfully, provision the resource with the `apply` command.

```shell
$ terraform apply
```
Type `yes` and hit ENTER if you see a message at the bottom of the output asking for confirmation. 

The `apply` command may take up to a few minutes to run and will display a message indicating that the resource was created.

```shell

docker_image.nginx: Creating...
docker_image.nginx: Still creating... [10s elapsed]
docker_image.nginx: Still creating... [20s elapsed]
docker_image.nginx: Still creating... [30s elapsed]
docker_image.nginx: Creation complete after 39s [id=sha256:c316d5a335a5cf324b0dc83b3da82d7608724769f6454f6d9a621f3ec2534a5anginx:latest]
docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 5s [id=9b96276ff6dc13e29515004901f758ac529d881431fe1980fe9da96fd870fb56]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

```

## Destroying Infrastructure

Finally, destroy the infrastructure.

```shell
$ terraform destroy
```
Type `yes` and hit ENTER if you see a message at the bottom of the output asking for confirmation. Terraform will then destroy the resources you created.

```shell

docker_container.nginx: Destroying... [id=9b96276ff6dc13e29515004901f758ac529d881431fe1980fe9da96fd870fb56]
docker_container.nginx: Still destroying... [id=9b96276ff6dc13e29515004901f758ac529d881431fe1980fe9da96fd870fb56, 10s elapsed]
docker_container.nginx: Destruction complete after 11s
docker_image.nginx: Destroying... [id=sha256:c316d5a335a5cf324b0dc83b3da82d7608724769f6454f6d9a621f3ec2534a5anginx:latest]
docker_image.nginx: Destruction complete after 2s

Destroy complete! Resources: 2 destroyed.
```

## Next Steps
This tutorial taught you how to install and provision infrastructure using Terraform, then destroy that infrastructure. Please visit our [Terraform learn](https://learn.hashicorp.com/terraform) page for more tutorials. 

