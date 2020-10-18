---
title: Trend Micro Deep Security 
date: "2019-10-10T21:32:37.121Z"
layout: post
description: Detect and protect against vulnerabilities and malware for your cloud environment.
--- 
Cloud intrusion detection system (IDS) and intrusion prevention system (IPS)

## Overview
Intrusion detection is the process of monitoring the events occurring in your network and analyzing them for signs of possible incidents or threats to your security policies. 
Intrusion prevention is the process of performing intrusion detection and then stopping the detected incidents. These security measures are available as intrusion detection systems (IDS) and intrusion prevention systems (IPS), which become part of your network to detect and stop potential incidents.

Trend Micro Deep Security https://www.trendmicro.com/aws/ gives a comprehensive set of security controls delivered from a single agent. Manage your security from a single console, API, or orchestration tool across physical, virtual, cloud and container environments.

<a href="http://www.youtube.com/watch?feature=player_embedded&v=lpbg-xKrM3s
" target="_blank"><img src="https://img.youtube.com/vi/lpbg-xKrM3s/0.jpg" 
alt="Deep Security 12" width="480" height="320" border="10" /></a>


## Deep Security Components
Deep Security consists of the following set of components that work together to provide protecion:
- Deep Security Manager, the centralized web-based management console that administrators use to configure security policy and deploy protection to the enforcement components: the Deep Security Virtual Appliance and the Deep Security Agent.
- Deep Security Agent is a security agent deployed directly on a computer which provides application control, anti- malware, web reputation service, firewall, intrusion prevention, integrity monitoring, and log inspection protection to computers on which it is installed.
The Deep Security Agent contains a Relay module. A relay-enabled agent distributes software and security updates throughout your network of Deep Security components.
- Deep Security Notifier is a Windows System Tray application that communicates information on the local computer about security status and events, and, in the case of relay- enabled agents, also provides information about the security updates being distributed from the local machine.

## Pricing
You can subscribe to Deep Security as as Service and have it billed directly to your AWS account. Yoy can choose from either a Pay as you Go or an Annual subscription. A Pay as you Go subscription only bills you for the hours your computers are protected.

The following table depicts Deep Security cost based on instance types.  

| Hosts       | Costs         | 
| ------------- |:-------------:| 
| Any Micro, Small or Medium EC2 instance types      | $0.01 / host / hour | 
| Any Large EC2 instance types     | $0.03 / host / hour     |   
| Any Xlarge or larger EC2 instance types | $0.06 / host / hour      |  
| Other Cloud - 1 Core | $0.01 / host / hour      |
| Other Cloud - 2 Cores | $0.03 / host / hour      |
| Other Cloud - 4+ Cores | $0.06 / host / hour      |
| Data Center / Not Cloud | $0.06 / host / hour      |
| Amazon WorkSpaces | $0.01 / host / hour      |

  

