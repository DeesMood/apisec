Business logic vulnerabilities are weaknesses within applications that are unique to the policies and features of a given API provider. The exploitation of business logic takes place when an attacker leverages misplaced trust or features of an application against the API.

## Attack Vector Description

Business logic vulnerabilities are unique to each application and exploit the normal intended functioning of an application's business processes. They often require specific knowledge of the application's functionality and the flow of transactions or data. Since these vulnerabilities are specific to the business logic of each application, there's no one-size-fits-all approach to identifying them.

## Security Weakness Description

Business logic vulnerabilities arise when the assumptions and constraints of a given business process aren't properly enforced in the application's control structures. This allows users to manipulate the application's functionality to achieve outcomes that are detrimental to the business. These weaknesses typically occur when developers fail to anticipate the various ways that an application's features can be misused or when they don't consider the wider context of the business rules. This is often due to a lack of comprehensive understanding of the application's business logic, a lack of input validation, or incomplete function-level authorization checks.

## Impacts Description

Business logic vulnerabilities can cause a variety of technical impacts, depending on the specific flaw and the systems involved. These impacts can range from unauthorized access to data or functionality to a total bypass of system controls.

## Summary

The Experian partner API leak, in early 2021, was a great example of an API trust failure. A certain Experian partner was authorized to use Experian’s API to perform credit checks, but the partner added the API’s credit check functionality to their web application and inadvertently exposed all partner-level requests to users. This request could be intercepted when using the partner’s web application, and if it included a name and address, the Experian API would respond with the individual’s credit score and credit risk factors. One of the leading causes of this business logic vulnerability was that Experian trusted the partner to not expose the API.

Examine an API's documentation for telltale signs of business logic vulnerabilities. Statements like the following should be indications of potential business logic flaws:

“Only use feature X to perform function Y.”

“Do not do X with endpoint Y.”

“Only admins should perform request X.”

These statements may indicate that the API provider is trusting that you won’t do any of the discouraged actions, as instructed. An attacker will easily disobey such requests to test for the presence of technical security controls.

As an example, consider a web application authentication portal that a user would normally employ to authenticate to their account. Say the web application issued the following API request:

POST /api/v1/login HTTP 1.1

Host: example.com

--snip--

UserId=hapihacker&password=arealpassword!&MFA=true

There is a chance that an attacker could bypass multifactor authentication by simply altering the parameter MFA to false.

## Preventative Measures

- Use a Threat Modeling Approach: Understand the business processes and workflows your API supports. Identifying the potential threats, weaknesses, and risks during the design phase can help to uncover and mitigate business logic vulnerabilities.
    
- Reduce or remove trust relationships with users, systems, or components. Business logic vulnerabilities can be used to exploit these trust relationships, leading to broader impacts.
    
- Regular training can help developers to understand and avoid business logic vulnerabilities. Training should cover secure coding practices, common vulnerabilities, and how to identify potential issues during the design and coding phases.
- Implement a bug bounty program, third-party penetration testing, or a responsible disclosure policy. This allows security researchers, who are a step removed from the design and delivery of an application, to disclose vulnerabilities they discover in APIs.