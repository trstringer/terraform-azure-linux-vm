# Linux VM Provisioning with Terraform

*Quickly create a single Linux VM in Azure, with sane and secure defaults*

## Requirements

- Terraform
- Azure Subscription

## Setup and Configuration

First you need to pull down this repo if you want to run it directly (versus sourcing it from another Terraform module)...

```
git clone https://github.com/tstringer/terraform-azure-linux-vm
```

Ensure that you have Terraform installed. If you don't, you can [reference the official Terraform documentation on installing](https://www.terraform.io/intro/getting-started/install.html)...

```
which terraform
```

The Azure provider in Terraform requires the following environment variables defined...

- `ARM_SUBSCRIPTION_ID`
- `ARM_TENANT_ID`
- `ARM_CLIENT_SECRET`
- `ARM_CLIENT_ID`

## Provisioning

Navigate your terminal to this module's root directory. It's wise to first see what Terraform will do in your subscription...

```
terraform plan
```

If you are satisfied, then start the provisioning process...

```
terraform apply -var "name_prefix=linux" -var "hostname=linux$(echo $RANDOM)" -var "ssh_public_key=$(cat ~/.ssh/id_rsa.pub)"
```

> :bulb: As you can see here, there are three required variables (and only three): `name_prefix` (what to prefix your Azure resources with), `hostname` (this will be the public DNS name, recommended to randomize it to prevent likeliness of collisions), and `ssh_public_key` (your public key). To see optional variables and their defaults, take a look at `vars.tf`

## Output

After you run this Terraform module, there will be two outputs: `admin_username` and `vm_fqdn`. These two pieces are what you need to then immediately ssh into your new Linux machine.
