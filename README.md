# tf-012-autoscaling-events-demo

Creates demo stack with Oracle Cloud Infrastructure (OCI) using Terraform 0.12 which creates an autoscaling demo environment and sends Notifications through Events Service after you have subscribed to the Notification topic with your email (this needs to be done manually).

Running this creates following resources:


* Compartment
* Virtual Cloud Network (VCN)
* Nat Gateway
* Internet Gateway
* Public load balancer and a backend set which has instance pool as destination
* Two public subnets for the load balancer (ports 22 and 80 are open)
* One private subnet for the instance pool instances (ports 22 and 80 are open)
* Public & Private routetables - Public RT will have a route to Internet Gateway and Private RT route to NAT Gateway
* Public & Private securitylists - Both allow traffic to ports 22 and 80 only. By default they allow traffic from any source but this can be modified to allow only traffic from CIDR block deemed necessary
* One compute instance with the smallest shape to act as a jump server and a 7.6 linux image - the instance public IP will be displayed in the end. 
* Notification topic in the Notification service - you need to subscribe to it manually with your email!
* Events Rule with action on autoscaling and instance events which then uses the Notification topic to send information.


## Requirements and install instructions

1. Valid OCI account to install these components
2. Download these .tf files
3. Set following environment variables to your environment:

* TF_VAR_tenancy_ocid - Your tenancy OCID
* TF_VAR_user_ocid - Your user OCID which you are connecting to OCI
* TF_VAR_fingerprint - Fingerprint for your key found from user details
* TF_VAR_private_key_path - Path to your private key on your machine
* TF_VAR_region - region which you are using

4. Modify variables.tf and edit variable "ssh_public_key" to have your own SSH key defined.

5. Running Terraform:

* terraform init
* terraform plan !! Remember to review the plan !!
* terraform apply

6. Review the public IP of compute instance and the private IP's for compute instance. You can use the private ssh key and *opc* user to login to these instances

## Removal of stack

In case you want to remove created resources:

* terraform destroy

## Additional notes

You can freely change the variables in the variables.tf depending what you need. One could potentially scale down the database shape, open different ports in security list or change database version. Try and test!

Thanks for [Stephen Cross](https://gist.github.com/scross01/bcd21c12b15787f3ae9d51d0d9b2df06) for the filtering of OCI images using specific OS version. 




