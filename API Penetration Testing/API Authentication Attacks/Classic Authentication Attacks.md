The classic authentication attacks in this section include password brute-force attacks with base64 encoding, password reset brute-force, and password spraying. 

## Password Brute-Force Attacks
Brute-forcing an API’s authentication is not very different from any other brute-force attack, except you’ll send the request to an API endpoint, the payload will often be in JSON, and the authentication values may require base64 encoding.

For more information about creating targeted password lists, check out the Mentalist app (https://github.com/sc0tfree/mentalist) or the Common User Passwords Profiler (https://github.com/Mebus/cupp).

You can unzip rockyou.txt using the following command.
`$gzip -d /usr/share/wordlists/rockyou.txt.gz`

To start out with WFuzz, access the help menu and read the options to get a better idea of what can be done with this tool.
`$wfuzz --help`
Important items to note for API testing include the headers option (-H), hide responses (--hc, --hl, --hw, --hh), and POST body requests (-d). All of these will be useful when fuzzing APIs. We will need to specify the content-type headers for APIs which will be 'Content-Type: application/json' for crAPI. The POST body for the crAPI login is '{"email":"a@email.com","password":"FUZZ"}'. Notice that FUZZ is the WFuzz attack position.

`$ wfuzz -d '{"email":"a@email.com","password":"FUZZ"}' -H 'Content-Type: application/json' -z file,/usr/share/wordlists/rockyou.txt -u http://127.0.0.1:8888/identity/api/auth/login --hc 405`

## Password Spraying
There are way too many unlikely passwords in such a file to have any success. Instead, craft a short list of likely passwords, taking into account the constraints of the API provider’s password policy, which you can discover during reconnaissance.

The first type includes obvious passwords like QWER!@#$, Password1!, and the formula Season+Year+Symbol (such as Winter2025!, Spring2025?, Fall2025!, and Autumn2025?).

Here is a short password-spraying list I might generate if I were attacking an endpoint for Twitter employees: Summer2025! Spring2025! QWER!@#$ March212006! July152006! Twitter@2025 JPD1976! Musk@2025

You can generate a file from an API response using postman "save response to file" feature

Next, you can use the power of grep and regular expressions to pull emails from the saved JSON response.

`$grep -oe "[a-zA-Z0-9._]\+@[a-zA-Z]\+.[a-zA-Z]\+" response.json`

You can see a successful password-spraying attempt that has two anomalous features: a status code of 200 and a response length of 698.

## Note on Base64 Encoding
Some APIs will base64-encode authentication payloads sent in an API request.

Once your payload processing rule is in place, you can perform an attack as normal. When you are reviewing anomalous results, you can simply use CTRL+Shift+B or use the convert selection option by highlighting and right-clicking the payload. 

