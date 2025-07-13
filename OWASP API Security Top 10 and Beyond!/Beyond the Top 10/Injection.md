They take place when an attacker is able to send commands that are executed by the systems that support the web application. The most common forms of injection attacks are SQL injection, Cross-site scripting (XSS), and operating system command injection.

## [OWASP 2019 Attack Vector Description](https://owasp.org/API-Security/editions/2019/en/0xa8-injection/)

"Attackers will feed the API with malicious data through whatever injection vectors are available (e.g., direct input, parameters, integrated services, etc.), expecting it to be sent to an interpreter."

## [OWASP 2019 Security Weakness Description](https://owasp.org/API-Security/editions/2019/en/0xa8-injection/)

"Injection flaws are very common and are often found in SQL, LDAP, or NoSQL queries, OS commands, XML parsers, and ORM. These flaws are easy to discover when reviewing the source code. Attackers can use scanners and fuzzers."

## [OWASP 2019 Impacts Description](https://owasp.org/API-Security/editions/2019/en/0xa8-injection/)

"Injection can lead to information disclosure and data loss. It may also lead to DoS, or complete host takeover."

## Summary

Verbose error messaging, HTTP response codes, and unexpected API behavior can all be clues to an attacker and will be an indication that they have discovered an injection flaw. Say, for example, an attacker were to send OR 1=0-- as an address in an account registration process. The API may pass that payload directly to the backend SQL database, where the OR 1=0 statement would fail (as 1 does not equal 0), causing some SQL error:

POST /api/v1/register HTTP 1.1

Host: example.com

--snip--

{

“Fname”: “hAPI”,

“Lname”: “Hacker”,

“Address”: “' OR 1=0--”,

}

An error in the backend database could show up as a response to the consumer. In this case, the attacker might receive a response like “Error: You have an error in your SQL syntax…”, but any response directly from databases or the supporting system will serve as a clear indicator that there is likely an injection vulnerability.

## [OWASP 2019 Preventative Measures](https://owasp.org/API-Security/editions/2019/en/0xa8-injection/)

#### Preventing injection requires keeping data separate from commands and queries.

- Perform data validation using a single, trustworthy, and actively maintained library.
- Validate, filter, and sanitize all client-provided data, or other data coming from integrated systems.
- Special characters should be escaped using the specific syntax for the target interpreter.
- Prefer a safe API that provides a parameterized interface.
- Always limit the number of returned records to prevent mass disclosure in case of injection.
- Validate incoming data using sufficient filters to only allow valid values for each input parameter.
- Define data types and strict patterns for all string parameters.