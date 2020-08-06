<div align="center">
  <h1>Pen Test Toolkit</h1>
  <p></p>
  <sup>
    <a href="https://hub.docker.com/r/mcnamee/pentest-toolkit">
      <img src="https://img.shields.io/docker/v/mcnamee/pentest-toolkit?style=flat-square" alt="version" />
    </a>
    <a href="https://github.com/mcnamee/pentest-toolkit/actions">
      <img alt="Build Status" src="https://img.shields.io/endpoint.svg?url=https%3A%2F%2Factions-badge.atrox.dev%2Fmcnamee%2Fpentest-toolkit%2Fbadge&label=build&logo=none" />
    </a>
    <a href="/LICENSE">
      <img src="https://img.shields.io/github/license/mcnamee/pentest-toolkit?style=flat-square" alt="license" />
    </a>
  </sup>
  <br />
  <p align="center">
    <a href="#intro"><b>What is this?</b></a>
    &nbsp;&nbsp;&mdash;&nbsp;&nbsp;
    <a href="#instructions"><b>Instructions</b></a>
    &nbsp;&nbsp;&mdash;&nbsp;&nbsp;
    <a href="#tools"><b>Tools</b></a>
  </p>
  <br />
</div>

## What is this?

A collection of pen testing in a single Docker container. Simply run the image and start using the tools.

__Why?__

I got sick of waiting for VitualBox to boot, Kali to boot, then dealing with the slugish-ness of operating in a VM. I still use Kali for certain tasks. But for a quick nmap scan (for example), this image is a lot quicker.

## Instructions

### Run from DockerHub

```bash
docker run -it mcnamee/pentest-toolkit

# - OR - to map a local drive (line 2) + allow OpenVPN (line 3)
docker run -it \
  -v ~/Projects:/root/projects \
  --cap-add=ALL --net=host --device=/dev/net/tun \
  mcnamee/pentest-toolkit
```

