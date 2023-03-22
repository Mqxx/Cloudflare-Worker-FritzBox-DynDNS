# Cloudflare-Worker-FritzBox-DynDNS
A simple Cloudflare Worker script to update your IP address using the build-in Fritz!Box DynDNS.

## Why this script?
Why I did even wrote this script in the first place? There is already a service from AVM (the company behind Fritz!Box) called MyFritz! which does exactly that... Well, since I manage my domains with Cloudflare, I wanted to avoid an extra service where I need an extra account again. And since I already manage my domains via Cloudflare, I chose a Cloudflare Worker.

## What even is a Cloudflare Worker?
A Cloudflare worker is basically just a script that is stored under a certain URL and is executed when this URL is called. Or rather, that's what we use it for. Cloudflare Worker can do so much more. For more info, check out [this overview](https://www.cloudflare.com/learning/serverless/glossary/serverless-and-cloudflare-workers/) to see what else you can use Cloudflare Workers for.

## Getting started
In this guide I will explain how you can set up your own DynDNS service using a free Cloudflare worker. I will go into all the steps that are necessary to create the worker and how to set the service up correctly.

##### Base URL:
`https://<Worker Subdomain>.<Worker name>.workers.dev/`

##### URL parameters:
- `token=<pass>`
- `zoneID=<username>`
- `ipv4address=<ipaddr>`
- `ipv4name=<domain>`
- `ipv4proxied=[true|false]`
- `ipv4ttl=1 (auto)`
- `ipv6address=<ip6addr>`
- `ipv6name=<domain>`
- `ipv6proxied=[true|false]`
- `ipv6ttl=1 (auto)`

# ⚠ Work in progress! ⚠
## I am currently working on a good documentation for the script!
