# aws-vpc

Will create a VPC in the specified AWS region. This role will also attach an Internet Gateway to the VPC.

## Requirements

None

## Role Variables

The variables uses in this role are

| Variable Name | Required | Description | 
|----|----|----|
| `region`| **Yes** | The region that you will deploy into |
| `vpc_cidr_block` | **Yes** | The CIDR block of VPC  | 
| `vpc_name` | **Yes** | The name of the VPC | 
| `enable_dns_support` | Optional | Enable DNS Support<br>   - Default `true` | 
| `enable_dns_hostnames` | Optional | Enable AWS DNS support<br>   - Default `true` |
| `vpc_multi_ok` | Optional | Create new VPC if another VPC with the same name and CIDR block exists? <br>   - Default `no` |
| `vpc_tenancy` | Optional | Default or dedicated tenancy<br>   - Default `default` |

## Dependencies

None

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```
---
- name: Create a VPC
  hosts: localhost
  connection: local
  gather_facts: false
  vars_files:
    - vars/vars.yml
  roles:
    - aws-vpc
```

And `vars/vars.yml` contains

```
vpc_name: vpc_test
vpc_cidr_block: 192.168.100.0/24
region: us-east-2
tags:
 - foo: bar
 - blah: meh
 - ami: 3syllables
```

## Running the playbook

To create the VPC

`ansible-playbook main.yml -e "create=true"`

To remove the VPC

`ansible-playbook main.yml -e "rollback=true"`

## License

BSD

## Author Information
This role was created by [Maish Saidel-Keesing](https://www.maishsk.com/), author of [The Cloud Walkabout](http://cloudwalkabout.com/).