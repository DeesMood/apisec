API5:2023 Broken Function Level Authorization (BFLA) is a vulnerability where API functions have insufficient access controls. Where BOLA is about access to data, BFLA is about altering or deleting data. In addition, a vulnerable API would allow an attacker to perform actions of other roles including administrative actions.

## [OWASP Attack Vector Description](https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/)

_Exploitation requires the attacker to send legitimate API calls to an API endpoint that they should not have access to as anonymous users or regular, non-privileged users. Exposed endpoints will be easily exploited._

## [OWASP Security Weakness Description](https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/)

_Authorization checks for a function or resource are usually managed via configuration or code level. Implementing proper checks can be a confusing task since modern applications can contain many types of roles, groups, and complex user hierarchies (e.g. sub-users, or users with more than one role). It's easier to discover these flaws in APIs since APIs are more structured, and accessing different functions is more predictable._

## [OWASP Impacts Description](https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/)

_Such flaws allow attackers to access unauthorized functionality. Administrative functions are key targets for this type of attack and may lead to data disclosure, data loss, or data corruption. Ultimately, it may lead to service disruption._

## Summary

![](attachments/Pasted%20image%2020250713190914.png)

This request results in a telling response, "This is an admin function. Try to access the admin API". This would lead an attacker to try using an admin path in the DELETE request (/identity/api/v2/**admin**/videos/758).

![](attachments/Pasted%20image%2020250713191004.png)

 The admin path and request method did not have authorization controls in place and was left vulnerable to a BFLA attack.

## [OWASP Preventative Measures](https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/)

_Your application should have a consistent and easy-to-analyze authorization module that is invoked from all your business functions. Frequently, such protection is provided by one or more components external to the application code._

- _The enforcement mechanism(s) should deny all access by default, requiring explicit grants to specific roles for access to every function._
- _Review your API endpoints against function level authorization flaws, while keeping in mind the business logic of the application and groups hierarchy._
- _Make sure that all of your administrative controllers inherit from an administrative abstract controller that implements authorization checks based on the user's group/role._
- _Make sure that administrative functions inside a regular controller implement authorization checks based on the user's group and role._