## Subscribing to Deep Security as a Service
1.  Go to the Trend Micro Deep Security as a Service - Pay as you Go Marketplace page
[AWS Marketplace Trend Micro](https://aws.amazon.com/marketplace/pp/B01LXMNGHB?qid=1528381025605&sr=0-1&ref_=srh_res_product_title )

2. Click Continue > Subscribe > Set Up Your Account.

![]({{ site.baseurl }}/images/trendmicro-subscription.png)

3. You will be brought to the Deep Security as a Service account creation page. Fill out the form and click Sign Up.
4. You will receive an account confirmation email. Click the account activation link in the email.
5. Sign in to Deep Security as a Service using the company or account name and user name specified in the account confirmation email. Your subscription is now active. 


# Getting Started
Once the subscription is confirmed, you will need to add your AWS account or accounts to Deep Security Manager. 
1. In the Deep Security Manager, go to the Computers page and click Add > Add AWS Account.
2. Select Quick.
3. Click Next . A page appears that describes what happens during the setup process with a URL. The URL is valid for one hour.
4. Click Next.
5. If you have not already signed into your AWS account you are prompted to do so.
6. Click Next on the Select Template page to accept the defaults.
7. If your organization uses tags, you can add them on the Options page.
8. Click Next.
9. On the Review page, select the check box next to I acknowledge that this template might cause AWS CloudFormation to create IAM resources.
10. Click Create. When AWS CloudFormation finishes setting up a cross account role, the Deep Security Manager wizard displays a success message. You can close the screen before the success message is displayed. The account is added to Deep Security immediately after the cross account role is set up.
11. The Deep Security Manager Computers tab is now populated with the EC2 instances associated with your account.

## Port Numbers
You may need to enable the network access list and security groups for connecting to the Deep Security Manager.

Incoming 

| Transport Protocol      | Destination Port        | Service|     Source    |Purpose  |
| ------------- |:-------------:| :-------------:|:-------------:|:-------------:|
|   TCP   | 22 | SSH| Deployment tools such as Ansible| Remote agent installation | 
|   TCP   | 4118 | HTTPS| Deep Security as a Service (54.221.196.0/24)| Manager to Agent hearbeat. Send events and get configuration updates from the Manager. | 
|   TCP   | 3389 | RDP| Deployment tools such as Ansible| Remote agent installation (Windows) | 

Outgoing

| Transport Protocol      | Destination Port        | Service|     Destination   |Purpose  |
| ------------- |:-------------:| :-------------:|:-------------:|:-------------:|
|   UDP   | 53 | DNS| DNS server | Domain name resolution of Deep Security as a Service, NTP servers and others.
|   UDP  | 123 | NTP| NTP server| Accurate time for SSL or TLS connections, schedules and event logs | 
|   TCP   | 80/443 | HTTP/HTTPS| Deep Security as a Service| Administrative connections to the Deep Security as a Service GUI. Discovery and Agent activation. Deep Security Agent software installer downloads. | 
|   TCP   | 4122 | HTTPS| Relay | Agent-to-relay communications. |



## Agent Installation
Before proceeding with the Agents installation, configure the activation type which is the process of registering an agent with the Security Manager:
1. Log in to Deep Security Manager.
2. Click Administration at the top.
3. On the left, click System Settings.
4. In the main pane, make sure the Agents tab is selected.

![]({{ site.baseurl }}/images/agent-installation.png)

5. Select or deselect Allow Agent- Initiated Activation , noting that:
"Agent-initiated" activation does not require you to open up inbound ports to your Amazon EC2 instances or Amazon WorkSpaces, while manager- initiated activation does.
If "agent-initiated" activation is enabled, manager- initiated activation continues to work.
6. If you selected Allow Agent- Initiated Activation , also select Reactivate cloned Agents , and Enable Reactivate unknown Agents.
7. Click Save .    

You will need to make sure that the necessary ports are open to your Amazon EC2 instances. To open ports:
1. Log in to your Amazon Web Services Console.
2. Go to EC2 > Network & Security > Security Groups.
3. Select the security group that is associated with your EC2 instances, then select Actions > Edit outbound rules.
4. Open the necessary ports. Generally- speaking, agent-to-manager communication requires you to open the outbound TCP port (443 or 80, by default), while manager-to-agent communication requires you to open an inbound TCP port (4118). More specifically, If you enabled "Allow Agent-Initiated Activation", you'll need to open the outbound TCP port (443 or 80, by default) and If you disabled "Allow Agent-Initiated Activation" , you'll need to open the inbound TCP port of 4118.

You will need to install agents into your Amazon EC2 instances. There are a couple of options:
Option A: Use a deployment script to install, activate and assign a policy.
1. Log in to Deep Security Manager.
2. Click Support at the top and Deployment Scripts.

![]({{ site.baseurl }}/images/download-installation-script.png)

3. Select the system Platform: Linux, Windows or Solaris and the Security Policy to be applied.

4. Copy or Save to File the script provided.
5. SSH to the EC2 instance and run the script.

![]({{ site.baseurl }}/images/script-execution.png)

6. The EC2 instance will be displayed as Managed on the Security Manager Computers tab.

![]({{ site.baseurl }}/images/trendmicro-dashboard-instances.png)

Option B: Manually install and activate.
1. Log in to Deep Security Manager.
2. Click Support at the top and Download Agents.
3. Install the agent following the instruction for each platform 
[Trend Micro agent installation](https://help.deepsecurity.trendmicro.com/Get-Started/Install/install-dsa.html#Install3 )


Since the agent is installed, you can create policies to protect your computers and other resources. Check the documentation for further details https://help.deepsecurity.trendmicro.com/Policies/create-policy.html 
