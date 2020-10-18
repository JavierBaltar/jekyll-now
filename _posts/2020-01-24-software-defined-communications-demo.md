---
title: Software Defined Communications Demo
date: "2020-01-24T23:46:37.121Z"
description: Hey your API is ringing!

--- 

The goal of this post is to share some ideas on open source software based versatile communications platform. Shipping SIP endpoints and APIs, are flexible and elastic solutions for providing programmable voice, video, messaging, conferencing, etc.


## FreeSWITCH Modules in Action

FreeSWITCH is a software defined telecom stack enabling the digital transformation from proprietary telecom switches to a versatile software implementation that runs on any commodity hardware.
There are several endpoint modules that come with FreeSWITCH, which implement several protocols such as SIP, H.323, WebRTC, and some others. In this case, the xml_curl_mod is used to fetch data from an external database and make SIP routing decisions. 

The following demo uses FreeSWITCH, NGINX, PHP and a MySQL backend database to manage SIP client registrations. The beauty of the setup is decoupling components: SIP server, webserver and database are deployed in different instances so flexibility and operability are enhanced. 

![]({{ site.baseurl }}/images/software-defined-comms-freeswitch-xml-curl-module.png "FreeSWITCH module demo")
[Watch the FreeSWITCH module demo](https://youtu.be/JecE0N0Mnr4)


## Scaling SIP Dinamically

Kamailio is an open source SIP server that is a suitable option for SIP load balancing across multiple softswitches such as Asterisk. The following demo briefly describes how to scale a setup of Docker containers running Asterisk. For service discovery purposes, Consul will provide to Kamailio the list of available backend Asterisk servers in order to route the SIP messages properly. Furthermore, you can scale up and down the number of Asterisk containers giving the foundation of a dynamic environment. 


![]({{ site.baseurl }}/images/software-defined-comms-scaling-sip-servers.png "Scaling SIP demo")
[Watch the scaling SIP containers demo](https://youtu.be/qo3LRqw5xG4)
 


## Documentation

Before concluding this post, I wish to suggest you to take a glance of the above components concepts and give them a try on your own setup:

- https://freeswitch.com
- https://www.kamailio.org/w/
- https://www.asterisk.org 
- https://www.consul.io 
