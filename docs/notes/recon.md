
# Recon & OSINT Essentials — Practical Guide

> “Recon is the slow art of collecting silent truths.”  
> Practical reconnaissance for web, network, and people — ethical and methodical.

---

## 🧰 Tools & Goals (quick map)

- DNS & subdomains: `dig`, `host`, `amass`, `subfinder`, `crt.sh` (passive).  
- Port scanning: `nmap`, `masscan`.  
- Directories & endpoints: `gobuster`, `ffuf`, `dirsearch`.  
- Web fingerprinting: `whatweb`, `wappalyzer` (browser extension).  
- Service probes: `curl`, `nikto`, `sqlmap` (be careful).  
- OSINT/person search: `whois`, `theHarvester`, `hunter.io` (email), social media scrapers.

---

## 🧾 Passive Recon Examples

### Certificates & public records

- Check public certificates for subdomains (crt.sh or `openssl` if you have cert):

```bash
# look up cert transparency entries with crt.sh (web)
# CLI: use `dig` to check CNAME / A records
dig +short example.com
````

### WHOIS & DNS records

```bash
# whois
whois example.com

# dig for DNS records
dig +nocmd example.com any +noall +answer
dig sub.example.com A
dig example.com MX
```

### Search engines & GitHub leaks

- Google dorks: `site:example.com "index of"`, `site:github.com example.com "API_KEY"`.
- GitHub search for potential secrets — use responsibly.

---

## 🛠️ Active Recon Examples

### Subdomain discovery

```bash
# passive lists
subfinder -d example.com -o subfinder.txt

# brute + wordlist
amass enum -d example.com -passive -o amass_passive.txt
amass enum -d example.com -o amass_all.txt

# resolve
cat subfinder.txt amass_all.txt | sort -u > allsubs.txt
cat allsubs.txt | httprobe -c 50 > live.txt
```

### Port & service scanning

```bash
# fast top-ports scan with nmap
nmap -sS -T4 --top-ports 100 -oA quick_scan example.com

# full TCP scan (slower)
nmap -sC -sV -p- -T3 -oA full_scan example.com
```

### Directory & content discovery

```bash
# ffuf example: fuzz for directories
ffuf -u https://sub.example.com/FUZZ -w /usr/share/wordlists/dirb/common.txt -fs 0

# gobuster
gobuster dir -u https://sub.example.com -w /usr/share/wordlists/dirb/common.txt -o gobuster.txt
```
