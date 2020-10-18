---
title: Automate UCS Manager configuration with Ansible
date: "2019-03-07T23:46:37.121Z"
description: Ansible playbooks for configuring Cisco UCS Manager.  
thumbnail: " "
--- 

## Getting Started
The Cisco Unified Computing System (UCS) is an architecture data center server platform composed of computing hardware, virtualization support, switching fabric, and management software. 

Management of the system devices is handled by the Cisco UCS Manager software embedded into the Fabric Interconnect, which is accessed by the administrator through a common browser, a command line interface like Windows PowerShell or programmatically through an API. 

Don´t panic if there is no UCS Manager around. Some of the examples below are executed upon Cisco Devnet sandboxes: https://developer.cisco.com/site/sandbox/ 

![](./cisco-devnet-sandboxes.png)

Once your lab is reserved, the VPN credentials are sent by email as shown below:

![](./cisco-devnet-details.png)

Before connecting from your local laptop to the Devnet sandbox, you have to set up the VPN connection with the details provided above. 

![](./cisco-anyconnect-vpn.png)

In order to interwork with UCS Manager, there is a Python dependency required. The "ucsmsdk" must be installed:

![](./ansible-module-dependencies.png)

For testing purposes, I have created the inventory file below including the UCS Manager credentials. 

![](./ansible-inventory.png)

The following playbook configures an address pool in UCS. 

![](./ansible-playbook.png)

Running the playbook: 
```bash
ansible-playbook create-pool-yaml -i inventory -vvv
```
![](./ansible-playbook-execution.png)

You can establish an SSH connection towards UCS Manager and check that the pool is created: 

![](./ansible-validate-changes.png)

Similarly, the following playbook creates a VLAN. 

![](./ansible-playbook-create-vlan.png)

Running the playbook: 

![](./ansible-playbook-execution-vlan.png)

Validating that the VLAN is created: 

![](./ansible-validate-changes-vlan.png)

