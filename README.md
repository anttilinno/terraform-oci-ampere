![Ampere Computing](https://avatars2.githubusercontent.com/u/34519842?s=400&u=1d29afaac44f477cbb0226139ec83f73faefe154&v=4)

# terraform-oci-ampere

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

## Original Source
https://github.com/AmpereComputing/terraform-oci-ampere-tensorflow

## Description

Terraform code to launch a Ampere A1 Shape on Oracle Cloud Infrastructure (OCI) Free-Tier.

## Requirements

 * [Terraform](https://www.terraform.io/downloads.html)
 * [Oracle OCI "Always Free" Account](https://www.oracle.com/cloud/free/#always-free)

## What exactly is Terraform doing

The goal of this code is to supply the minimal ammount of information to quickly have working Ampere A1 instances on OCI ["Always Free"](https://www.oracle.com/cloud/free/#always-free).

Addtional tasks performed by this code:

* Create compartment defined in tfvars. All resources are under this compartment.
* Dynamically creating sshkeys to use when logging into the instance.
* Dynamically getting region, availability zone and image id..
* Creating necessary core networking configurations for the tenancy
* Launch 1 Ampere A1 instance with 24GB RAM, 4 Cores, and pass in rendered metadata 
* Output IP information to connect to the instance.

To get started clone this repository from GitHub locally.

## Configuration with terraform.tfvars

The easiest way to configure is to use a terraform.tfvars in the project directory.  
Please note that Compartment OCID are the same as Tenancy OCID for Root Compartment.
The following is an example of what terraform.tfvars should look like:

```
tenancy_ocid =
user_ocid = 
fingerprint = 
private_key_path =
compartment_name =
```

### Running Terraform

```
terraform init && terraform plan && terraform apply -auto-approve
```

### Log into created system

```
terraform show -json | jq -r '.values.root_module.resources[] | select(.name == "oci-ssh-privkey") | .values.content' > ~/.ssh/id_rsa && chmod go-r ~/.ssh/id_rsa
ssh -i ~/.ssh/id_rsa ubuntu@
```

## References

* [Original repo, what was used as base](https://github.com/AmpereComputing/terraform-oci-ampere-tensorflow)
* [Terraform provider configuration](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/terraformproviderconfiguration.htm)
* [Where to Get the Tenancy's OCID and User's OCID](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/apisigningkey.htm#five)
* [API Key Authentication](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/terraformproviderconfiguration.htm#APIKeyAuth)
* [Instance Principal Authorization](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/terraformproviderconfiguration.htm#instancePrincipalAuth)
* [Security Token Authentication](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/terraformproviderconfiguration.htm#securityTokenAuth)
* [How to Generate an API Signing Key](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/apisigningkey.htm#two)
* [Bootstrapping a VM image in Oracle Cloud Infrastructure using Cloud-Init](https://martincarstenbach.wordpress.com/2018/11/30/bootstrapping-a-vm-image-in-oracle-cloud-infrastructure-using-cloud-init/)
* [Oracle makes building applications on Ampere A1 Compute instances easy](https://blogs.oracle.com/cloud-infrastructure/post/oracle-makes-building-applications-on-ampere-a1-compute-instances-easy?source=:ow:o:p:nav:062520CloudComputeBC)
* [scross01/oci-linux-instance-cloud-init.tf](https://gist.github.com/scross01/5a66207fdc731dd99869a91461e9e2b8)
* [scross01/autonomous_linux_7.7.tf](https://gist.github.com/scross01/bcd21c12b15787f3ae9d51d0d9b2df06)
* [Oracle Cloud Always Free](https://www.oracle.com/cloud/free/#always-free)
