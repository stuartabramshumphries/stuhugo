---
title: "AWS Load balancing comparisons"
date: 2023-03-02T20:04:05+01:00
Description: ""
Tags: []
Categories: []
DisableComments: false
---

AWS Elastic Load Balancing Solutions

Overview
Elastic Load Balancing - commonly referred to as ELB helps with disrtibuting traffic accross multiple EC2 instances in several availability zones, this helps with:

    scaling
    performance
    reliability

Initially AWS provided the classic load balancer (which can confusingly sometimes be referred to as ELB also, due to historical reasons).

Usually requests are round-robin, however can make them sticky so that connection between a client and endpoint is persistent for defied periods of time.. at least with ALB.

Both ALB and NLB are capable of load balancing to multiple ports on the same instance.

Dont necessarily need to define individual instances at backend - can create 'target groups' - which represents a logical grouping of resources such as EC2 instances or microservices, containers etc.

Network Load Balancers expose a public static IP, whereas an Application or Classic Load Balancer exposes a static DNS

AWS also has wizards to assist in migrating from CLB to ALB and NLB.
The main ELB solutions are:
CLB - classic load balancer:
operates at Layer 4 and 7 of OSI model - connection oriented - requests forwarded without introspection - just forwarded to backend. For sticky sessions need to add to cookie.
Protocols supported are HTTP, HTTPS, TCP, SSL/TLS

ALB - application load balancer:
operates at Layer 7of OSI model - allows traffice distribution based on introspection - conenction is terminated on ALB and connection pool towards backend. Sticky sessions can be created by load balancer. Content based routing, very flexible, best for load balancing HTTP/HTTPS
Protocols supported are HTTP, HTTPS

NLB - network load balancer:
operates at layer 4 of OSI model. Bets for balancing TCP requests - originally couldnt do cross zone load balancing, but recently this feature was added. Can have IP addresses as target - static of elastic ip.

