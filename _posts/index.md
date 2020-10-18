---
title: Ansible ServiceNow Integration
date: "2018-08-29T22:40:32.169Z"
description: Automating workflows with ServiceNow and Ansible API.
---

## Introduction
ServiceNow https://www.servicenow.com is an Information Technology Service Management suite, which provides five major services:
- IT
- Security
- HR Service Delivery
- Customer Service
- Business Applications. 

ServiceNow is an integrated cloud solution which combines all these services in a single system of record.
In this post, I am integrating Ansible with ServiceNow in order to automate complex workflows.

## Creating the workflow
In order to run AWX playbooks from ServiceNow you have to create the Ansible AWX endpoint and a workflow as shown below 

#### Create ServiceNow AWX endpoint
The workflow is just running a "hello world" playbook but it can trigger any playbook available. 

![](./servicenow-workflow.png)


#### Order your request
![](./servicenow-order.png)

#### AWX Playbook is executed 
![](./servicenow-awx.png)


## Related

* [Ansible Modules](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html) - List of Ansible modules
* [ServiceNow Sandbox](https://developer.servicenow.com/app.do#!/home) - Request ServiceNow sandbox

