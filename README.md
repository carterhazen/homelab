# Welcome!

Thank you for visiting my Homelab repo!

Here I share what's on my home server, what I'm working on, and I also have a learning section where I try to explain technical concepts.  Documentation allows me to track my progress, distill what I learn, and share with others. 
<br>

# Repo Structure


## /docker

Everything related to my docker environment.  Compose files, docker networking setup, it's all there.

## /kubernetes

Everything related to my kubernetes setup, which is currently RKE2 running on a single-node.

## /learning

Here I go into detail about various IT concepts.  IT can be difficult to learn and certain concepts can be hard to grasp.  This section allows me to solidify my understanding because the best way to truly understand something is to try and explain it.  I hope people find it helpful!

<br>

# The Infrastructure

It ain't pretty, but it's cost-effective and runs everything great.

## Media Server

This is a custom build for the purpose of running Jellyfin.  An Intel CPU with QuickSync and a dedicated Intel Spark GPU means it can transcode all the main codecs.

Because this was my first server it's currently the primary host for all my services running via Docker.

## RKE2 Host

This is the most recent addition.  I've been wanting to dive more into Kubernetes so I purchased a mini PC to run Rancher's RKE2 platform.

## NAS

A repurposed PC serves as my NAS.  The case can hold several 3.5" drives and I added dual 10Gb NICs.

# Infrastructure Security

IT security is always a layered approach.  Every level of the architecture needs to be looked at from a security perspective.  What I started with was planning the network isolation. 

## VLANs

- Storage
- Services
- Management
- IoT

This allows me to segregate traffic and apply rules as to how that traffic can interact with the rest of the network.  This has an obvious security benefit, but it also has network performance benefits.

For example, the media server and NAS have bonded 10Gb links.  Because those interfaces are on a single VLAN & subnet, that storage traffic never needs to traverse the firewall / router.  This keeps the high-speed storage traffic at Layer 2, saving the upstream routing device from having to deal with that traffic.   

## Port Forwarding & Reverse Proxy

There are lots of discussions online about the best way to deliver self-hosted services to users via the public internet.  Security decisions should always be weighed against the user experience, and this topic is no different.  A VPN is the most secure way to share your services, but for many non-tech-savvy people setting up a VPN is too much to ask.  I choose to expose my services in a way that makes it easy for the user, while also maintaining a healthy level of security.

I only port forward 443.  Cloudflare resolves all requests destined for my domain to my public IP, and internal DNS resolves it to a reverse proxy.  The proxy has several security add-ons for things like geo-blocking, etc. and is tied to CrowdSec which monitors the logs and implements bans on malicious requests.  My sercies offer trusted, auto-renewed certificates via Let's Encrypt and any request that does not match a valid subdomain is met with a 403 Forbidden response.
