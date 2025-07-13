API5:2023 Broken Function Level Authorization (BFLA) is a vulnerability where API functions have insufficient access controls. Where BOLA is about access to data, BFLA is about altering or deleting data. In addition, a vulnerable API would allow an attacker to perform actions of other roles including administrative actions.

## [OWASP Attack Vector Description](https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/)

_Exploitation requires the attacker to send legitimate API calls to an API endpoint that they should not have access to as anonymous users or regular, non-privileged users. Exposed endpoints will be easily exploited._

## [OWASP Security Weakness Description](https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/)

_Authorization checks for a function or resource are usually managed via configuration or code level. Implementing proper checks can be a confusing task since modern applications can contain many types of roles, groups, and complex user hierarchies (e.g. sub-users, or users with more than one role). It's easier to discover these flaws in APIs since APIs are more structured, and accessing different functions is more predictable._

## [OWASP Impacts Description](https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/)

_Such flaws allow attackers to access unauthorized functionality. Administrative functions are key targets for this type of attack and may lead to data disclosure, data loss, or data corruption. Ultimately, it may lead to service disruption._

## Summary

![](attachments/Pasted%20image%2020250713190914.png)

