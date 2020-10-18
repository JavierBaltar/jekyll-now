---
title: Docker security benchmark
date: "2018-11-05T22:40:32.169Z"
layout: post
description: Checking Docker security compliance. 
---
The Docker Bench for Security is a script that checks for dozens of common best-practices around deploying Docker containers in production.

## Introduction
The tests are all automated, and are inspired by the CIS Docker Benchmark.  

## Docker Security
Docker offers the Docker Bench for Security script (https://github.com/docker/docker-bench-security) , which checks a Docker configuration against the published hardening guide: CIS DOCKER 1.12.0 BENCHMARK. 
You can just download the script and run it straight from your host. Once you have run the script, you will be presented the output shown below

![]({{ site.baseurl }}/images/dockerSecurity.gif)


The script results in Info, Warning, and Pass notes for each of the recommendations which are grouped into 5 sections:
- Host Configuration
- Docker Daemon Configuration
- Docker Daemon Configuration Files
- Container Images and Build Files
- Container Runtime

Once the reported is generated, you can follow the mentioned benchmark document to remediate them.