[![asciicast](https://asciinema.org/a/343944.svg)](https://asciinema.org/a/343944)

### Build

```bash
# 1. Clone the repo
git clone https://github.com/mcnamee/pentest-toolkit.git && cd pentest-toolkit

# 2. Build the image
docker build . -t mcnamee/pentest-toolkit
```

## Tools

### Recon

| Tool | Description & Example |
| --- | --- |
| [amass](https://github.com/OWASP/Amass) | _Network mapping of attack surfaces and external asset discovery using open source information gathering and active reconnaissance techniques._ <br>`amass enum -v -src -ip -brute -min-for-recursive 2 -d kali.org` |
| [brutespray](https://github.com/x90skysn3k/brutespray) | _Service scanner by bruteforcing._ <br>`brutespray --file nmap.gnmap` |
| [cloudfail](https://github.com/m0rtem/CloudFail) | _CloudFail is a tool to find origin servers of websites protected by CloudFlare._ <br> `cloudfail --target resound.ly` |
| [cloudflair](https://github.com/christophetd/CloudFlair) | _CloudFlair is a tool to find origin servers of websites protected by CloudFlare who are publicly exposed and don't restrict network access to the CloudFlare IP ranges as they should._ <br> `export CENSYS_API_ID=... && export CENSYS_API_SECRET=...` <br> `cloudflair resound.ly` |
| [dirb](https://tools.kali.org/web-applications/dirb) | _Looks for existing (and/or hidden) Web Objects, by launching a dictionary based attack against a web server and analyzing the response._ <br> `dirb https://kali.org $WORDLISTS/seclists/Discovery/Web-Content/CommonBackdoors-PHP.fuzz.txt` |
| [dnmasscan](https://github.com/rastating/dnmasscan) | _dnmasscan is a bash script to automate resolving a file of domain names and subsequentlly scanning them using masscan._ <br> `dnmasscan listofdomains.txt dns.log -p80,443 - oG masscan.log` |
| [dnsprobe](https://github.com/projectdiscovery/dnsprobe) | _Allows you to perform multiple dns queries of your choice with a list of user supplied resolvers._ <br> `cat domains.txt | dnsprobe` |
| [ffuf](https://github.com/ffuf/ffuf) | _A fast web fuzzer._ <br> - `ffuf -w /path/to/postdata.txt -X POST -d "username=admin\&password=FUZZ" -u https://target/login.php -fc 401` |
| [gobuster](https://github.com/OJ/gobuster) | _Gobuster is a tool used to brute-force: URIs (directories and files), DNS subdomains, Virtual Hosts._ <br> - `gobuster dns -d kali.org -w $WORDLISTS/seclists/Discovery/DNS/fierce-hostlist.txt` <br>- `gobuster dir -u https://www.kali.org  -w $WORDLISTS/dirb/common.txt` <br>- `gobuster vhost -u kali.org  -w $WORDLISTS/seclists/Discovery/DNS/fierce-hostlist.txt` |
| [httpx](https://github.com/projectdiscovery/httpx) | _Take a list of domains and probe for working http and https servers._ <br> `cat domains.txt | httpx` |
| [linkfinder](https://github.com/GerbenJavado/LinkFinder) | _Discover endpoints and their parameters in JavaScript files._ <br> `linkfinder -i https://example.com -d -o cli` |
| [masscan](https://github.com/robertdavidgraham/masscan) | _An Internet-scale port scanner._ <br> `masscan -p1-65535 -iL listofips.txt --max-rate 1800 -oG masscan.log` |
| [meg](https://github.com/robertdavidgraham/masscan) | _A tool for fetching lots of URLs but still being 'nice' to servers._ <br> `meg paths.txt hosts.txt` |
| [nikto](https://tools.kali.org/information-gathering/nikto) | _Web server scanner which performs comprehensive tests against web servers for multiple items (dangerous files, outdated dependencies...)._ <br> `nikto -host=https://kali.org` |
| [nmap](https://nmap.org/) | _A utility for network discovery and security auditing_. <br> `nmap -sV 192.168.0.1` |
| [nuclei](https://github.com/projectdiscovery/nuclei) | _Nuclei is a fast tool for configurable targeted scanning based on templates offering massive extensibility and ease of use._. <br> `nuclei -l urls.txt -t $ADDONS/nuclei/ADDONS/nuclei/technologies/ -o ~/projects/results.txt` |
| [recon-ng](https://github.com/lanmaster53/recon-ng) | _Web-based open source reconnaissance framework._ <br> `recon-ng` |
| [sherlock](https://github.com/sherlock-project/sherlock) | _Hunt down social media accounts by username across social networks._ <br> `sherlock user1 user2 user3` |
| [subfinder](https://github.com/projectdiscovery/subfinder) | _Subdomain discovery tool to find valid subdomains for websites by using passive online sources._ <br> `subfinder -d kali.org -v` |
| [subjack](https://github.com/haccer/subjack) | _A Subdomain Takeover tool._ <br> `SJFP=$(find "${GOPATH}/pkg" -name fingerprints.json)` <br> `subjack -w subdomains.txt -t 100 -timeout 30 -o results.txt -a -c $SJFP` |
| [sublist3r](https://github.com/aboul3la/Sublist3r) | _Enumerates subdomains using many search engines such as Google, Yahoo, Bing, Baidu and more._ <br> `sublist3r -d kali.org` |
| [theharvester](https://tools.kali.org/information-gathering/theharvester) | _Gather emails, subdomains, hosts, employee names, open ports and banners from different public sources like search engines, PGP key servers and SHODAN computer database._ <br> <code>theharvester -d kali.org -b "bing, certspotter, dnsdumpster, dogpile, duckduckgo, google, hunter, linkedin, linkedin_links, twitter, yahoo"</code> |
| [wafw00f](https://github.com/enablesecurity/wafw00f) | _Web Application Firewall Fingerprinting Tool._ <br> `wafw00f resound.ly` |
| [whatweb](https://github.com/urbanadventurer/WhatWeb) | _Scans websites and highlights the CMS used, JavaScript libraries, web servers, version numbers, email addresses, account IDs, web framework modules, SQL errors, and more._ <br> `whatweb kali.org` |
| [wpscan](https://github.com/wpscanteam/wpscan) | _WordPress Security Scanner._ <br> `wpscan --url kali.org` |
| [xsstrike](https://github.com/s0md3v/XSStrike) | _Advanced XSS Detection Suite._ <br> `xsstrike --help` |

### Exploitation

| Tool | Description & Example |
| --- | --- |
| [commix](https://github.com/commixproject/commix) | _Command injection exploiter - used to test web applications with the view to find bugs, errors or vulnerabilities related to command injection attacks._ <br> `commix --url="http://192.168.0.23/commix-testbed/scenarios/referer/referer(classic).php" --level=3` |
| [john](https://github.com/magnumripper/JohnTheRipper) | _John the Ripper is a fast password cracker._ <br> `zip2john filename.zip > hash.txt`<br> `john hash.txt` |
| [metasploit](https://tools.kali.org/exploitation-tools/metasploit-framework) | _A penetration testing platform that enables you to find, exploit, and validate vulnerabilities.._ <br> `msfconsole` |
| [hydra](https://tools.kali.org/password-attacks/hydra) | <code>hydra -f -l email@admin.com -P $WORDLISTS/seclists/Passwords/darkweb2017-top1000.txt website.com http-post-form "/login:user=^USER^&pass=^PASS^:Failed"</code> |
| [netcat](http://netcat.sourceforge.net/) | _A networking utility which reads and writes data across network connections, using the TCP/IP protocol._ <br> `nc -nvlp 1234` |
| [pwncat](https://pwncat.org/) | _a sophisticated bind and reverse shell handler with many features as well as a drop-in replacement or compatible complement to netcat._ <br> `pwncat -nvlp 1234` |
| [searchsploit](https://tools.kali.org/exploitation-tools/exploitdb) | _Searchable archive from The Exploit Database._ <br> `searchsploit oracle windows remote` |
| [setoolkit](https://www.trustedsec.com/tools/the-social-engineer-toolkit-set/) | _Social Engineering Toolkit._ <br> `setoolkit` |
| [sqlmap](http://sqlmap.org/) | _Automates the process of detecting and exploiting SQL injection flaws and taking over of database servers_ <br> `sqlmap -u https://example.com --forms --crawl=10 --level=5 --risk=3` |

### Other

| Tool | Description |
| --- | --- |
| FTP | _Connect to an FTP server._ <br> `ftp ftp.google.com` |
| [Interlace](https://github.com/codingo/Interlace) | _Easily turn single threaded command line applications into a fast, multi-threaded application._ <br> `interlace -tL targets.txt -threads 5 -c "gobuster dns -d _target_ -w wordlist.txt --noprogress --quiet >> _target_.txt" -v` |
| [Oh My Zsh](https://ohmyz.sh/) | _Zsh is a framework for managing your zsh configuration, bundled with thousands of helpful functions, helpers, plugins, themes._ |
| [OpenVPN](https://openvpn.net/) | _Connect to a VPN._ <br> Add `--cap-add=NET_ADMIN --device=/dev/net/tun` to the `docker run` to use OpenVPN |
| [tmux](https://github.com/tmux/tmux/wiki) | _tmux is a terminal multiplexer. It lets you switch easily between several programs in one terminal, detach them (they keep running in the background) and reattach them to a different terminal._ <br>`CNTR+b c` Create window <br>`CNTR+b n` Switch to next window |
| [unfurl](https://github.com/tomnomnom/unfurl) | _Pull out bits of URLs provided on stdin._ <br> `cat urls.txt | unfurl --unique domains` |
| [zsh](https://www.zsh.org/) | _Zsh is an extended Bourne shell with many improvements, including some features of Bash, ksh, and tcsh._ |

<!-- END -->

## Wordlists

- Amass
- Dirb
- Kali's rockyou.txt
- Metasploit
- The Harvester
- [SecLists](https://github.com/danielmiessler/SecLists)
