# Sysdig Workstation

Cookbooks and Packer config to create a workstation for use during Sysdig workshops. Thanks to the Chef team for letting us copy [their code](https://github.com/chef-cft/habitat_workstation).

## Current Amazon Machine Image IDs

AMIs are currently only available in `us-east` region. Make sure you have a keypair setup in that region.
*NOTE*: Ensure your `~/.aws/config` region is set to `us-east-1` pending region abstraction.

An `~/.aws/config` file might look like:

```
[default]
region=us-east-1
aws_access_key_id = MYKEYID
aws_secret_access_key = MYACCESSKEY
```

Platform     | AMI         
----         | ------      
CentOS 7     | ami-d9281aa3
Ubuntu 16.04 | ami-822d1ff8
Ubuntu 17.10 | ami-f3e2d089

## Pre-requisites

```
# The name of the AWS KeyPair (your SSH key) to use for deployment
export AWS_KEYPAIR_NAME='your_aws_keypair_name'
```

## Build the Amazon Machine Images (AMIs)

This rake task updates the vendored cookbooks before building the AMIs.

`$ rake cookbook:vendor`

### To create a single AMI 

`$ rake "ami:build[centos-7,none]"`

### To build all AMIs

`$ rake "ami:build_all"`

### List available templates

```
$ rake list:templates
centos-7
ubuntu-1604
ubuntu-1714
```

## Share the AMIs with other Amazon accounts

The `subscribers.yml` is a YAML file with account IDs to share an image with. 

```bash
$ export AMI_ID=the_ami_id_generated_by_packer
$ rake ami:share
```

## Deploy the CloudFormation stack

This deploys a CloudFormation stack with the number of hosts and a TTL in days.
A template will be created in `stacks/` with the arguments above using values from
`config.yml`.  An example, `config.example.yml`, is included

```bash
# rake deploy[name,num_hosts,ttl]

$ export AMI_ID=the AMI_ID #(generated by the rake task "ami:build above)
$ rake "deploy[sysdigcts-stack,10,3]"
```

```
$ cat config.example.yml
---
contact: Your Name Here 
dept: Marketing
project: Sysdig Workshops
region: us-east-1
sg: sg-57fa1720
subnet: subnet-dae6d691
type: t2.medium
```

## List Workstation IPs for a CloudFormation stack

```bash
$ rake "list:ips[sysdigcts-stack]"
workstation1: 54.172.141.159
workstation2: 54.87.195.76
workstation3: 54.91.122.177
workstation4: 54.86.46.187
workstation5: 54.227.190.60
```
