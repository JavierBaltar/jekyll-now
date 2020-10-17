---
layout: post
title: Installing AWX on Centos7
date: "2019-01-24T22:40:32.169Z"
description: Host an AWX (Ansible Tower) server on a Google Cloud VM.
---
## Introduction
AWX https://github.com/ansible/awx provides a web-based user interface, REST API, and task engine built on top of Ansible. It is the upstream project for Tower, a commercial derivative of AWX. 

## AWX Installation
The goal of this post is to install AWX in a CentOS virtual machine hosted on Google Cloud. 


Firts step is to set the SELinux to permissive mode. 
```bash
[root@awx ~]$ sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31

#Edit SELINUX to "permissive"
[root@awx ~]$ cat /etc/selinux/config

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=permissive
# SELINUXTYPE= can take one of three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected. 
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted 

```
Secondly, enable EPEL repository as shown below: 
```bash

[root@awx ~]$ sudo yum install -y epel-release
Loaded plugins: fastestmirror
Determining fastest mirrors
epel/x86_64/metalink                                                                                                                                             |  13 kB  00:00:00     
 * base: mirror.fileplanet.com
 * epel: mirror.layeronline.com
 * extras: repos-lax.psychz.net
 * updates: mirror.fileplanet.com
base                                                                                                                                                             | 3.6 kB  00:00:00     
epel                                                                                                                                                             | 4.9 kB  00:00:00     
extras                                                                                                                                                           | 3.4 kB  00:00:00     
google-cloud-sdk/signature                                                                                                                                       |  454 B  00:00:00     
google-cloud-sdk/signature                                                                                                                                       | 1.4 kB  00:00:00 !!! 
google-compute-engine/signature                                                                                                                                  |  454 B  00:00:00     
google-compute-engine/signature                                                                                                                                  | 1.4 kB  00:00:00 !!! 
updates                                                                                                                                                          | 3.4 kB  00:00:00     
(1/9): epel/x86_64/updateinfo                                                                                                                                    | 1.5 MB  00:00:00     
(2/9): base/7/x86_64/group_gz                                                                                                                                    | 166 kB  00:00:00     
(3/9): epel/x86_64/group_gz                                                                                                                                      |  88 kB  00:00:00     
(4/9): google-cloud-sdk/primary                                                                                                                                  | 100 kB  00:00:00     
(5/9): google-compute-engine/primary                                                                                                                             | 3.6 kB  00:00:00     
(6/9): epel/x86_64/primary_db                                                                                                                                    | 6.7 MB  00:00:00     
(7/9): extras/7/x86_64/primary_db                                                                                                                                | 200 kB  00:00:01     
(8/9): updates/7/x86_64/primary_db                                                                                                                               | 5.0 MB  00:00:01     
(9/9): base/7/x86_64/primary_db                                                                                                                                  | 6.0 MB  00:00:05     
google-cloud-sdk                                                                                                                                                                705/705
google-compute-engine                                                                                                                                                             10/10
Package epel-release-7-11.noarch already installed and latest version
Nothing to do
[jbaltar25@awx ~]$ 
```


AWX relies on posgreSQL as the backend database so you have to install it: 
```bash
yum install -y https://download.postgresql.org/pub/repos/yum/9.6/redhat/rhel-7-x86_64/pgdg-centos96-9.6-3.noarch.rpm
yum install postgresql96-server -y
```

Install the AWX additional packages. 
```bash
yum install -y rabbitmq-server wget memcached nginx ansible
```

Enable AWX repository
```bash
wget -O /etc/yum.repos.d/awx-rpm.repo https://copr.fedorainfracloud.org/coprs/mrmeee/awx/repo/epel-7/mrmeee-awx-epel-7.repo
--2019-01-11 12:04:34--  https://copr.fedorainfracloud.org/coprs/mrmeee/awx/repo/epel-7/mrmeee-awx-epel-7.repo
```

## Install AWX 
```bash
yum install -y awx
```

#### Initialize posgreSQL
```bash
$ /usr/pgsql-9.6/bin/postgresql96-setup initdb
Initializing database ... OK
```

#### Initialize Rabbitmq
```bash
[jbaltar25@awx ~]$ sudo systemctl start rabbitmq-server
[jbaltar25@awx ~]$ sudo systemctl enable rabbitmq-server
Created symlink from /etc/systemd/system/multi-user.target.wants/rabbitmq-server.service to /usr/lib/systemd/system/rabbitmq-server.service.

```


