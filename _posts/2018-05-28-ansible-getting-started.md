---
title: Ansible Getting Started
date: "2018-05-28T22:40:32.169Z"
description: Getting started on Ansible Engine features.
---

This post includes some bits around Ansible Engine fundamentals. 

## Inventory
Intentory file location

`$ /etc/ansible/hosts`

### Format

```yaml
[cisco-ios-devices]
10.10.0.1
10.10.0.2

[cisco-nxos-devices]
10.10.20.1
10.10.20.2
```

### Variables

```yaml
[cisco-ios-devices]
10.10.0.1
10.10.0.2

[cisco-ios-devices:vars]
ansible_port=22
ansible_user=netadmin
```

### Grouping

```yaml
[cisco-ios-devices]
10.10.0.1
10.10.0.2

[cisco-nxos-devices]
10.10.20.1
10.10.20.2

[network-devices:children]
cisco-ios-devices
cisco-nxos-devices
```

### Patterns

```yaml
[cisco-nexus-7000]
nexus[01:04].companydomain.com
```
Generates the output below

```yaml 
nexus01.companydomain.com
nexus02.companydomain.com
nexus03.companydomain.com
nexus04.companydomain.com
```

## Playbooks

### Handlers

```yaml
---
- hosts: webservers

  tasks: 
    - name: Install Nginx
      apt: pkg=nginx state=installed update_cache=true
      notify:
        - Start Nginx

  handlers:
    - name: Start Nginx
      service: name=nginx state=started

```

## Roles

A role directory structure contains the directories below. Each directory must contain a main.yml 

- Defaults: default variables for the role
- Vars: variables for the role
- Tasks: the main list of steps to be executed by the role
- Files: contains files which we want to be transferred to the host
- Templates: file template which supports modifications from the role
- Meta: contains metadata of role such as dependencies
- Handlers: handlers which can be invoked by notify directives

## Templates

```yaml
- hosts: webservers
  vars:
    variable_to_be_replaced: 'value1'
    inline_variable: 'value2'
  tasks:
    - name: Ansible using templates
      template:
        src: template_example.j2
        dest: /userA/docs/personal_data.txt
```
Template file
```
template_example.j2

{{ variable_to_be_replaced }}
Variable given as inline - {{ inline_variable }} -
```
Output
```
value1
Variable given as inline - value2 -
```

## Loops
### With_items

```yaml
- name: Remove users from the system.
  user:
    name: "{{ item }}"
    state: absent
    remove: yes
  with_items:
    - userA
    - userB
```

### With_nested

Define variables
```yaml
users_with_items:
  - name: "userA"
    personal_directories:
      - "old_files"
      - "maps"
      - "usb_files"
  - name: "userB"
    personal_directories:
      - "old_files"
      - "backup"
      - "storage"

common_directories:
  - "docs"
  - "media"
 ```
 
 ```yaml
 - name: Create common users directories using
  file:
    dest: "/home/{{ item.0.name }}/{{ item.1 }}"
    owner: "{{ item.0.name }}"
    group: "{{ item.0.name }}"
    state: directory
  with_nested:
    - "{{ users_with_items }}"
    - "{{ common_directories }}"
```

## Useful Commands
### List group nodes

`# ansible group-name --list-hosts`

## Modules
### Files

#### Archive

```yaml
- name: Backup Directory /var/log/application01/
  hosts: webservers
  tasks:
   - archive:
      path: /var/log/application01/
      dest: "/var/backups/application01-{{ ansible_date_time.date }}.tgz"

```

#### Copy

```yaml
- name: Copy File
  hosts: webserver
  tasks:
   - copy:
      src: /var/log/application01/filename
      dest: "/var/backups/filename
      owner: root
      group: root
      mode: u=r,g=r,o=

```

#### Fetch

```yaml
- name: Copy File from Remote Node
  hosts: webserver
  tasks:
   - fetch:
      src: /var/log/application01/filename
      dest: "/var/backups/
```

#### Delete a File

```bash
#Delete the file /backups/tmp/nodelist.txt on all servers
ansible all -b -m file -a "state=absent path=/backups/tmp/nodelist.txt"
```

#### Update a Line in File

```bash
#Update the line of text "MY_SETTING" to "BLUE" in /opt/configuration.txt on all servers
ansible all -b -m lineinfile -a "regexp=MY_SETTING line=BLUE path=/opt/configuration.txt"
```

### Notifications

#### Slack
```yaml
- name: Sending message to Slack Channel
    slack:
      token: '{{ slack_token }}'
      channel: "#companynameAnsible"
      domain: "companyname.slack.com"
      parse: "full"
      color: "good"
      msg: 'The changes is completed on {{ inventory_hostname }}.'
 ```

#### Twilio

```yaml
- name: Send an SMS to multiple phone numbers when the change is completed
    twilio:
      msg: The configuration change is completed!
      account_sid: XXXXXXXXXXX
      auth_token: XXXXXXXXXXX
      from_number: +34XXXXXXXXX
      to_number:
        - +34XXXXXXXX1
        - +34XXXXXXXX2
    delegate_to: localhost
```

## Related

* [Ansible Modules](https://docs.ansible.com/ansible/latest/modules/modules_by_category.html) - List of Ansible modules
 
