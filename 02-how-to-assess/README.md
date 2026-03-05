# RECON, aka info gathering
🎯 TLDR: SCOPE -> PASSIVE RECON -> ACTIVE RECON

## Table of Contents
- [01. 'scope' the target](#01-scope-the-target)
- [02. passive info gathering](#02-passive-info-gathering) 
  - [First things to check](#first-things-to-check)
  - [1. DNS records](#1-dns-records)
  - [2. Tech stack](#2-tech-stack)
  - [3. Subdomains](#3-subdomains)
  - [4. Hidden directories & files](#4-hidden-directories--files)
  - [5. Email Harvest](#5-email-harvest)
  - [6. Download website files](#6-download-website-files)
- [03. active info gathering](#03-active-info-gathering)
- [04. questions & additional notes](#04-questions--additional-notes)

## 01. 'scope' the target
What's 'target scoping'? 
- Define systems/networks you're allowed to test 

Why do we scope? 
- focus our efforts on relevant stuff & be efficient
- stay within legal bounds



What could 'targets' be?
1. ip 
    - either specific devices or ip ranges 

2. application
    - entire app
    - specific API endpoint
    - login portal 

3. domain
    - e.g. example.com 
    - and any specified subdomains too: e.g. subdomain.example.com

**IMPORTANT**: Must not interact with external, 3rd party services!


## 02. passive info gathering
Keywords: Fingerprinting

**What**: get info from public sources without direct interaction with target
**Examples**: 
    - OSINT (open source intel), like gathering info from facebook or google 
    - DNS records (getting server IPs)

### First things to check
- robots.txt file at the root of website (e.g. https://www.canva.com/robots.txt)
    - What: For web crawlers - let them now which directories/routes are indexable
        - Btw, web crawlers are softwares that discover new HTML content on the web, mapping them to search queries. Used by search engines. 
        - the "Disallow:" fields may reveal hidden routes!
            - that's why we must also protect sensitive routes using server authentication too

- ip address of domain
    - CLI: `host` tells us a domain's ipv4, v6 address, and smtp mail server domain names
        - e.g. host google.com
    - Other CLIs:
        - `dig`
        - `nslookup`

- sitemap
    - like their xml sitemap at the root
    - usually made for SEO optimisation 
    - not always present


Next is a list of things to look at:
### 1. DNS records
- https://dnsdumpster.com/
    - cool map of servers & their IPs

What if website is behind a firewall or proxy server (like cloudflare)?
- CLI Tool: `wafw00f <website>`
- What is WAF? 
    - Web application firewall. Is a reverse proxy (direction: client to server).
    - Safeguards incoming requests, checks that they are safe, if so, forward request to server which is hidden. If WAF is used, the website domain resolves to the WAF's IP instead. 
- Why would we care? 
    - If a website is behind a web-app firewall (WAF), the origin server's IP will generally be hidden.



### 2. Tech stack
- Netcraft
- BuiltWith website profiler (browser extension)
- https://www.wappalyzer.com/
- CLI: `whatweb <website>` tells you what http server (e.g. cloudflare), version of php, if it's running on WordPress, HTTP headers 

### 3. Subdomains
- **Google Dorking**
    - `site:*.domain.com`
    - `intitle:<keyword>`
    - `inurl:<keyword>`
    - `filetype:<type>`
  
- **dirb**
    - Command: `dirb <domain> common.txt`
    - Purpose: Brute-force with a word list
  
- **sublist3r**

### 4. Hidden directories & files  
- also `dirb`

### 5. Email Harvest 
    - If the domain uses MX (mail) servers 
    - CLI: theHarvester

### 6. Download website files 
    - called 'mirroring'
    - publicly available stuff only: HTML, CSS, JS, images...
    - https://www.httrack.com/ (or CLI too)




# 03. active info gathering 
- engage with the target
1. Port scanning - scan the open ports of a server 




# 04: questions & additional notes
- Why would an attacker care about info on dns server / proxy servers used? what difference does it make if the website is using cloudflare vs google? 
    - Ans: To know what defence is in place. If it's pure DNS without proxy, then attackers know the origin server's IP. On cloudflare, if a website is hosted with DNS-only without using reverse proxy, it's 'grey-clouded', compared to 'orange-clouded' meaning proxied. 

- Wordpress websites are in PHP
    - Common misconfigs? 