#### Initialize posgreSQL service
```bash
[jbaltar25@awx ~]$ sudo systemctl enable postgresql-9.6
Created symlink from /etc/systemd/system/multi-user.target.wants/postgresql-9.6.service to /usr/lib/systemd/system/postgresql-9.6.service.
[jbaltar25@awx ~]$ sudo systemctl start postgresql-9.6

```



#### Initialize Memcached
```bash
[jbaltar25@awx ~]$ sudo systemctl enable memcached
Created symlink from /etc/systemd/system/multi-user.target.wants/memcached.service to /usr/lib/systemd/system/memcached.service.

```
#### Create database and user
```bash
#Ignore errors

[jbaltar25@awx ~]$ sudo -u postgres createuser -S awx
could not change directory to "/home/jbaltar25": Permission denied
[jbaltar25@awx ~]$ sudo -u postgres createdb -O awx awx
could not change directory to "/home/jbaltar25": Permission denied
```






#### Import Data to the database
```bash
[jbaltar25@awx ~]$ sudo -u awx /opt/awx/bin/awx-manage migrate
Operations to perform:
  Apply all migrations: auth, conf, contenttypes, main, oauth2_provider, sessions, sites, social_django, sso, taggit
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying taggit.0001_initial... OK
  Applying taggit.0002_auto_20150616_2121... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0001_initial... OK
  Applying main.0001_initial... OK
  Applying main.0002_squashed_v300_release... OK
  Applying main.0003_squashed_v300_v303_updates... OK
  Applying main.0004_squashed_v310_release... OK
  Applying conf.0001_initial... OK
  Applying conf.0002_v310_copy_tower_settings... OK
  Applying main.0005_squashed_v310_v313_updates... OK
  Applying main.0006_v320_release... OK
```

#### Initialize AWX configuration
```bash
echo "from django.contrib.auth.models import User; User.objects.create_superuser('admin', 'root@localhost', 'password')" | sudo -u awx /opt/awx/bin/awx-manage shell
[root@localhost ~]# sudo -u awx /opt/awx/bin/awx-manage create_preload_data
Default organization added.
Demo Credential, Inventory, and Job Template added.
(changed: True)
[root@localhost ~]# sudo -u awx /opt/awx/bin/awx-manage provision_instance --hostname=$(hostname)
Successfully registered instance localhost.localdomain
(changed: True)
2019-01-11 11:14:43,068 DEBUG    awx.main.dispatch publish awx.main.tasks.apply_cluster_membership_policies(047f737a-4f3e-48c6-9b90-7e9cf40fc4bf, queue=awx_private_queue)
[root@localhost ~]# sudo -u awx /opt/awx/bin/awx-manage register_queue --queuename=tower --hostnames=$(hostname)
2019-01-11 11:15:13,889 DEBUG    awx.main.dispatch publish awx.main.tasks.apply_cluster_membership_policies(f994f4f4-73b3-458f-8d74-d25ec77fb7af, queue=awx_private_queue)
Creating instance group tower
2019-01-11 11:15:13,917 DEBUG    awx.main.dispatch publish awx.main.tasks.apply_cluster_membership_policies(55edd072-140a-4bff-972b-d11c69b27c81, queue=awx_private_queue)
Added instance localhost.localdomain to tower
(changed: True)
[root@localhost ~]#

[jbaltar25@awx ~]$ sudo sudo -u awx /opt/awx/bin/awx-manage create_preload_data
Default organization added.
Demo Credential, Inventory, and Job Template added.
(changed: True)
[jbaltar25@awx ~]$ sudo -u awx /opt/awx/bin/awx-manage provision_instance --hostname=$(hostname)
Successfully registered instance awx
(changed: True)
2019-05-30 11:48:30,535 DEBUG    awx.main.dispatch publish awx.main.tasks.apply_cluster_membership_policies(19890f59-cb8b-4ef4-aa76-6dfa1ae4bd9e, queue=awx_private_queue)
[jbaltar25@awx ~]$ sudo -u awx /opt/awx/bin/awx-manage register_queue --queuename=tower --hostnames=$(hostname)
2019-05-30 11:49:00,097 DEBUG    awx.main.dispatch publish awx.main.tasks.apply_cluster_membership_policies(ce90ae83-84dc-415e-9197-7aac49d68bef, queue=awx_private_queue)
Creating instance group tower
2019-05-30 11:49:00,165 DEBUG    awx.main.dispatch publish awx.main.tasks.apply_cluster_membership_policies(36986ed8-2854-4935-af9f-53723ee2f677, queue=awx_private_queue)
Added instance awx to tower
(changed: True)
[jbaltar25@awx ~]$ 

```

