API2:2023 Broken authentication refers to any weakness within the API authentication process. Authentication is the process that is used to verify the identity of an API user, whether that user is a person, device, or system.

## [OWASP Attack Vector Description](https://owasp.org/API-Security/editions/2023/en/0xa2-broken-authentication/)

_The authentication mechanism is an easy target for attackers since it's exposed to everyone. Although more advanced technical skills may be required to exploit some authentication issues, exploitation tools are generally available._

## [OWASP Security Weakness Description](https://owasp.org/API-Security/editions/2023/en/0xa2-broken-authentication/)

_Software and security engineers’ misconceptions regarding authentication boundaries and inherent implementation complexity make authentication issues prevalent. Methodologies of detecting broken authentication are available and easy to create._

## [OWASP Impacts Description](https://owasp.org/API-Security/editions/2023/en/0xa2-broken-authentication/)

_Attackers can gain complete control of other users’ accounts in the system, read their personal data, and perform sensitive actions on their behalf. Systems are unlikely to be able to distinguish attackers’ actions from legitimate user ones._

## Summary

Broken Authentication continues to be a significant security issue due to poor password policies, weak authentication mechanisms, and misconfigurations

**Weak Password Policy**

- Allows users to create simple passwords
- Allows brute force attempts against user accounts
- Allows users to change their password without asking for password confirmation
- Allows users to change their account email without asking for password confirmation
- Discloses token or password in the URL
- GraphQL queries allow for many authentication attempts in a single request
- Lacking authentication for sensitive requests

**Credential Stuffing**

-  Allows users to brute force many username and password combinations

**Predictable Tokens**

- Using incremental or guessable token IDs

**Misconfigured JSON Web Tokens**

- API provider accepts unsigned JWT tokens
- API provider does not check JWT expiration
- API provider discloses sensitive information within the encoded JWT payload
- JWT is signed with a weak key

## [OWASP Preventative Measures](https://owasp.org/API-Security/editions/2023/en/0xa2-broken-authentication/)

- _Make sure you know all the possible flows to authenticate to the API (mobile/ web/deep links that implement one-click authentication/etc.). Ask your engineers what flows you missed._
- _Read about your authentication mechanisms. Make sure you understand what and how they are used. OAuth is not authentication, and neither are API keys._
- _Don't reinvent the wheel in authentication, token generation, or password storage. Use the standards._
- _Credential recovery/forgot password endpoints should be treated as login endpoints in terms of brute force, rate limiting, and lockout protections._
- _Require re-authentication for sensitive operations (e.g. changing the account owner email address/2FA phone number)._
- _Use the [OWASP Authentication Cheatsheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)._
- _Where possible, implement multi-factor authentication._
- _Implement anti-brute force mechanisms to mitigate credential stuffing, dictionary attacks, and brute force attacks on your authentication endpoints. This mechanism should be stricter than the regular rate limiting mechanisms on your APIs._
- _Implement [account lockout](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism\(OTG-AUTHN-003\))/captcha mechanisms to prevent brute force attacks against specific users. Implement weak-password checks._
- _API keys should not be used for user authentication. They should only be used for [API clients](https://cloud.google.com/endpoints/docs/openapi/when-why-api-key) authentication._


