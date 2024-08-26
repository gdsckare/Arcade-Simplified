# GSP206
## Run in cloudshell
```cmd
export REGION1=
```
```cmd
export REGION2=
```
```cmd
export REGION3=
```
```cmd
git clone https://github.com/CodingWithHardik/terraform-google-lb-http.git
cd ~/terraform-google-lb-http/examples/multi-backend-multi-mig-bucket-https-lb
cat >> variables.tf <<EOF
variable "group1_region" {
  default = "$REGION1"
}

variable "group2_region" {
  default = "$REGION2"
}

variable "group3_region" {
  default = "$REGION3"
}

variable "network_name" {
  default = "ml-bk-ml-mig-bkt-s-lb"
}

variable "project" {
  type = string
}
EOF
terraform init
terraform plan -out=tfplan -var "project=$DEVSHELL_PROJECT_ID"
terraform apply tfplan
```
## Wait 5 Minutes
