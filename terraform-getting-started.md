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

Initialize Terraform with the `init` command. The AWS provider will be installed. 

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

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

You shoud check for any errors. If it ran successfully, provision the resource with the `apply` command.

```shell
$ terraform apply
```

The command will take up to a few minutes to run and will display a message indicating that the resource was created.

```shell
C:\Users\Sue Machtley\Documents\GitHub\https-github.com-hashicorp-interviews-education-assignments\terraform-demo>terraform apply
docker_image.nginx: Refreshing state... [id=sha256:c316d5a335a5cf324b0dc83b3da82d7608724769f6454f6d9a621f3ec2534a5anginx:latest]
docker_container.nginx: Refreshing state... [id=7534a9dc153b205763955aa103f10a095d93456345b99cb1f598db70f0b672b7]

Note: Objects have changed outside of Terraform

Terraform detected the following changes made outside of Terraform since the last "terraform apply":

  # docker_container.nginx has been deleted
  - resource "docker_container" "nginx" {
      - attach         = false -> null
      - env            = [] -> null
      - id             = "7534a9dc153b205763955aa103f10a095d93456345b99cb1f598db70f0b672b7" -> null
      - image          = "sha256:c316d5a335a5cf324b0dc83b3da82d7608724769f6454f6d9a621f3ec2534a5a" -> null
      - logs           = false -> null
      - must_run       = true -> null
      - name           = "training" -> null
      - read_only      = false -> null
      - remove_volumes = true -> null
      - restart        = "no" -> null
      - rm             = false -> null
      - security_opts  = [] -> null
      - start          = true -> null
      - stdin_open     = false -> null
      - tty            = false -> null

      - ports {
          - external = 80 -> null
          - internal = 80 -> null
          - ip       = "0.0.0.0" -> null
          - protocol = "tcp" -> null
        }
    }


Unless you have made equivalent changes to your configuration, or ignored the relevant attributes using ignore_changes, the following plan may include actions to undo or respond to these changes.

──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach           = false
      + bridge           = (known after apply)
      + command          = (known after apply)
      + container_logs   = (known after apply)
      + entrypoint       = (known after apply)
      + env              = (known after apply)
      + exit_code        = (known after apply)
      + gateway          = (known after apply)
      + hostname         = (known after apply)
      + id               = (known after apply)
      + image            = "sha256:c316d5a335a5cf324b0dc83b3da82d7608724769f6454f6d9a621f3ec2534a5a"
      + init             = (known after apply)
      + ip_address       = (known after apply)
      + ip_prefix_length = (known after apply)
      + ipc_mode         = (known after apply)
      + log_driver       = (known after apply)
      + logs             = false
      + must_run         = true
      + name             = "training"
      + network_data     = (known after apply)
      + read_only        = false
      + remove_volumes   = true
      + restart          = "no"
      + rm               = false
      + security_opts    = (known after apply)
      + shm_size         = (known after apply)
      + start            = true
      + stdin_open       = false
      + tty              = false

      + healthcheck {
          + interval     = (known after apply)
          + retries      = (known after apply)
          + start_period = (known after apply)
          + test         = (known after apply)
          + timeout      = (known after apply)
        }

      + labels {
          + label = (known after apply)
          + value = (known after apply)
        }

      + ports {
          + external = 80
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

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
You may see a message at the bottom of the output asking for confirmation. Type `yes` and hit ENTER. Terraform will destroy the resources you created.

```shell
C:\Users\Sue Machtley\Documents\GitHub\https-github.com-hashicorp-interviews-education-assignments\terraform-demo>terraform destroy
docker_image.nginx: Refreshing state... [id=sha256:c316d5a335a5cf324b0dc83b3da82d7608724769f6454f6d9a621f3ec2534a5anginx:latest]
docker_container.nginx: Refreshing state... [id=2b658a7b51c8482b4781da275320868679615d4b7dc1449096048d35d14d44a6]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # docker_container.nginx will be destroyed
  - resource "docker_container" "nginx" {
      - attach            = false -> null
      - command           = [
          - "nginx",
          - "-g",
          - "daemon off;",
        ] -> null
      - cpu_shares        = 0 -> null
      - dns               = [] -> null
      - dns_opts          = [] -> null
      - dns_search        = [] -> null
      - entrypoint        = [
          - "/docker-entrypoint.sh",
        ] -> null
      - env               = [] -> null
      - gateway           = "172.17.0.1" -> null
      - group_add         = [] -> null
      - hostname          = "2b658a7b51c8" -> null
      - id                = "2b658a7b51c8482b4781da275320868679615d4b7dc1449096048d35d14d44a6" -> null
      - image             = "sha256:c316d5a335a5cf324b0dc83b3da82d7608724769f6454f6d9a621f3ec2534a5a" -> null
      - init              = false -> null
      - ip_address        = "172.17.0.2" -> null
      - ip_prefix_length  = 16 -> null
      - ipc_mode          = "private" -> null
      - links             = [] -> null
      - log_driver        = "json-file" -> null
      - log_opts          = {} -> null
      - logs              = false -> null
      - max_retry_count   = 0 -> null
      - memory            = 0 -> null
      - memory_swap       = 0 -> null
      - must_run          = true -> null
      - name              = "training" -> null
      - network_data      = [
          - {
              - gateway                   = "172.17.0.1"
              - global_ipv6_address       = ""
              - global_ipv6_prefix_length = 0
              - ip_address                = "172.17.0.2"
              - ip_prefix_length          = 16
              - ipv6_gateway              = ""
              - network_name              = "bridge"
            },
        ] -> null
      - network_mode      = "default" -> null
      - privileged        = false -> null
      - publish_all_ports = false -> null
      - read_only         = false -> null
      - remove_volumes    = true -> null
      - restart           = "no" -> null
      - rm                = false -> null
      - security_opts     = [] -> null
      - shm_size          = 64 -> null
      - start             = true -> null
      - stdin_open        = false -> null
      - storage_opts      = {} -> null
      - sysctls           = {} -> null
      - tmpfs             = {} -> null
      - tty               = false -> null

      - ports {
          - external = 80 -> null
          - internal = 80 -> null
          - ip       = "0.0.0.0" -> null
          - protocol = "tcp" -> null
        }
    }

  # docker_image.nginx will be destroyed
  - resource "docker_image" "nginx" {
      - id          = "sha256:c316d5a335a5cf324b0dc83b3da82d7608724769f6454f6d9a621f3ec2534a5anginx:latest" -> null
      - latest      = "sha256:c316d5a335a5cf324b0dc83b3da82d7608724769f6454f6d9a621f3ec2534a5a" -> null
      - name        = "nginx:latest" -> null
      - repo_digest = "nginx@sha256:2834dc507516af02784808c5f48b7cbe38b8ed5d0f4837f16e78d00deb7e7767" -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

docker_container.nginx: Destroying... [id=9b96276ff6dc13e29515004901f758ac529d881431fe1980fe9da96fd870fb56]
docker_container.nginx: Still destroying... [id=9b96276ff6dc13e29515004901f758ac529d881431fe1980fe9da96fd870fb56, 10s elapsed]
docker_container.nginx: Destruction complete after 11s
docker_image.nginx: Destroying... [id=sha256:c316d5a335a5cf324b0dc83b3da82d7608724769f6454f6d9a621f3ec2534a5anginx:latest]
docker_image.nginx: Destruction complete after 2s

Destroy complete! Resources: 2 destroyed.
```

## Next Steps
This tutorial taught you how to install and provision infrastructure using Terraform, then destroy that infrastructure. Please visit our [Terraform learn](https://learn.hashicorp.com/terraform) page for more tutorials. 

