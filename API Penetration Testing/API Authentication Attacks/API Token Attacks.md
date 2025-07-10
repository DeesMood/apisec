## Token Analysis
When implemented correctly, tokens can be an excellent tool that can be used to authenticate and authorize users. However, if anything goes wrong when generating, processing, or handling tokens, they can become our keys to the kingdom.

To see what an analysis of a poor token generation process looks like perform an analysis of the "bad tokens" located on the Hacking APIs Github repository (https://raw.githubusercontent.com/hAPI-hacker/Hacking-APIs/main/bad_tokens). This time around we will use the Manual load option, to provide our own set of bad tokens. 

## JWT Attacks
JWT.io is a free web JWT debugger that you can use to check out these tokens.

You can spot a JWT because they consist of three periods and begin with "ey". They begin with "ey" because that is what happens when you base64 encode a curly bracket followed by a quote, which is the way that a decoded JWT always begins.

Here you can see the example JWT:
`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6ImhhcGloYWNrZXIiLCJpYXQiOjE1MTYyMzkwMjJ9.U1Agy_OvIULTQwBrYx0dlWVzcBqcI90no2pAcoy4-uo`

We can take this JWT and decode the individual parts. The header is the first segment. You can simply echo that part of the token and base64 decode it to see what the header contains:
**command:**
`$ echo eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9|base64 -d`
**output:**
`{"alg":"HS256","typ":"JWT"}`

The algorithm (alg) used for this token is HS256 and the token type (typ) is JWT. Next, we can do the same with the payload:
**command:**
`$ echo eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6ImhhcGloYWNrZXIiLCJpYXQiOjE1MTYyMzkwMjJ9|base64 -d`
**output:**
`{"sub":"1234567890","name":"hapihacker","iat":1516239022}`

Now, this token only contains the subject (sub), the username (name), and the time that the token was issued at (iat). More information could be contained in the body, in fact, whatever the provider would like to add here they can. A JWT body can be a great source of information disclosure. Consider a JWT that contains the username, email, password, and role all hiding behind one base64 decode. If we run the same command for the JWT signature we will see the following:
**command:**
`$ echo U1Agy_OvIULTQwBrYx0dlWVzcBqcI90no2pAcoy4-uo|base64 -d`
**output:**
`SP base64: invalid input`

Finally, the signature is the output of HMAC used for token validation and generated with the algorithm specified in the header.

To create the signature, the API base64-encodes the header and payload and then applies the hashing algorithm and a secret. The secret can be in the form of a password or a secret string, such as a 256-bit key.
`HMACSHA256( base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`
If you would like to learn more about JWTs, check out https://jwt.io/introduction. 

## Automating JWT attacks with JWT_Tool
Some of the options to note include:

    -h           to show more verbose help options
    -t            to specify the target URL
    -M          to specify the scan mode
        pb         to perform a playbook audit (default tests)
        at          to perform all tests
    -rc          to add request cookies
    -rh          to add request headers
    -rc          to add request cookies
    -pd         to add POST data
Also, make sure to check out the Wiki for more information https://github.com/ticarpi/jwt_tool/wiki .

**Playbook scan**
`$ jwt_tool -t http://target-name.com/ -rh "Authorization: Bearer JWT_Token" -M pb`

## The None Attack
If you ever come across a JWT using "none" as its algorithm, you’ve found an easy win. After decoding the token, you should be able to clearly see the header, payload, and signature. From here, you can alter the information contained in the payload to be whatever you’d like. For example, you could change the username to something likely used by the provider’s admin account (like root, admin, administrator, test, or adm), as shown here: { "username": "root", "iat": 1516239022 } Once you’ve edited the payload, use Burp Suite’s Decoder to encode the payload with base64; then insert it into the JWT. Importantly, since the algorithm is set to "none", any signature that was present can be removed. In other words, you can remove everything following the third period in the JWT. Send the JWT to the provider in a request and check whether you’ve gained unauthorized access to the API.

## The Algorithm Switch Attack
One of the first things you should attempt is sending a JWT without including the signature. This can be done by erasing the signature altogether and leaving the last period in place, like this:
`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJoYWNrYXBpcy5pbyIsImV4cCI6IDE1ODM2Mzc0ODgsInVzZ XJuYW1lIjoiU2N1dHRsZXBoMXNoIiwic3VwZXJhZG1pbiI6dHJ1ZX0`

If this isn’t successful, attempt to alter the algorithm header field to "none". Decode the JWT, update the "alg" value to "none", base64-encode the header, and send it to the provider. To simplify this process, you can also use the jwt_tool to quickly create a token with the algorithm switched to none.
`$ jwt_tool eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ1c2VyYWFhQGVtYWlsLmNvbSIsImlhdCI6MTY1ODg1NTc0MCwiZXhwIjoxNjU4OTQyMTQwfQ._EcnSozcUnL5y9SFOgOVBMabx_UAr6Kg0Zym-LH_zyjReHrxU_ASrrR6OysLa6k7wpoBxN9vauhkYNHepOcrlA -X a`

A more likely scenario than the provider accepting no algorithm is that they accept multiple algorithms. For example, if the provider uses RS256 but doesn’t limit the acceptable algorithm values, we could alter the algorithm to HS256. This is useful, as RS256 is an asymmetric encryption scheme, meaning we need both the provider’s private key and a public key in order to accurately hash the JWT signature. Meanwhile, HS256 is symmetric encryption, so only one key is used for both the signature and verification of the token. If you can discover and obtain the provider’s RS256 public key then switch the algorithm from RS256 to HS256, there is a chance you may be able to leverage the RS256 public key as the HS256 key.  It uses the format jwt_tool TOKEN -X k -pk public-key.pem, as shown below. You will need to save the captured public key as a file on your attacking machine (You can simulate this attack by taking any public key and saving it as public-key-pem).

`$ jwt_tool eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ1c2VyYWFhQGVtYWlsLmNvbSIsImlhdCI6MTY1ODg1NTc0MCwiZXhwIjoxNjU4OTQyMTQwfQ._EcnSozcUnL5y9SFOgOVBMabx_UAr6Kg0Zym-LH_zyjReHrxU_ASrrR6OysLa6k7wpoBxN9vauhkYNHepOcrlA -X k -pk public-key-pem`

## JWT Crack Attack

Crunch, a password-generating tool, to create a list of all possible character combinations to use against crAPI.

`$crunch 5 5 -o crAPIpw.txt`

To perform a JWT Crack attack using JWT_Tool, use the following command: 
`jwt_tool TOKEN -C -d /wordlist.txt`

The -C option indicates that you’ll be conducting a hash crack attack, and the -d option specifies the dictionary or wordlist you’ll be using against the hash.



