---
title: Build an Ansible custom module in 15 minutes
date: "2019-09-11T18:40:32.169Z"
layout: post
description: A quick introduction on Ansible custom modules. 
---

## Getting Started
The list is of Ansible modules is extensive (see Related section below), but it might be the case where you have automate a task, which is not supported by Ansible out of the box. 

This post covers the basics of developing your own Ansible module. 
I will use Python to write a module which multiplies two given numbers. Let´s start!


## Folder Structure
Ansible requires that the module is stored at /library/custom_module_name.py at the same level where the playbook is located. 
In the following example, the playbook is called "main.yaml" and the customer module "multiply.py" is located in the /library/ folder.  

![]({{ site.baseurl }}/images/module-folder-tree.png)

Ansible recommends to include strings for DOCUMENTATION and EXAMPLES at the top of our module code.

```bash
DOCUMENTATION = '''
---
module: multiply
short_description: Multiply two given numbers
'''

EXAMPLES = '''
tasks:
- multiply:
    a: 200
    b: 10
  register: result
- debug: var=result
'''
```

The key part is to import the boilerplate code from ansible.module_utils.basic like this:

```python 
from ansible.module_utils.basic import AnsibleModule
if __name__ == '__main__':
    main()

```

The AnsibleModule provides lots of common code for handling returns, parses your arguments for you, and allows you to check inputs.

![]({{ site.baseurl }}/images/custom-module.png)

Let's run the playbook and check the result:

![]({{ site.baseurl }}/images/playbook-execution.png)


## Related

* [Ansible Modules](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html) - List of Ansible modules
