
| **Google Dorking Query**                                | Expected results                                                                                                                                   |
| ------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| inurl:"/wp-json/wp/v2/users"                            | Finds all publicly available WordPress API user directories.                                                                                       |
| intitle:"index.of" intext:"api.txt"                     | Finds publicly available API key files.                                                                                                            |
| inurl:"/api/v1" intext:"index of /"                     | Finds potentially interesting API directories.                                                                                                     |
| ext:php inurl:"api.php?action="                         | Finds all sites with a XenAPI SQL injection vulnerability. (This query was posted in 2016; four years later, there are currently 141,000 results.) |
| intitle:"index of" api_key OR "api key" OR apiKey -pool | This is one of my favorite queries. It lists potentially exposed API keys.                                                                         |

**Git Dorking**
Begin by searching GitHub for your target organization’s name paired with potentially sensitive types of information, such as “api key,” "api keys", "apikey", "authorization: Bearer", "access_token", "secret", or “token.” Then investigate the various GitHub repository tabs to discover API endpoints and potential weaknesses. Analyze the source code in the Code tab, find bugs in the Issues tab, and review proposed changes in the Pull requests tab.

Using the Code tab, you can review the code in its current form or use ctrl-F to search for terms that may interest you (such as API, key, and secret).

**TruffleHog**

TruffleHog is a great tool for automatically discovering exposed secrets. You can simply use the following Docker run to initiate a TruffleHog scan of your target's Github.

 $ sudo docker run -it -v "$PWD:/pwd" trufflesecurity/trufflehog:latest github --org=target-name

**Shodan**

Shodan is the go-to search engine for devices accessible from the internet. Shodan regularly scans the entire IPv4 address space for systems with open ports and makes their collected information public on https://shodan.io. Like with Google dorks, you can search Shodan casually by entering your target’s domain name or IP addresses.

| Shodan Queries                   | Purpose                                                                                                                                                                                                      |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| hostname:"targetname.com"        | Using hostname will perform a basic Shodan search for your target’s domain name. This should be combined with the following queries to get results specific to your target.                                  |
| "content-type: application/json" | APIs should have their content-type set to JSON or XML. This query will filter results that respond with JSON.                                                                                               |
| "content-type: application/xml"  | This query will filter results that respond with XML.                                                                                                                                                        |
| "200 OK"                         | You can add "200 OK" to your search queries to get results that have had successful requests. However, if an API does not accept the format of Shodan’s request, it will likely issue a 300 or 400 response. |
| "wp-json"                        | This will search for web applications using the WordPress API.                                                                                                                                               |

**The Wayback Machine**

The Wayback Machine is an archive of various web pages over time. This is great for passive API reconnaissance because this allows you to check out historical changes to your target.



