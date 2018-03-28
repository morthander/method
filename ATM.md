# Setup

+ Updated for OWASP.
+ Disable the built-in Burp Collaborator functionality
+ Purchase and install a Burp license if you don’t already have one
+ Install the SwitchySharp Extension in Google Chrome.
+ Configure Chrome’s SwitchySharp Extension to point to Burp on localhost:8080 for all protocols.
+ Install the Chrome BuiltWith Extension
+ Configure Burp to save state every 30 minutes to a local directory
+ Browse to the site through Chrome and enable auto-forwarding in Burp
+ Add your target site(s) to Burp's scope
+ Configure Burp to only passively scan in-scope content
+ Configure Burp to use a 100ms delay between scan requests (with randomness)
+ Install the .NET beautifier Burp Extension
+ Install the Additional Scanner Checks Burp Extension
+ Install the Authz Burp Extension
+ Install the Autorize Burp Extension
+ Install the .NET beautifier Burp Extension
+ Install the Hackvertor Burp Extension
+ Install the Identity Crisis Burp Extension
+ Install the Retire.js Burp Extension
+ Install the Site Map Fetcher Burp Extension
+ Install the SQLiPy Burp Extension

# Familiarity

+ Log in to the application and browse site through Burp for 3-5 minutes; navigate as much functionality as possible
+ Watch the Burp proxy traffic for every page you visit
+ Note the request format (URL structure)
+ Note the request format (POST structure)
+ Note the cookies being given by the server and sent by the client
+ Note the server platform
+ Note the programming language
+ Note any frameworks in use
+ Document all sensitive functionality
+ List abuse case possibilities for each sensitive function 
+ Brute force domains (where applicable)
+ Google for sites site:paypal.com
+ Comprehensive nmap scan nmap -sS -A -PN -p- --script=http-title (if in scope)
+ Discover platform using danielmiessler.com/services/checkyourstack

# Discovery

+ Find content using RobotsDisallowed (InterestingDirectories.txt)
+ Find content using RobotsDisallowed (Top10000-RobotsDisallowed.txt)
+ Find content using RobotsDisallowed (Top100000-RobotsDisallowed.txt)
+ Perform Shodan search of IP range (assuming it's in scope)
+ Disable form submissions in Burp's Spider settings
+ Spider the site using Burp's built-in spidering functionality
+ Run Recon-NG's discovery module [ discovery/info_disclosure/interesting_files ]
+ Run Recon-NG's Google Site Web module [ recon/domains-hosts/google_site_web ] 

# Automation

+ From the root of the domain in Burp's Target tab, start an Active Scan
+ If you have access to another scanner, configure it to the same scope and start it as well
+ If you have access to a third scanner, run it after at least one of the others has finished (be cautious not to cause session drama with manual or other automated testing)
+ Set Burp’s scanner settings to Thorough and Minimize False Negatives 

# OSINT

+ Search Punk spider for your domain to find any existing vulns. [ ReconNG's Punkspider module ]
+ Check Google's blacklist for your domain using Nmap. [ nmap --script http-google-malware $target ]
+ Check the domain for malware using Nmap [ nmap --script http-malware-host $target ]
+ Harvest Github repos from company using Recon-NG's github-miner module. [ /recon/companies-multi-github_miner.py ] 
+ Run Github Dorks against company repos that look interesting [ https://github.com/techgaun/github-dorks ]
+ Run Gitrob against company repos that look interesting [ https://github.com/michenriksen/gitrob ]
+ Run Bluto against the target [ https://github.com/RandomStorm/Bluto ]
+ Run Recon-NG's brute-hosts module on the domain [ recon/domains-hosts/brute_hosts ]
+ Run Recon-NG's [ recon/domains-hosts/brute_hosts ] 

# Authentication

+ User enumeration due to response differentiation; check timing differences between failed auth on real user vs. non-existent user
+ Password reset mechanism (cleartext password sent)
+ Password reset mechanism (guessable token)
+ Password reset mechanism (reusable token)
+ Password reset mechanism (reuse of token doesn’t generate an alert)
+ Password reset mechanism (token doesn’t expire)
+ Password reset mechanism (reusable token)
+ User enum via message on password reset
+ User enum via message on login
+ User enum via message on registration
+ Lack of account lockout
+ Password not required to perform sensitive account actions (email change)
+ Password not required to perform sensitive account actions (password change)
+ Password not required to perform sensitive account actions (shipping address change)
+ Password not required to perform sensitive account actions (mailing address change)
+ Email address can be changed for arbitrary users without authorization
+ Shipping address can be changed for arbitrary users without authorization
+ Sensitive data can be changed within an account without asking for the user’s password

# Session Management

+ Cookies not invalidated after logout
+ New cookie not given upon login
+ Reversible cookies (base64, etc.)
+ Multiple sessions allowed
+ Multiple sessions allowed without notification
+ Sensitive functions do not contain CSRF defenses
+ Determine if access to sensitive resources is being granted due to referer header
+ Use Burp’s Identity Crisis to determine if URLs respond differently based on User Agent strings
+ Sensitive pages or actions within one context can be forcefully browsed or executed to by other/lower users, e.g. viewing a profile, viewing a report, viewing messages, etc.
+ Make a list of all sensitive functions on the site, execute each one thoroughly, and analyze all proxy traffic during each of their workflows
+ Explore all numeric values that appear to be identifiers, e.g., UIDs, GUIDs, etc. Rotate those values both within the URL and in POST parameters and see if you can access other +contexts
+ Attempt to fall back to HTTP for sensitive functions

# Input Validation

+ Set Burp’s scanner settings to enable URL to Body and Body to URL
+ Perform a Burp active scan against the root of the site. If you receive authentication failures, lower your number of threads and/or increase the delay between requests
+ Intruder on key fields using SecLists XSS (look for field markers)
+ Intruder on key fields using SecLists SQLi (look for field markers)
+ Intruder on key fields using SecLists Polyglots (look for field markers)
+ For any SQLi discovered, exploit using standalone SQL Map instance
+ Test the target with XSS polyglots [ https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/Polyglots/XSS_Polyglots.txt ] 
+ Test the target with SQLi polyglots [ https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/Polyglots/SQLi_Polyglots.txt ] 
+ Test for RFI manually.
+ Test for LFI manually.

# Logic Testing

+ Test CAPTCHAs for replay attacks
+ Test CAPTCHAs for cleartext disclosure in source
+ Test CAPTCHAs for removal of CAPTCHA field in request
+ Test 2FA for removal of 2FA field in request
+ Test 2FA for skip ahead attack
+ Test e-Commerce sites for skip ahead attacks (skip payment and move to shipping)
+ Test e-Commerce sites for handling of negative numbers
+ Test sensitive functions for addition of repeated parameters that have different values

# Platform Testing

+ Full portscan of the system (assuming scope) [ nmap -sUS -p- -A -oA target $target ]
+ Within the proxy traffic, look for any signs of HTTP fallback
+ Check the server headers for security-oriented ones [ X-Frame-Options, Strict-Transport-Security, X-XSS-Protection, Content-Security-Policy, X-Content-Security-Policy,  etc. ] [ http://cyh.herokuapp.com/cyh ]
+ Scan for use of insecure HTTP methods
+ Nginx was detected: check Nginx vesion and look for vulnerabilities
+ Express was detected. Check for versions and any associated issues
+ Node was detected. Check for versions and any associated issues
+ Apache was detected: check Apache version and look for vulnerabilities
+ Scan the site with CMSMap
+ Check all sensitive transaction codes.