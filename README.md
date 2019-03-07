# aws-subnet

Will create a subnet(s) in an existing VPC 

## Requirements

An existing VPC.

## Role Variables

The variables uses in this role are

| Variable Name | Required | Description | 
|----|----|----|
| `region`| **Yes** | The region that you will deploy into |
| `vpc_id` | **Yes** | The Id of the VPC | 
| `subnet_cidr_block` | **Yes** | The CIDR block of the Subnet  | 
| `subnets` | **Yes** | List of subnets for deployment
| `vpc_name` | Optional | Used for tagging purposes |
| `wait_timeout` | Optional | Period of time to wait for timeout <br> - Default `300` |
| `wait` | Optional | Wait for subnet to become available <br> - Default `yes` |
| `map_public` | Optional | Assign public IP addresses by default to instances <br> - Default `no` |

## Dependencies

None

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```
---
- name: Create Subnets
  hosts: localhost
  connection: local
  gather_facts: false
  vars_files:
    - vars/vars.yml
  roles:
    - aws-subnets
```

And `vars/vars.yml` contains

```
vpc_name: maish_test
region: us-east-2
vpc_id: vpc-077c143fcf318b68c
subnets:
  - subnet_name: "{{ vpc_name | default (omit) }}-Public-{{ region }}a"
    subnet_cidr: 192.168.100.0/26
    subnet_az: "{{ region }}a"
    subnet_map_public: yes
  - subnet_name: "{{ vpc_name | default (omit) }}-Public-{{ region }}b"
    subnet_cidr: 192.168.100.64/26
    subnet_az: "{{ region }}b"
    subnet_map_public: yes
  - subnet_name: "{{ vpc_name | default (omit) }}-Private-{{ region }}a"
    subnet_cidr: 192.168.100.128/26
    subnet_az: "{{ region }}a"
  - subnet_name: "{{ vpc_name | default (omit) }}-Private-{{ region }}b"
    subnet_cidr: 192.168.100.192/26
    subnet_az: "{{ region }}b"
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