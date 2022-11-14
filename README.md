# Terraform Lab - Install Terraform

In this lab you will learn how to install the Terraform CLI. You will also use Docker locally to check that Terraform is installed correctly.

Lab based on instructions at https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli

## Install Terraform

Terraform can be installed either [manually or through package managers](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli#install-terraform). We will do a manual install.

We will need to:

1. Download the latest Terraform zip archive
1. Unzip the archieve
1. Add the `terraform` binary to a directory in your `PATH`.

Precompiled binaries are available on the [Terraform download page](https://developer.hashicorp.com/terraform/downloads). Download the latest version of Terraform. You can use the following command. This will use `curl` to download the latest version (v1.3.4 at time of writing) and save the file as `terraform.zip`

```sh
curl "https://releases.hashicorp.com/terraform/1.3.4/terraform_1.3.4_linux_amd64.zip" -o "terraform.zip"
```

Unzip the zip archive.

```sh
unzip terraform.zip
```

We need to add the `terraform` binary to our `PATH`. (These are the locations Linux will look for programs when you type commands). You can check your `PATH` locations using the command `echo $PATH`. Make sure `/usr/local/bin/` is in the list.

Move the `terraform` binary to `usr/local/bin/`:

```sh
sudo mv ./terraform /usr/local/bin/
```

Finally, we don't need the zip archive anymore so we can delete it:

```sh
rm terraform.zip
```

### Verify Installation

Finally, we want to check that the installation worked. Use the following command to check the install version:

```sh
terraform --version
```

The output should be something similar to:

```sh
Terraform v1.3.4
on linux_amd64
```

## Quick Start Tutorial

We will check Terraform funcationally further by provisioning an NGINX server (a popular web server engine) using Docker. We will cover what the content and commands mean in later labs.

Create a file called `main.tf` and copy the following content into it:

```terraform
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 2.23.0"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "tutorial"
  ports {
    internal = 80
    external = 8000
  }
}
```

Initialise the project. This will download the plugin that allows Terraform to interact with Docker.

```sh
terraform init
```

Provision the NGINX server container with `terraform apply`. When prompted, comfirm by typing `yes` and pressing `ENTER`.

```sh
terraform apply
```

If using Codespaces, you may get a pop-up informing you of that ports have been automatically opened. Click on `Open in Broswer`. You should see the NGINX welcome page.

When ready to finish, stop the container using `terraform destroy`.

```sh
terraform destroy
```

:tada: Well done. You installed Terraform and used it to provision an NGINX web server.