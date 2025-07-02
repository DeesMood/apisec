**Nmap**
$ nmap -sC -sV [target address or network range] -oA nameofoutput

The Nmap all-port scan will quickly check all 65,535 TCP ports for running services, application versions, and host operating system in use:

$ nmap -p- [target address] -oA allportscan

As soon as the general detection scan begins returning results, kick off the all-port scan. 

Once you discover a web server, you can perform HTTP enumeration using a Nmap NSE script (use -p to specify which ports you'd like to test).
$ nmap -sV --script=http-enum <target> -p 80,443,8000,8080

**Amass**

First, we can see which data sources are available for Amass (paid and free) by running:
$ amass enum -list

Next, we will need to create a config file to add our API keys to.
$ sudo curl https://raw.githubusercontent.com/OWASP/Amass/master/examples/config.ini >~/.config/amass/config.ini

You can repeat this process with many free accounts and API keys, then you will make OWASP Amass into a powerhouse for API reconnaissance.
$ amass enum -active -d target-name.com |grep api

Start by providing the command with target IP addresses
$ amass intel -addr [target IP addresses]

If this scan is successful, it will provide you with domain names. These domains can then be passed to intel with the whois option to perform a reverse Whois lookup:
$ amass intel -d [target domain] –whois

If you specify the -passive option, Amass will refrain from directly interacting with your target:
$ amass enum -passive -d [target domain]

The active enum scan will perform much of the same scan as the passive one, but it will add domain name resolution, attempt DNS zone transfers, and grab SSL certificate information:
$ amass enum -active -d [target domain]

$ amass enum -active -brute -w /usr/share/wordlists/API_superlist -d [target domain] -dir [directory name]  

**Directory Brute-force with Gobuster**

The following example uses an API-specific wordlist to find the directories on an IP address:

$ gobuster dir -u target-name.com:8000 -w /home/hapihacker/api/wordlists/common_apis_160

$ gobuster dir -u
://targetaddress/ -w /usr/share/wordlists/api_list/common_apis_160 -x 200,202,301 -b 302
Gobuster provides a quick way to enumerate active URLs find API paths.

**Kiterunner**

You can perform a quick scan of your target’s URL or IP address like this:
$ kr scan HTTP://127.0.0.1 -w ~/api/wordlists/data/kiterunner/routes-large.kite

If you want to use a text wordlist rather than a .kite file, use the brute option with the text file of your choice:
$ kr brute <target> -w ~/api/wordlists/data/automated/nameofwordlist.txt

You can use any of the following line-separated URI formats as input:
Test.com
Test2.com:443
http://test3.com
http://test4.com
http://test5.com:8888/api

In order to replay a request, copy the entire line of content into Kiterunner, paste it using the kb replay option, and include the wordlist you used:
$ kr kb replay "GET     414 [    183,    7,   8]
://192.168.50.35:8888/api/privatisations/count 0cf6841b1e7ac8badc6e237ab300a90ca873d571" -w
~/api/wordlists/data/kiterunner/routes-large.kite