#### Configure NGINX
```bash
jbaltar25@awx ~]$ cd /etc/nginx/
[jbaltar25@awx nginx]$ cp nginx.conf nginx.conf.bkp
cp: cannot create regular file ‘nginx.conf.bkp’: Permission denied
[jbaltar25@awx nginx]$ sudo cp nginx.conf nginx.conf.bkp
[jbaltar25@awx nginx]$ sudo wget -O /etc/nginx/nginx.conf https://raw.githubusercontent.com/sunilsankar/awx-build/master/nginx.conf
--2019-05-30 11:51:01--  https://raw.githubusercontent.com/sunilsankar/awx-build/master/nginx.conf
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.0.133, 151.101.64.133, 151.101.128.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.0.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2621 (2.6K) [text/plain]
Saving to: ‘/etc/nginx/nginx.conf’

100%[==============================================================================================================================================>] 2,621       --.-K/s   in 0s      

2019-05-30 11:51:01 (29.3 MB/s) - ‘/etc/nginx/nginx.conf’ saved [2621/2621]

[jbaltar25@awx nginx]$ sudo systemctl start nginx
[jbaltar25@awx nginx]$ sudo systemctl enable nginx
Created symlink from /etc/systemd/system/multi-user.target.wants/nginx.service to /usr/lib/systemd/system/nginx.service.
[jbaltar25@awx nginx]$ 
```

#### Start AWS services
```bash

jbaltar25@awx nginx]$ sudo systemctl start awx-cbreceiver
[jbaltar25@awx nginx]$ sudo systemctl start awx-channels-worker
[jbaltar25@awx nginx]$ sudo systemctl start awx-daphne
[jbaltar25@awx nginx]$ sudo systemctl start awx-web
[jbaltar25@awx nginx]$ sudo systemctl enable awx-cbreceiver
Created symlink from /etc/systemd/system/multi-user.target.wants/awx-cbreceiver.service to /usr/lib/systemd/system/awx-cbreceiver.service.
[jbaltar25@awx nginx]$ sudo systemctl enable awx-channels-worker
Created symlink from /etc/systemd/system/multi-user.target.wants/awx-channels-worker.service to /usr/lib/systemd/system/awx-channels-worker.service.
[jbaltar25@awx nginx]$ sudo systemctl enable awx-daphne
Created symlink from /etc/systemd/system/multi-user.target.wants/awx-daphne.service to /usr/lib/systemd/system/awx-daphne.service.
[jbaltar25@awx nginx]$ sudo systemctl enable awx-web
Created symlink from /etc/systemd/system/multi-user.target.wants/awx-web.service to /usr/lib/systemd/system/awx-web.service.
[jbaltar25@awx nginx]$ 
```


#### Access AWX URL

http://ip_or_fqdn/#/login

Default user: admin Pass:password 

## API
This section offers a basic understanding of the REST API used by AWX and Ansible Tower
REST APIs provide access to resources (data entities) via URI paths. 
- https://docs.ansible.com/ansible-tower/2.3.0/html/towerapi/intro.html

You can visit the AWX REST API in a web browser at http://<AWX Server IP>/api/ as shown below:
  
![](./awx-api.png)

As an example, the following curl command retrieves the list of AWX Job templates provisioned
```bash
export CREDENTIAL='admin:password'
curl -s  -k  -u $CREDENTIAL "http://AWX-IP/api/v2/job_templates/" | jq '.results | .[] | .name '
"IOS Change mgcp call agent"
"Retrieve IOS Running Config to File"
```
Similarly, list the AWX inventories 
```bash
curl -s -k -u $CREDENTIAL http://AWX-IP/api/v2/inventories/ | jq '.results | .[] | .name'

"Customer Webservers"
"Customer Databases"
```
Create a new AWX user
```bash
curl -H "Content-type: application/json" -d "$(jo username=jbaltar first_name=Javier last_name=Baltar email=jbaltar@mydomain.com password=dontshareit)" -u $CREDENTIAL http://AWX-IP/api/v2/users/
```

## Notifications
AWX notifications provide a mechanism of signaling when AWX jobs succeed or fail. This can take the form of sending a message to a Slack channel, an email or sending an HTTP POST to another service to trigger other actions.
In AWX the following notification types are supported:
- Email
- Slack
- Hipchat
- Pagerduty
- Twilio
- IRC
- Webhook (POST)

![](./awx-notifications.png)
