# aws-subnet

Will create a subnet(s) in an existing VPC 

## Requirements

- AWS credentials and the correct permissions to create the resources
- An existing VPC with a tagged Name
- List of subnets and their location is defined in teh `subnets` variable (see example below)

## Role Variables

The variables uses in this role are

| Variable Name | Required | Description | 
|----|----|----|
| `region`| **Yes** | The region that you will deploy into |
| `vpc_name` | **Yes** | Used for identification of VPC |
| `subnets` | **Yes** | List of subnets for deployment
| `wait_timeout` | Optional | Period of time to wait for timeout <br> - Default `300` |
| `wait` | Optional | Wait for subnet to become available <br> - Default `yes` |
| `map_public` | Optional | Assign public IP addresses by default to instances <br> - Default `no` |

## Dependencies

None

## Example Playbook

### Download dependencies

#### Create requirements file

Create a `requirements.yml` file with the following contents
```
- src: https://github.com/maishsk/aws-subnet
  version: master
```

#### Download dependencies
Run the following command:
```
ansible-galaxy install -r requirements.yml --force -p .
```

### Create playbook
Create a `main.yaml` file with the following contents:
```
---
- name: Create Subnets
  hosts: localhost
  connection: local
  gather_facts: false
  vars_files:
    - vars/vars.yml

  tasks:
  - name: Create Process
    include_role:
      name: "{{ item }}"
    with_items:
      - aws-subnet
    tags: [ 'never', 'create' ]

  - name: Rollback Process
    include_role:
      name: "{{ item }}"
    with_items:
      - aws-subnet
    tags: [ 'never', 'rollback' ]
```
Create a `vars/vars.yml` with the content similar to:
```
vpc_name: maish_test
region: us-east-2
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

`ansible-playbook main.yml --tags create`

To remove the VPC

`ansible-playbook main.yml --tags rollback`

## License

BSD

## Author Information
This role was created by [Maish Saidel-Keesing](https://www.maishsk.com/), author of [The Cloud Walkabout](http://cloudwalkabout.com/).